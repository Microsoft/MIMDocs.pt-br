---
title: Configurar o SharePoint para o Microsoft Identity Manager 2016 | Microsoft Docs
description: Instale e configure o SharePoint Foundation para que ele possa hospedar a página do Portal do MIM.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 466f5eb7d4aee27336948e15f96087d6ba898170
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358628"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configure um servidor de gerenciamento de identidade: SharePoint

> [!div class="step-by-step"]
> [« SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server »](prepare-server-exchange.md)
> 
> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome do domínio - **contoso**
> - Nome do Servidor do Serviço MIM - **corpservice**
> - Nome do Servidor de Sincronização do MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Senha – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Instalar o **SharePoint 2016**

> [!NOTE]
> O instalador exige uma conexão com a Internet para baixar seus pré-requisitos. Se o computador estiver em uma rede virtual que não forneça conectividade com a Internet, adicione uma interface de rede extra para o computador que forneça uma conexão com a Internet. Isso pode ser desabilitado depois que a instalação estiver concluída.

Siga estas etapas para instalar o SharePoint 2016. Após concluir a instalação, o servidor será reiniciado.

1.  Inicie o **PowerShell** como uma conta de domínio com administrador local no **corpservice** e **sysadmin**, no servidor de banco de dados SQL usaremos **contoso\miminstall**.

    -   Altere para o diretório em que o SharePoint foi desempacotado.

    -   Digite o seguinte comando.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Após a instalação dos pré-requisitos do **SharePoint**, instale o **SharePoint 2016** digitando o seguinte comando:

    ```
    .\setup.exe
    ```

3.  Selecione o tipo de servidor completo.

4.  Depois que a instalação for concluída, execute o assistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Execute o Assistente para configurar o SharePoint

Siga as etapas alinhadas no **Assistente de Configuração de Produtos do SharePoint** para configurar o SharePoint para trabalhar com o MIM.

1. Na guia **Conectar-se a um farm de servidores** , altere para criar um novo farm de servidores.

2. Especifique esse servidor como o servidor de banco de dados, por exemplo, **corpsql**, para o banco de dados de configuração, e *Contoso\SharePoint* como a conta de acesso do banco de dados para uso do SharePoint.
3. Crie uma senha de segurança para o farm.

4. No Assistente de configuração, recomendamos a seleção do tipo [MinRole](https://docs.microsoft.com/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) de **Front-end**

5. Quando o assistente de configuração tiver concluído a tarefa de configuração 10 de 10, clique em Concluir e um navegador da Web será aberto.

6. Se a pop-up do Internet Explorer for exibida, autentique-se como *Contoso\miminstall* (ou a conta de administrador de domínio equivalente) para continuar.

7. No assistente da Web (dentro do aplicativo Web), clique em **Cancelar/Ignorar**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparação do SharePoint para hospedar o Portal do MIM

> [!NOTE]
> Inicialmente, SSL não será configurada. Certifique-se de configurar SSL ou equivalente antes de habilitar o acesso a este portal.

1. Inicie o **Shell de Gerenciamento do SharePoint 2016** e execute o script do PowerShell a seguir para criar um **Aplicativo Web do SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Uma mensagem de aviso será exibida informando que o método de autenticação Clássico do Windows está sendo usado e pode ser que demore para que o comando final seja retornado. Quando concluído, a saída indicará a URL do novo portal. Mantenha a janela **Shell de Gerenciamento do SharePoint 2016** aberta para referência posterior.

2. Inicie o Shell de Gerenciamento do SharePoint 2016 e execute o script a seguir do PowerShell para criar um **Conjunto de Sites do SharePoint** associado ao aplicativo Web.

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > Verifique se o resultado da variável *CompatibilityLevel* é "15". Se o resultado for diferente de "15", o conjunto de sites não terá sido criado para a versão correta; exclua o conjunto de sites e recrie-o.

3. Desabilite o **estado de exibição do lado do servidor do SharePoint** e a tarefa do SharePoint "Trabalho de Análise de Integridade (Por Hora, Timer do Microsoft SharePoint Foundation, Todos os Servidores)", executando os seguintes comandos do PowerShell no **Shell de Gerenciamento do SharePoint 2016**:

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. No servidor de gerenciamento de identidade, abra uma nova guia do navegador da Web, navegue até http://mim.contoso.com/ e faça logon como *contoso\miminstall*.  Um site do SharePoint vazio chamado *Portal do MIM* será mostrado.

    ![Portal do MIM na imagem http://mim.contoso.com/](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Em seguida, em Internet Explorer, abra **Opções da Internet**, alterne para a guia **Segurança**, selecione **Intranet local** e clique em **Sites**.

    ![Imagem de opções da Internet](media/MIM-DeploySP2.png)

6. Na janela **Intranet Local**, clique em **Avançado** e cole a URL copiada na caixa de texto **Adicionar este site à zona**. Clique em **Adicionar** e feche as janelas.

7. Abra o programa **Ferramentas Administrativas**, navegue até **Serviços**, localize o serviço de Administração do SharePoint e inicie-o, se ainda não estiver sendo executado.

> [!div class="step-by-step"]  
> [« SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server »](prepare-server-exchange.md)
