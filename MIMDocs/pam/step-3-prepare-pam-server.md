---
title: Etapa 3 para implantar o PAM – servidor PAM | Microsoft Docs
description: Prepare um servidor PAM que hospedará o SQL e SharePoint para sua implantação do Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3098816d07603384a28db47c0bc60ac92a340e8f
ms.sourcegitcommit: 80507a128d2bc28ff3f1b96377c61fa97a4e7529
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83279973"
---
# <a name="step-3--prepare-a-pam-server"></a>Etapa 3 – Preparar um servidor PAM

> [!div class="step-by-step"]
> [« Etapa 2](step-2-prepare-priv-domain-controller.md)
> [Etapa 4 »](step-4-install-mim-components-on-pam-server.md)

## <a name="install-windows-server-2012-r2"></a>Instalar o Windows Server 2012 R2

Em uma terceira máquina virtual, instale o Windows Server 2012 R2, especificamente, o Windows Server 2012 R2 Standard (Servidor com GUI) x64, para criar *PAMSRV*. Como o SQL Server e o SharePoint 2013 serão instalados neste computador, é necessário, pelo menos, 8 GB de RAM.

1. Selecione **Windows Server 2012 R2 Standard (Servidor com GUI) x64**.

    ![Escolher Windows Server Standard com GUI – captura de tela](media/PAM_GS_Select_WS2012.png)

2. Leia e aceite os termos de licença.

3.  Já que o disco estará vazio, selecione **Personalizado: instalar somente o Windows** e use o **espaço em disco não inicializado**.

4.  Entre no novo computador como administrador.  Usando o Painel de Controle, dê a ele um endereço IP estático na rede virtual, configure essa interface de rede para enviar consultas DNS para o endereço IP de PRIVDC e defina o nome do computador como *PAMSRV*.  Isso exigirá reinicializar o servidor.

5.  Se a rede virtual não fornecer conectividade com a Internet, adicione uma interface de rede extra para o computador que forneça uma conexão com a Internet.  Isso será necessário para a instalação do SharePoint e poderá ser desabilitado após a conclusão dessa etapa.

6.  Após a reinicialização do servidor, entre como administrador. Usando o Painel de Controle, configure o computador para verificar se há atualizações e instale todas as atualizações necessárias.  Isso pode exigir reinicializar o servidor.

7.  Após a reinicialização do servidor, entre como Administrador, abra o Painel de Controle e ingresse PAMSRV no domínio PRIV (priv.contoso.local).  Será necessário fornecer o nome de usuário e as credenciais de um administrador de domínio PRIV (PRIV\Administrator). Depois que a mensagem de boas-vindas for exibida, feche a caixa de diálogo e reinicie o servidor.


### <a name="add-the-web-server-iis-and-application-server-roles"></a>Adicione as funções servidor Web (IIS) e servidor de aplicativos

Adicione o servidor Web (IIS) e funções de servidor de aplicativos, os recursos do .NET Framework 3.5, o módulo do Active Directory para o Windows PowerShell e outros recursos exigidos pelo SharePoint

1.  Entre como administrador de domínio PRIV (PRIV\Administrator) e inicie o PowerShell.

2.  Digite os seguintes comandos. Observe que talvez seja necessário especificar um local diferente para os arquivos de origem para os recursos do .NET Framework 3.5. Normalmente, esses recursos não estão presentes no momento da instalação do Windows Server, mas estão disponíveis na pasta SxS (lado a lado) na pasta de origens do disco de instalação do sistema operacional, por exemplo, “d:\Sources\SxS\”.

    ```PowerShell
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### <a name="configure-the-server-security-policy"></a>Configurar a política de segurança do servidor

Configure a política de segurança do servidor para permitir que as contas criadas recentemente sejam executadas como serviço.

1.  Inicie o programa de **Política de Segurança Local** .   
2.  Navegue até **Políticas Locais** > **Atribuição de Direitos do Usuário**.  
3.  No painel de detalhes, clique com o botão direito do mouse em **Fazer logon como um serviço**e selecione **Propriedades**.  
4.  Clique em **Adicionar Usuário ou Grupo** e, em Nomes de Usuário e grupo, digite *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Clique em **Verificar Nomes** e clique em **OK**.  

5.  Clique em **OK** para fechar a janela Propriedades.
6.  No painel de detalhes, clique com o botão direito do mouse em **Negar acesso a este computador pela rede**e selecione **Propriedades**.  
7.  Clique em **Adicionar Usuário ou Grupo** e, em Nomes de usuário e grupo, digite *priv\mimmonitor; priv\MIMService; priv\mimcomponent* e clique em **OK**.  
8.  Clique em **OK** para fechar a janela Propriedades.

9. No painel de detalhes, clique com o botão direito do mouse em **Negar logon localmente** e selecione **Propriedades**.  
10. Clique em **Adicionar Usuário ou Grupo** e, em Nomes de usuário e grupo, digite *priv\mimmonitor; priv\MIMService; priv\mimcomponent* e clique em **OK**.  
11. Clique em **OK** para fechar a janela de propriedades.  
12. Feche a janela de Política de Segurança Local.  

13. Abra o Painel de controle e mude para **Contas de Usuário**.
14. Clique em **Conceder a outros usuários acesso a este computador**.
15. Clique em **Adicionar**, insira o usuário *MIMADMIN* no domínio *PRIV* e, na próxima tela do assistente, clique em **Adicionar este usuário como Administrador**.  
16. Clique em **Adicionar**, insira o usuário *SharePoint* no domínio *PRIV* e, na próxima tela do assistente, clique em **Adicionar este usuário como Administrador**.  
17. Feche o Painel de Controle.

### <a name="change-the-iis-configuration"></a>Alterar a configuração do IIS

Há duas maneiras de alterar a configuração do IIS para permitir que os aplicativos usem o modo de Autenticação do Windows. Verifique se você está conectado como MIMAdmin e siga uma destas opções.

Se você quiser usar o PowerShell:

1. Clique com o botão direito do mouse em PowerShell e selecione **Executar como administrador**.
2. Parar o IIS e desbloquear as configurações de host do aplicativo usando esses comandos
   ```CMD
   iisreset /STOP
   C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
   iisreset /START
   ```

Se você quiser usar um editor de texto como o Bloco de Notas:

1. Abra o arquivo **C:\Windows\System32\inetsrv\config\applicationHost.config**
2. Role para baixo até a linha 82 desse arquivo. O valor de marca **overrideModeDefault** deve ser **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Altere o valor de **overrideModeDefault** para *Permitir*  
4. Salve o arquivo e reinicie o IIS com o comando do PowerShell `iisreset /START`

## <a name="install-sql-server"></a>Instalar o SQL Server

Se o SQL Server não estiver no ambiente de bastiões, instale o SQL Server 2012 (Service Pack 1 ou posterior) ou o SQL Server 2014. As etapas a seguir pressupõem SQL 2014.

1. Verifique se você está conectado como MIMAdmin.
2. Clique com o botão direito do mouse em PowerShell e selecione **Executar como administrador**.
3. Navegue até o diretório em que o programa de instalação do SQL Server está localizado.  
4. Digite o seguinte comando.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## <a name="install-sharepoint-foundation-2013"></a>Instalar o SharePoint Foundation 2013

Usando o instalador do SharePoint Foundation 2013 com SP1, instale os pré-requisitos de software do SharePoint no PAMSRV.

> [!NOTE]
> Este instalador exige uma conexão com a Internet para baixar os pré-requisitos. E após a instalação, o servidor será reiniciado.

1. Clique com o botão direito do mouse em PowerShell e selecione **Executar como administrador**.  
2. Altere para o diretório em que o SharePoint foi desempacotado.  
3. Digite o comando `.\prerequisiteinstaller.exe`.

Depois que os pré-requisitos do SharePoint forem instalados, instale o SharePoint Foundation 2013 com SP1.

1.  Clique com o botão direito do mouse em PowerShell e selecione **Executar como administrador**.  
2.  Altere para o diretório em que o SharePoint foi desempacotado.  
3.  Digite o comando `.\setup.exe`.  
4.  Selecione o tipo de **servidor completo** .  
5.  Depois que a instalação for concluída, selecione para executar o assistente.  

### <a name="configure-sharepoint"></a>Configurar o SharePoint

Execute o Assistente de Configuração de Produtos do SharePoint para configurar o SharePoint.

1.  Na guia Conectar-se a um Farm de Servidores, altere para **Criar um novo farm de servidores**.  
2.  Especifique **PAMSRV** como o servidor de banco de dados do banco de dados de configuração e **PRIV\SharePoint** como a conta de acesso de banco de dados que será usada pelo SharePoint.  
3.  Especifique uma senha como a frase secreta de segurança do farm (ela não será usada posteriormente neste passo a passo).  
4.  Por enquanto, aceite o restante das configurações padrão do assistente de configuração do SharePoint para criar um farm de servidor único.    
5.  Quando o assistente de configuração tiver concluído a tarefa de configuração 10 de 10, clique em **Concluir** e um navegador da Web será aberto.  
6.  No pop-up do Internet Explorer, autentique-se como o administrador de domínio (PRIV\MIMAdmin) para continuar.  
7.  Inicie o assistente no aplicativo Web para configurar o farm do SharePoint.  
8.  Selecione para usar a conta gerenciada existente (PRIV\SharePoint), desmarque para desabilitar todos os serviços opcionais e clique em **Avançar**.  
9. Depois que a Janela de Criação de Conjunto de Sites for exibida, clique em **Ignorar** e **Concluir**.  

## <a name="create-a-sharepoint-foundation-2013-web-application"></a>Criar um aplicativo Web do SharePoint Foundation 2013

Depois de concluir os assistentes, use o PowerShell para criar um Aplicativo Web do SharePoint Foundation 2013 para hospedar o Portal do MIM. Como este passo a passo se destina a fins de demonstração, o SSL não será habilitado.

1.  Clique com o botão direito do mouse em Shell de Gerenciamento do SharePoint 2013, selecione **Executar como administrador** e execute o seguinte script do PowerShell:

    ```PowerShell
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Uma mensagem de aviso será exibida informando que o método de autenticação Clássico do Windows está sendo usado, e poderá levar vários minutos para que o comando final seja retornado.  Quando concluído, a saída fornecerá a URL do novo portal.

> [!NOTE]
> Mantenha a janela do Shell de Gerenciamento do SharePoint 2013 aberta para usá-la na próxima etapa.

## <a name="create-a-sharepoint-site-collection"></a>Criar uma coleção de sites do SharePoint

Em seguida, crie uma Coleção de Sites do SharePoint associado a esse aplicativo Web para hospedar o Portal do MIM.

1.  Inicie o **Shell de Gerenciamento do SharePoint 2013** se ainda não estiver aberto e execute o seguinte script do PowerShell

    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Verifique se a variável **CompatibilityLevel** está definida como *14*. Se ele retornar *15*, o conjunto de sites não foi criado para a versão da experiência 2010; exclua o conjunto de sites e recrie-o.

2.  Execute os seguintes comandos do PowerShell no **Shell de Gerenciamento do SharePoint 2013**. Isso desabilitará o ViewState do servidor do SharePoint e a tarefa do SharePoint **Trabalho de Análise da Integridade (Por Hora, Temporizador do Microsoft SharePoint Foundation, Todos os Servidores)** .

    ```PowerShell
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## <a name="change-update-settings"></a>Alterar as configurações de atualização

1. Abra o Painel de Controle, navegue até **Windows Update** e clique para **alterar as configurações**.  
2. Altere as configurações para receber atualizações do Windows Update e de outros produtos do Microsoft Update.  
3. Verifique se há novas atualizações e garanta que todas as atualizações importantes pendentes serão instaladas antes de continuar.

## <a name="set-the-website-as-the-local-intranet"></a>Definir o site como a intranet local

1. Inicie o Internet Explorer e abra uma nova guia do navegador da Web
2. Navegue até `http://pamsrv.priv.contoso.local:82/` e entre como PRIV\MIMAdmin.  Um site do SharePoint vazio chamado “Portal do MIM” será mostrado.  
3. No Internet Explorer, abra **Opções da Internet**, mude para a guia **Segurança**, selecione **Intranet local** e adicione a URL `http://pamsrv.priv.contoso.local:82/`.

Se a entrada falhar, os SPNs do Kerberos criados anteriormente na [Etapa 2](step-2-prepare-priv-domain-controller.md) talvez precisem ser atualizados.

## <a name="start-the-sharepoint-administration-service"></a>Iniciar o serviço de administração do SharePoint

Usando **Serviços** (localizado em Ferramentas Administrativas), inicie o serviço **Administração do SharePoint**, se ainda não estiver em execução.

Na Etapa 4, você começará a instalação dos componentes do MIM no servidor PAM.

> [!div class="step-by-step"]
> [« Etapa 2](step-2-prepare-priv-domain-controller.md)
> [Etapa 4 »](step-4-install-mim-components-on-pam-server.md)
