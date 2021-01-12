---
title: Usar o Azure MFA para ativar o PAM | Microsoft Docs
description: Configure o Azure MFA como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 1c26147dc1e192804011ee104989a508acb25974
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104047"
---
# <a name="using-azure-mfa-for-activation-in-mim-pam"></a>Usando o Azure MFA para ativação no PAM do MIM
> [!IMPORTANT]
> As versões anteriores do MIM usaram o SDK (Software Development Kit) da MFA (autenticação multifator) do Azure para integrar com o Azure MFA.  Devido à reprovação do SDK do Azure MFA, os clientes do MIM novos e existentes não poderão mais baixar o SDK do Azure MFA. Para obter informações sobre como usar o servidor MFA do Azure no lugar do SDK do Azure MFA, consulte [usando o servidor do Azure MFA no Pam ou SSPR](../working-with-mfaserver-for-mim.md).




Ao configurar uma função do PAM, você pode escolher como autorizar os usuários que solicitam a ativação da função. As opções que a atividade de autorização de PAM implementa são:

- Aprovação do proprietário da função
- [MFA (Autenticação Multifator) do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Se nenhuma das verificações estiver habilitada, os usuários de candidatos serão ativados automaticamente para sua função.

O MFA (Microsoft Azure Multi-Factor Authentication) é um serviço de autenticação que exige que os usuários verifiquem suas tentativas de entrada usando aplicativo móvel, chamada telefônica ou mensagem de texto. Ele está disponível para uso com o Active Directory do Microsoft Azure e como um serviço para aplicativos corporativos de nuvem e locais. Para o cenário do PAM, o Azure MFA fornece um mecanismo de autenticação adicional. O Azure MFA pode ser usado para autorização, independentemente de como um usuário foi autenticado no domínio PRIV do Windows.

> [!NOTE]
> A abordagem do PAM com um ambiente de bastiões fornecido pelo MIM destina-se a ser usada em uma arquitetura personalizada para ambientes isolados em que o acesso à Internet não está disponível, onde essa configuração é exigida pela regulamentação ou em ambientes isolados de alto impacto como laboratórios de pesquisa offline e tecnologia de controle de supervisão e de aquisição de dados.  Como o Azure MFA é um serviço de Internet, essas diretrizes são fornecidas exclusivamente para clientes existentes do PAM no MIM ou para os ambientes em que essa configuração é exigida pela regulamentação. Se seu Active Directory fizer parte de um ambiente conectado à Internet, consulte [protegendo o acesso privilegiado](/security/compass/overview) para obter mais informações sobre onde começar.

## <a name="prerequisites"></a>Pré-requisitos

Para usar o Azure MFA com o PAM do MIM, você precisa de:

- Acesso à Internet de cada serviço de MIM fornecendo PAM para entrar em contato com o serviço Azure MFA
- Uma assinatura do Azure
- MFA do Azure
- Azure Active Directory Premium licenças para usuários candidatos
- Números de telefone para todos os usuários candidatos

## <a name="downloading-the-azure-mfa-service-credentials"></a>Baixando as Credenciais do Serviço Azure MFA

Anteriormente, você baixaria um arquivo ZIP do SDK do Azure MFA que incluía o material de autenticação do MIM para entrar em contato com o Azure MFA. No entanto, como o SDK do Azure MFA não está mais disponível, consulte [usando o servidor do Azure MFA no Pam ou SSPR](../working-with-mfaserver-for-mim.md) para obter informações sobre como usar o servidor MFA do Azure em vez disso.


## <a name="configuring-the-mim-service-for-azure-mfa"></a>Configurando o Serviço MIM para o Azure MFA

1.  No computador em que o Serviço MIM está instalado, entre como administrador ou como o usuário que instalou o MIM.

2.  Crie uma nova pasta de diretório no diretório em que o Serviço MIM foi instalado, como ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Usando o Windows Explorer, navegue até a pasta ```pf\certs``` do arquivo ZIP baixado na seção anterior. Copie o arquivo ```cert\_key.p12``` no novo diretório.

4.  Usando o Windows Explorer, navegue até a ```pf``` pasta do zip e abra o arquivo ```pf\_auth.cs``` em um editor de texto como o bloco de notas.

5. Localize estes três parâmetros: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Copiar valores do arquivo pf\_auth.cs – captura de tela](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Usando o Bloco de Notas, abra **MfaSettings.xml** localizado em ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copie os valores dos parâmetros LICENSE\_KEY, GROUP\_KEY e CERT\_PASSWORD no arquivo pf\_auth.cs em seus respectivos elementos xml no arquivo MfaSettings.xml.

8. No **<CertFilePath>** elemento XML, especifique o nome do caminho completo do \_ arquivo. p12 da chave de certificado extraído anteriormente.

9. No **<username>** elemento, insira qualquer nome de usuário.

10. No **<DefaultCountryCode>** elemento, insira o código do país para discar os usuários, como 1 para o Estados Unidos e o Canadá. Esse valor é usado caso usuários sejam registrados com números de telefone que não têm um código de país. Se o número de telefone de um usuário tem um código de país internacional diferente do que é configurado para a organização, esse código de país deve ser incluído no número de telefone que será registrado.

11. Salve e substitua o arquivo **MfaSettings.xml** na pasta ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service``` do Serviço MIM.

> [!NOTE]
> Ao final do processo, verifique se o arquivo **MfaSettings.xml** ou quaisquer cópias dele ou se o arquivo ZIP não podem ser lidos publicamente.

## <a name="configure-pam-users-for-azure-mfa"></a>Configurar os usuários do PAM para o Azure MFA

Para um usuário ativar uma função que requer o Azure MFA, o número de telefone do usuário deve ser armazenado em MIM. Há duas maneiras de definir esse atributo.

Primeiro, o comando `New-PAMUser` copia um atributo de número de telefone da entrada de diretório do usuário no domínio CORP para o banco de dados do Serviço MIM. Observe que essa é uma operação realizada uma única vez.

Em segundo lugar, o comando `Set-PAMUser` atualiza o atributo de número de telefone no banco de dados do Serviço MIM. Por exemplo, o apresentado a seguir substitui o número de telefone do usuário de PAM presente no Serviço MIM. A entrada de diretório deles não é alterada.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Configurar as funções do PAM para o Azure MFA

Depois que todos os usuários candidatos a uma função do PAM tiverem seus números de telefone armazenados no banco de dados do Serviço MIM, a função pode ser configurada para exigir o Azure MFA. Faça isso usando os comandos `New-PAMRole` ou `Set-PAMRole`. Por exemplo,

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Especificando o parâmetro “-MFAEnabled 0” no comando `Set-PAMRole`, é possível desabilitar o Azure MFA para uma função.

## <a name="troubleshooting"></a>Solução de problemas

Os eventos a seguir podem ser encontrados no log de eventos de Privileged Access Management:

| ID  | Severidade | Gerado por | Descrição |
|-----|----------|--------------|-------------|
| 101 | Error       | Serviço MIM            | Usuário não concluiu a Azure MFA (por exemplo, não respondeu ao telefone) |
| 103 | Informações | Serviço MIM            | Usuário concluiu a Azure MFA durante a ativação                       |
| 825 | Aviso     | Serviço de Monitoramento do PAM | O número de telefone foi alterado                                |

## <a name="next-steps"></a>Próximas etapas

- [O que é a autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
