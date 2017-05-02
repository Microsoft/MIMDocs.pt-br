---
title: Configurar o SharePoint para o Microsoft Identity Manager 2016 | Microsoft Docs
description: "Instale e configure o SharePoint Foundation para que ele possa hospedar a página do Portal do MIM."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 2af432036033f8914d00228cd3d2d1af84f13054
ms.lasthandoff: 01/24/2017


---

# <a name="set-up-an-identity-management-server-sharepoint"></a>Configure um servidor de gerenciamento de identidade: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **mimservername**
> - Nome do domínio - **contoso**
> - Senha – **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>Instale o **SharePoint Foundation 2013 com SP1**

> [!NOTE]
> O instalador exige uma conexão com a Internet para baixar seus pré-requisitos. Se o computador estiver em uma rede virtual que não forneça conectividade com a Internet, adicione uma interface de rede extra para o computador que forneça uma conexão com a Internet. Isso pode ser desabilitado depois que a instalação estiver concluída.

Siga estas etapas para instalar o SharePoint Foundation 2013 SP1. Após concluir a instalação, o servidor será reiniciado.

1.  Inicie o **PowerShell** como um administrador de domínio.

    -   Altere para o diretório em que o SharePoint foi desempacotado.

    -   Digite o seguinte comando.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Depois de os pré-requisitos do **SharePoint** estarem instalados, instale o **SharePoint Foundation 2013 com SP1** digitando o seguinte comando:

    ```
    .\setup.exe
    ```

3.  Selecione o tipo de servidor completo.

4.  Depois que a instalação for concluída, execute o assistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Execute o Assistente para configurar o SharePoint

Siga as etapas alinhadas no **Assistente de Configuração de Produtos do SharePoint** para configurar o SharePoint para trabalhar com o MIM.

1. Na guia **Conectar-se a um farm de servidores** , altere para criar um novo farm de servidores.

2. Especifique esse servidor como o servidor de banco de dados para o banco de dados de configuração e *Contoso\SharePoint* como a conta de acesso do banco de dados para o SharePoint usar.

3. Crie uma senha de segurança para o farm.

4. Quando o assistente de configuração tiver concluído a tarefa de configuração 10 de 10, clique em Concluir e um navegador da Web será aberto.

5. No pop-up do Internet Explorer, autentique-se como *Contoso\Administrator* (ou a conta de administrador de domínio equivalente) para continuar.

6. Inicie o assistente (dentro do aplicativo web) para configurar o farm do SharePoint.

7. Selecione a opção para usar a conta gerenciada existente (*Contoso\SharePoint*) e clique em **Avançar**.

8. Na janela **Criar um Conjunto de Sites** , clique em **Ignorar**.  Em seguida, clique em **Concluir**.

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparação do SharePoint para hospedar o Portal do MIM

> [!NOTE]
> Inicialmente, SSL não será configurada. Certifique-se de configurar SSL ou equivalente antes de habilitar o acesso a este portal.

1. Inicie o **Shell de Gerenciamento do SharePoint 2013** e execute o script do PowerShell a seguir para criar um **Aplicativo Web do SharePoint Foundation 2013**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Uma mensagem de aviso será exibida informando que o método de autenticação Clássico do Windows está sendo usado e pode ser que demore para que o comando final seja retornado. Quando concluído, a saída indicará a URL do novo portal. Mantenha a janela **Shell de Gerenciamento do SharePoint 2013** aberta para referência posterior.

2. Inicie o Shell de Gerenciamento do SharePoint 2013 e execute o script a seguir do PowerShell para criar um **Conjunto de Sites do SharePoint** associado ao aplicativo Web.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Verifique se o resultado da variável *CompatibilityLevel* é "14". Se o resultado for "15", o conjunto de sites não foi criado para a versão da experiência 2010; exclua o conjunto de sites e recrie-o.

3. Desabilite o **viewstate do lado do servidor do SharePoint** e a tarefa do SharePoint "Trabalho de Análise de Integridade (Por Hora, Timer do Microsoft SharePoint Foundation, Todos os Servidores)", executando os seguintes comandos do PowerShell no **Shell de Gerenciamento do SharePoint 2013**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. No servidor de gerenciamento de identidade, abra uma nova guia do navegador da Web, navegue até http://localhost:82 / e faça logon como *contoso\Administrador*.  Um site do SharePoint vazio chamado *Portal do MIM* será mostrado.

    ![Portal do MIM em http://localhost:82 / imagem](media/MIM-DeploySP1.png)

5. Em seguida, em Internet Explorer, abra **Opções da Internet**, alterne para a guia **Segurança**, selecione **Intranet local** e clique em **Sites**.

    ![Imagem de opções da Internet](media/MIM-DeploySP2.png)

6. Na janela **Intranet Local**, clique em **Avançado** e cole a URL copiada na caixa de texto **Adicionar este site à zona**. Clique em **Adicionar** e feche as janelas.

7. Abra o programa **Ferramentas Administrativas**, navegue até **Serviços**, localize o serviço de Administração do SharePoint e inicie-o, se ainda não estiver sendo executado.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

