---
title: Configurar o SQL Server para o Microsoft Identity Manager 2016 SP2 | Microsoft Docs
description: Instale o SQL Server 2016 ou 2017 durante a preparação para a instalação do MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4be699f123bf7d48b709ee8b8e91e2222cd492e2
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568021"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Configurar um servidor de gerenciamento de identidade: SQL Server 2016 ou 2017

> [!div class="step-by-step"]
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
 
> [!NOTE] 
> O procedimento de instalação do SQL Server 2017 é igual ao do SQL Server 2016.

> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome do domínio - **contoso**
> - Nome do Servidor do Serviço MIM - **corpservice**
> - Nome do Servidor de Sincronização do MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Senha – <strong>Pass@word1</strong>

> [!IMPORTANT]
> O MIM 2016 SP2 dá suporte aos ouvintes do AoAG (Grupo de Disponibilidade AlwaysOn) do SQL com a opção *RegisterAllProvidersIP* definida como 0, o que significa que o failover de sub-rede cruzada do AoAG do SQL Server não tem suporte atualmente.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Instale o **SQL Server 2016 Standard/Enterprise Edition**

1. Inicie o **PowerShell** como um administrador de domínio.

2. Altere para o diretório em que o programa de instalação do SQL Server está localizado.

3. Digite os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Mais informações sobre serviços e contas de implantação de SQL podem ser encontradas [aqui](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)

> [!NOTE]
> O SSMS não está mais incluído no SQL 2016. Os detalhes do download podem ser encontrados [aqui](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

> [!div class="step-by-step"]  
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
