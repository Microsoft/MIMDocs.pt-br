---
title: Configurar o SQL Server para o Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Instale o SQL Server 2016 durante a preparação para a instalação do MIM 2016.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6fe251a3976167909aa55a687884585b1937ebf3
ms.sourcegitcommit: 28834821cbddd6384613d8ba45424c35f4c39ce6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538550"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Configure um servidor de gerenciamento de identidade: SQL Server 2016

> [!div class="step-by-step"]
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
> 
> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome do domínio - **contoso**
> - Nome do Servidor do Serviço MIM - **corpservice**
> - Nome do Servidor de Sincronização do MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Senha – <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Instale o **SQL Server 2016 Standard/Enterprise Edition**

1. Inicie o **PowerShell** como um administrador de domínio.

2. Altere para o diretório em que o programa de instalação do SQL Server está localizado.

3. Digite os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```
> 
> [!div class="step-by-step"]  
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
