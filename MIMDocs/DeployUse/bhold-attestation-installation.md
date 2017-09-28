---
title: "Instalação do Atestado do BHOLD | Microsoft Docs"
description: "O módulo Atestado do BHOLD permite designar revisores e realizar análises"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 93d0b9a17d82911b71b1b220465b6d637687444b
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-attestation-installation"></a>Instalação do Atestado do BHOLD

O módulo Atestado do BHOLD permite designar revisores e realizar análises recorrentes das relações entre os usuários e as contas e permissões por aplicativo.

## <a name="bhold-attestation-installation-requirements"></a>Requisitos para instalação do Atestado do BHOLD

Antes de instalar o módulo Atestado do BHOLD, você deve instalar o módulo BHOLD Core no servidor no qual você planeja instalar o módulo Atestado do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Já que o módulo Atestado do BHOLD envia mensagens de email aos usuários, seu ambiente deve ter um servidor de email de protocolo SMTP como o Microsoft Exchange Server.

>[!IMPORTANT]
Se você estiver instalando ambos o Relatório do BHOLD e o Atestado do BHOLD, você deverá instalar o Relatório do BHOLD antes de instalar o Atestado do BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de você começar a instalar o módulo Atestado do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de Configuração do Atestado do BHOLD requer para concluir a instalação. A planilha a seguir ajudará você a registrar essas informações, de modo que você estará pronto para fornecê-las quando necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar um provedor de segurança em um Domínio/Computador** | Quando selecionado, especifica que a segurança do Active Directory Domain Services controlará o acesso ao BHOLD Core.                                                                                                                | Marque a caixa de seleção. **Importante:** a instalação falhará se essa caixa de seleção não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio é fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Usuário**                                    | Especifica o nome de logon da conta de usuário do serviço BHOLD Core.                                                                                                                                                          | Grave o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Senha**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Grave a senha aqui: **Importante:** certifique-se de manter essa senha em um local seguro, oculto.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalação do Atestado do BHOLD

Para instalar o módulo Atestado do BHOLD, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo Atestado do BHOLD:

- BholdAttestation*\<Versão\>*\_Release.msi

Substitua *\<Versão\>* pelo número de versão do Atestado do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximas etapas

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)