---
title: Usar um provedor de Autenticação Multifator alternativo por meio de uma API para ativar o PAM ou no cenário de SSPR | Microsoft Docs
description: Configure a API de MFA do Azure personalizada como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management e usarem o Autoatendimento de Redefinição de Senha.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 9ce531fb3f6f9c831ecdb716f006f947611871e6
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256590"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Usar um provedor de Autenticação Multifator personalizado por meio de uma API durante a ativação de função do PAM ou no SSPR

Os clientes do Azure AD Premium ou do MFA do Azure podem integrar o MFA do Azure em dois cenários de MIM: ativação de função de PAM (Privileged Access Management) e SSPR (Autoatendimento de Redefinição de Senha).

Clientes do MIM têm duas opções adicionais:

 - Usar um provedor de entrega de senha avulsa personalizado, que é aplicável apenas no cenário de SSPR do MIM e documentado no guia para [Configurar o 	Autoatendimento de Redefinição de Senha com o portão de SMS com OTP](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10))
 - Use um provedor de telefonia de autenticação multifator personalizado. Isso é aplicável no cenário de SSPR do MIM e no cenário do PAM, ambos descritos neste artigo

Este artigo descreve como usar o MIM com um provedor de autenticação multifator personalizado, por meio de uma API e um SDK de integração desenvolvido pelo cliente.  

## <a name="prerequisites"></a>Pré-requisitos

Para usar uma API do provedor de Autenticação Multifator personalizado com o MIM, você precisa:

- Números de telefone para todos os usuários candidatos
- Hotfix do MIM 4.5.202.0 ou posterior: confira o [histórico de versões](reference/version-history.md) de comunicados
- Serviço MIM configurado para SSPR ou PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Abordagem usando código personalizado de autenticação multifator

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Etapa 1: verifique se o Serviço MIM está na versão 4.5.202.0 ou posterior

Baixe e instale o hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) do MIM ou uma versão posterior.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Etapa 2: criar uma DLL que implemente a interface IPhoneServiceProvider

A DLL deve incluir uma classe que implemente três métodos:

- `InitiateCall`: o Serviço MIM invocará esse método. O serviço passa a ID de solicitação e de número de telefone como parâmetros.  O método precisa retornar um valor de `PhoneCallStatus` igual a `Pending`, `Success` ou `Failed`.
- `GetCallStatus`: se uma chamada anterior para `initiateCall` tiver retornado `Pending`, o Serviço MIM invocará esse método. Esse método também retorna um valor de `PhoneCallStatus` igual a `Pending`, `Success` ou `Failed`.
- `GetFailureMessage`: se uma invocação anterior de `InitiateCall` ou `GetCallStatus` tiver retornado `Failed`, o Serviço MIM invocará esse método. Esse método retorna uma mensagem de diagnóstico.

As implementações desses métodos devem ser thread-safe e, além disso, a implementação do `GetCallStatus` e do `GetFailureMessage` não deve presumir que eles serão chamados pelo mesmo thread que uma chamada anterior para `InitiateCall`.

Armazene a DLL no diretório `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`.

Código de exemplo, que pode ser compilado usando o Visual Studio 2010 ou posterior.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Etapa 3: fazer backup do arquivo MfaSettings.xml, localizado na pasta "C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Etapa 4: editar o arquivo MfaSettings.xml

Atualize ou desmarque as linhas a seguir:

- Remover/desmarcar todas as linhas de entradas de configuração 

- Atualizar ou adicionar as linhas a seguir ao seguinte a MfaSettings.xml com o seu provedor de telefone personalizado <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Etapa 5: reiniciar o Serviço MIM

Depois que o serviço for reiniciado, use o SSPR e/ou o PAM para validar a funcionalidade com o provedor de identidade personalizado.

> [!NOTE] 
> Para reverter a configuração, substitua MfaSettings.xml pelo arquivo de backup na etapa 3


## <a name="next-steps"></a>Próximas etapas

- [Guia de Introdução com o Servidor de Autenticação Multifator do Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a Autenticação Multifator do Azure?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Histórico de lançamento de versão do MIM](./reference/version-history.md)
