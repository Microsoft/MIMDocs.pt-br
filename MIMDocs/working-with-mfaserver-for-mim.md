---
title: Usar o SDK do Servidor de Autenticação Multifator do Azure para ativar cenários de SSPR ou PAM | Microsoft Docs
description: Configure o SDK do Servidor de Autenticação Multifator do Azure como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management e no Autoatendimento de Redefinição de Senha.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 7191e445688cc9e3c5c02b9c6852c869a28a937a
ms.sourcegitcommit: ad0690bd57e3d056397108bf1c8417965d69a32c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43772671"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Usar o Servidor de Autenticação Multifator do Azure para ativar SSPR ou PAM
O documento a seguir descreve como configurar o servidor de MFA do Azure como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management ou no Autoatendimento de Redefinição de Senha.

> [!IMPORTANT]
> Devido ao aviso de reprovação do Software Development Kit da Autenticação Multifator do Microsoft Azure. O SDK da MFA do Azure terá suporte para os clientes atuais até a data de baixa, em 14 de novembro de 2018. Os clientes novos e atuais não poderão mais baixar o SDK por meio do portal clássico do Azure. Para baixá-lo, fale com o atendimento ao cliente do Microsoft Azure a fim de receber o pacote de credenciais de serviço gerado pela MFA. <br> A equipe de desenvolvimento da Microsoft está trabalhando nas alterações para a MFA por meio da integração com o SDK do Servidor de Autenticação Multifator do Azure.

O artigo a seguir descreverá a atualização da configuração e as etapas para habilitar uma mudança simples do SDK do MFA do Azure para o SDK do Servidor de Autenticação Multifator do Azure, quando lançadas, pois serão incluídas em um hotfix futuro. Confira o [histórico de versão](/reference/version-history.md) para ver os comunicados. 

## <a name="prerequisites"></a>Pré-requisitos

Para usar o Servidor de Autenticação Multifator do Azure com MIM, você precisa:

- Acesso à Internet de cada Serviço MIM ou Servidor de MFA fornecendo PAM para SSPR, para entrar em contato com o serviço MFA do Azure
- Uma assinatura do Azure
- A instalação já está usando o SDK do MFA do Azure
- Licenças do Azure Active Directory Premium para usuários candidatos, ou um meio alternativo de licenciar o Azure MFA
- Números de telefone para todos os usuários candidatos
- MIM com o hotfix 4.5. ou superior, confira o [histórico de versão](/reference/version-history.md) para ver os comunicados

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuração do Servidor de Autenticação Multifator do Azure 
> [!NOTE] 
> Na configuração, você precisará um certificado SSL válido instalado para o SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Etapa 1: baixar o Servidor de Autenticação Multifator do Azure do portal do Azure 
Entre no [portal do Azure](https://portal.azure.com/) e baixe o servidor de MFA do Azure.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Etapa 2: gerar credenciais de ativação
Use o link **Gerar credenciais de ativação para iniciar o uso** para gerar credenciais de ativação. Uma vez gerado, salve para uso posterior.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Etapa 3: instalar o Servidor de Autenticação Multifator do Azure
Depois de baixar o servidor, [instale](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server)-o.  Suas credenciais de ativação serão necessárias. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Etapa 4: criar seu aplicativo Web do IIS que hospedará o SDK
1. Abra o Gerenciador do IIS ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Crie um novo site chamado "MIM MFASDK" e vincule-o a um diretório vazio ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Abra o Console da Autenticação Multifator e clique no SDK do serviço Web ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Quando o assistente for exibido, clique nas opções pela configuração, selecione "MIM MFASDK" e o pool de aplicativos

> [!NOTE] 
> O assistente exigirá que um grupo de administradores seja criado. Mais informações podem ser encontradas na documentação localizada em Azure >> Documentação do Servidor de Autenticação Multifator do Azure.

5. Em seguida, precisamos importar a conta do Serviço MIM. Abra o Console de Autenticação Multifator e selecione "Usuários" a. Clique em "Importar do Active Directory" b. Navegue até a conta de serviço, também conhecido como "contoso\mimservice" c. Clique em "Importar" e "Fechar" ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Editar a conta de Serviço MIM a ser habilitada no Console de Autenticação Multifator ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Atualize a autenticação do IIS no site "MIM MFASDK". Primeiro, desabilitaremos a "Autenticação Anônima" e, em seguida, habilitaremos a "Autenticação do Windows" ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Etapa final: adicionar a conta de serviço de MIM a "Administradores do PhoneFactor" ![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurando o Serviço MIM para o Servidor de Autenticação Multifator do Azure 

### <a name="step-1-patch-server-to-452020"></a>Etapa 1: aplicar patch ao servidor, atualizando-o para a versão 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Etapa 2: fazer backup e abrir o arquivo MfaSettings.xml, localizado na pasta "C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Etapa 3: adicionar as linhas a seguir
1. Remover/desmarcar as linhas de entradas de configuração a seguir <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Atualizar ou adicionar as linhas a seguir ao seguinte a MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Reinicie o Serviço MIM e teste a funcionalidade com o Servidor de Autenticação Multifator do Azure.

> [!NOTE] 
> Para reverter a configuração, substitua MfaSettings.xml pelo arquivo de backup na etapa 2


## <a name="next-steps"></a>Próximas etapas

-    [Guia de Introdução com o Servidor de Autenticação Multifator do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a Autenticação Multifator do Azure?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Usar a API de Autenticação Multifator Personalizada para ativar SSPR ou PAM](Working-with-custommfaserver-for-mim.md)
- [Histórico de lançamento de versão do MIM](./reference/version-history.md)