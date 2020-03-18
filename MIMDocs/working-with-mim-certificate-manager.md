---
title: Implantar o aplicativo Windows do Gerenciador de Certificados do MIM | Microsoft Docs
description: Saiba como implantar o aplicativo Gerenciador de certificados para permitir que os usuários gerenciem seus próprios direitos de acesso.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/16/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2adf1152aaf874d0ff0d93079fb4bfbfcf731b60
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044286"
---
# <a name="mim-certificate-manager-windows-store-application-deployment"></a>Implantação de aplicativo Gerenciador de Certificados do MIM da Windows Store

Depois de ter o MIM 2016 e o Gerenciador de Certificados em funcionamento, você pode implantar o aplicativo Gerenciador de Certificados do MIM da Windows Store. O aplicativo da Windows Store permite que os usuários gerenciem seus cartões inteligentes físicos, cartões inteligentes virtuais e certificados de software. As etapas para implantar o aplicativo de MIM CM são as seguintes:

1. Criar um modelo de certificado.

2. Criar um modelo de perfil.

3. Preparar o aplicativo.

4. Implante o aplicativo por meio do SCCM ou do Intune.

## <a name="create-a-certificate-template"></a>Criar um modelo de certificado

Você cria um modelo de certificado para o aplicativo CM da mesma maneira que faria normalmente, exceto que você precisa certificar-se de que o modelo de certificado é a versão 3 e superiores.

1. Faça logon no servidor que executa o AD CS (o servidor de certificado).

2. Abra o MMC.

3. Clique em **Arquivo &gt; Adicionar/Remover Snap-in**.

4. Na lista de snap-ins disponíveis, clique duas vezes em **Modelos de Certificado** e, em seguida, clique em **Adicionar**.

5. Agora você verá **Modelos de Certificado** em **Raiz do Console** no MMC. Clique duas vezes para exibir todos os modelos de certificado disponíveis.

6. Clique com o botão direito do mouse no modelo de **Logon de Cartão Inteligente** e clique em **Duplicar Modelo**.

7. Na guia Compatibilidade, em Autoridade de Certificação, selecione Windows Server 2008. Em destinatário do certificado, selecione Windows 8.1/Windows Server 2012 R2. A versão do modelo de versão é definida na primeira vez que você cria e salva o modelo de certificado. Se você não tiver criado o modelo de certificado dessa maneira, não será possível modificá-lo para a versão correta.

   > [!NOTE]
   >  Esta etapa é essencial porque garante que você tenha um modelo de certificado versão 3 (ou superior). Apenas modelos da versão 3 funcionam com o aplicativo do gerenciador de certificados.

8. Na guia **Geral** , no campo **Nome de Exibição** , digite o nome que você deseja exibir na interface de usuário do aplicativo, como **Logon de Cartão Inteligente Virtual**.

9. Na guia **Tratamento de Solicitação**, defina a **Finalidade** para **Assinatura e criptografia** e em **Fazer o seguinte...** selecione **Avisar o usuário durante o registro**.

10. Na guia **Criptografia** em **Categoria de Provedor**, selecione **Provedor de armazenamento de chave e solicitações podem usar qualquer provedor disponível no computador da entidade**.

    > [!NOTE]
    > Você só verá o provedor de armazenamento de chave como uma opção se o modelo for da versão 3. Se você não o visualizar, provavelmente não criou o modelo de certificado corretamente com a versão correta. Recomece na etapa 5 acima.

11. Na guia **Segurança** , adicione o grupo de segurança ao qual deseja fornecer acesso de **Registro** . Por exemplo, se você quiser fornecer acesso a todos os usuários, selecione o grupo **Usuários autenticados** e, em seguida, selecione **Permissões de registro** para eles.

12. Clique em **OK** para finalizar suas alterações e criar o novo modelo. Você deve poder ver o novo modelo na lista de modelos de certificado.

13. Selecione **Arquivo** e clique em **Adicionar/Remover Snap-in** para adicionar o snap-in da Autoridade de Certificação ao console do MMC. Quando for perguntado qual computador você deseja gerenciar, selecione **Computador Local**.

14. No painel esquerdo do MMC, expanda **Autoridade de Certificação (Local)** e, em seguida, expanda a CA dentro da lista Autoridade de Certificação.

15. Clique com o botão direito do mouse em **Modelos de Certificado** e clique em **Novo &gt; Modelo de Certificado** a Ser Emitido.

16. Na lista, selecione o novo modelo criado e clique em **OK**.

## <a name="create-a-profile-template"></a>Criar um modelo de perfil

Certifique-se de quando criar um modelo de perfil para defini-lo para criar/destruir o vSC e remover a coleta de dados. O aplicativo CM não pode manipular os dados coletados, portanto, é importante para desabilitá-lo da seguinte maneira.

1.  Faça logon no portal do CM como um usuário com privilégios administrativos.

2.  Vá para Administração &gt; Gerenciar Modelos de Perfil. Verifique se a caixa está marcada ao lado do **Modelo de Perfil de Logon do Cartão Inteligente de Exemplo do MIM CM** e clique em Copiar um modelo de perfil selecionado.

3.  Digite o nome do modelo de perfil e clique em **OK**.

4.  Na próxima tela, clique em **Adicionar novo modelo de certificado** e certifique-se de marcar a caixa ao lado do nome da autoridade de certificação.

5.  Marque a caixa ao lado do nome de **Logon** do modelo de perfil e clique em **Adicionar**.

6.  Remova o modelo SmartCardLogon marcando a caixa ao lado dele e, em seguida, clicando em **Excluir modelos de certificado selecionados** e **OK**.

7.  Role até a parte inferior e clique em **Alterar configurações**.

8.  Marque as caixas ao lado de **Criar/Destruir cartão inteligente virtual** e **Diversificar chave de administração**.

9. Em **Política de PIN do Usuário** , selecione **Fornecida pelo Usuário**.

10. No painel esquerdo, clique em **Renovar Política &gt; Alterar configurações gerais**. Selecione **Reutilizar cartão ao renovar** e clique em **OK**.

11. Você precisa desabilitar itens de coleta de dados para cada política clicando na política no painel esquerdo. Você precisa então marcar a caixa ao lado de **Item de dados de exemplo**, clicar em **Excluir itens de coleta de dados** e, por fim, clicar em **OK**.

## <a name="prepare-the-cm-app-for-deployment"></a>Preparar o aplicativo CM para implantação

1. No prompt de comando, execute o comando a seguir para descompactar o aplicativo. O comando extrairá o conteúdo em uma nova subpasta chamada appx e criará uma cópia para que você não modifique o arquivo original.

    ```cmd
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2. Na pasta appx, altere o nome do arquivo chamado CustomDataExample.xml para Custom.data

3. Abra o arquivo Custom,data e modifique os parâmetros conforme necessário.


   |                     |                                                                                                                                                                                                          |
   |---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      URL do MIMCM      |                                              O FQDN do portal usado para configurar o CM. Por exemplo, https://mimcmServerAddress/certificatemanagement                                              |
   |      URL DO ADFS       | Se for usar o AD FS, insira a URL do AD FS. Por exemplo, <https://adfsServerSame/adfs> </br> Se o ADFS não for usado, defina essa configuração com uma cadeia de caracteres vazia.  Por exemplo, ```<ADFS URL=""/>``` |
   |     PrivacyUrl      |                                         Você pode incluir uma URL para uma página da Web explicando o que fazer com os detalhes do usuário coletados para o registro de certificado.                                          |
   |     SupportMail     |                                                                           Você pode incluir um endereço de email para problemas de suporte.                                                                           |
   | LobComplianceEnable |                                                                     Você pode definir isso como true ou false. Por padrão, é definido para true.                                                                      |
   |  MinimumPinLength   |                                                                                        Por padrão, é definido para 6.                                                                                         |
   |      NonAdmin       |           Você pode definir isso como true ou false. Por padrão, é definido para false. Modifique isso apenas se quiser que os usuários não administradores em seus computadores possam registrar e renovar certificados.            |

   > [!IMPORTANT]
   > É necessário especificar um valor para a URL do ADFS. Se nenhum valor for especificado, o aplicativo moderno gerará um erro no primeiro uso.
4. Salve o arquivo e saia do editor.

5. A assinatura do pacote cria um arquivo de assinatura, assim, você precisa excluir o arquivo de assinatura original chamado AppxSignature.p7x.

6. O arquivo AppxManifest.xml especifica o nome da entidade do certificado de autenticação. Abra esse arquivo para editá-lo.

7. Você precisa obter um certificado de assinatura antes de iniciar esta seção. Consulte a seguir, Habilitando a renovação do cartão inteligente para não administradores no Certificate Manager do MIM 2016, etapa 1.

8. No elemento &lt;Identity&gt;, modifique o valor do atributo Publisher para que este seja idêntico à entidade listada no certificado de autenticação, por exemplo “CN=SUBJECT”.

9. Salve o arquivo e saia do editor.

10. No prompt de comando, execute o seguinte comando para reempacotar e assinar o arquivo .appx.

    ```cmd
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    em que o nome do pacote do aplicativo é o mesmo nome usado quando você criou a cópia.

    ```cmd
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Isso fornece o novo arquivo .appx. O arquivo pfx fornece um local para o certificado de autenticação e uma senha para o arquivo .pfx.

11. Para trabalhar com a autenticação do AD FS:

    - Abra o aplicativo do Cartão Inteligente Virtual. Isso facilita a localização dos valores necessários para a próxima etapa.

    - No servidor do AD FS, abra o Windows PowerShell e execute o comando `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`, para adicionar o aplicativo como um cliente no servidor do AD FS e configurar o CM no servidor

       O script ConfigureMimCMClientAndRelyingParty.ps1 é exibido a seguir:

      ```PowerShell
       # HELP

       <#
       .SYNOPSIS
                       Configure ADFS for CM client app and server.
       .DESCRIPTION
          What the Script does:
                                       a. Registers the MIM CM client app on the ADFS server.
                                       b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                       For parameter information, see 'get-help -detailed'
       .PARAMETER redirectUri
                       The redirectUri for CM client app. Will be added to ADFS server.
                       It can be found as follows:
                       1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                       2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
       .PARAMETER serverFqdn
                       Your deployed MIM CM server’s FQDN.
       .EXAMPLE
          .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
       #>

       # Parameter declaration
       [CmdletBinding()]
       Param(
         [Parameter(Mandatory=$True)]
          [string]$redirectUri,

          [Parameter(Mandatory=$True)]
          [string]$serverFqdn
       )

       Write-Host "Configuring ADFS Objects for OAuth.."

       #Configure SSO to get persistent sign on cookie
       Set-ADFSProperties -SsoLifetime 2880

       #Configure Authentication Policy
       #Intranet to use Kerberos
       #Extranet to use U/P

       #Create Client Objects

       Write-Host "Creating Client Objects..."

       $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

       if ($existingClient -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Modern App, removing"
           Remove-ADFSClient -TargetName "MIM CM Modern App"
           Write-Host "Client object removed"
       }

       Write-Host "Adding Client Object for MIM CM Modern App client"
       Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
       Write-Host "Client Object for MIM CM Modern App client Created"

       #Create Relying Parties
       Write-Host "Creating Relying Party Objects"

       $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
       if ($existingRp -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Service RP, removing"
           Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
           Write-Host "RP object Removed"
       }

       $authorizationRules =
       "@RuleTemplate = `"AllowAllAuthzRule`"
       => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

       $issuanceRules =
       "@RuleTemplate = `"LdapClaims`"
       @RuleName = `"Emit UPN and common name`"
       c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
       => issue(store = `"Active Directory`", types =
       (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
       `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
       `";userPrincipalName,cn;{0}`", param = c.Value);

       @RuleTemplate = `"PassThroughClaims`"
       @RuleName = `"Pass through Windows Account Name`"
       c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

       Write-Host "Creating RP Trust for MIM CM Service"
       Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
       Write-Host "RP Trust for MIM CM Service has been created"
       ```

    - Atualize os valores de redirectUri e serverFQDN.

    - Para localizar o redirectUri, no aplicativo de Cartão Inteligente Virtual, abra o painel de configurações do aplicativo, clique em **Configurações**e o URI de redirecionamento deve ser listado na barra de endereços do servidor do AD FS. O URI só aparecerá se um endereço de servidor ADFS estiver configurado.

    - O serverFQDN é o nome do computador completo do servidor MIMCM somente.

    - Para obter ajuda com o script **ConfigureMIimCMClientAndRelyingParty.ps1**, execute: </br> 
      ```Powershell
      get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1
      ```

## <a name="deploy-the-app"></a>Implantar o aplicativo

Depois de configurar o aplicativo CM, no Centro de Download, baixe o arquivo MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip e extraia todo o conteúdo. O arquivo .appx é o instalador. Você pode implantá-lo da maneira que normalmente implanta aplicativos da Windows Store usando o [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx)ou o [Intune](https://technet.microsoft.com/library/dn613839.aspx) para carregar o aplicativo para que os usuários tenham acesso a ele por meio do portal da empresa ou para que seja enviado diretamente às suas máquinas.

## <a name="next-steps"></a>Próximas etapas

- [Configurando modelos de perfil](https://technet.microsoft.com/library/cc708656)
- [Gerenciamento de Aplicativos de Cartão Inteligente](https://technet.microsoft.com/library/cc708681)
