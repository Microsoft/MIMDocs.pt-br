---
title: Etapa 3 Configurar o SQL
description: Este artigo é a etapa 3 da série de artigos que abordam como configurar Microsoft Identity Manager usando scripts e discute as etapas de configuração do SQL Server.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e4317363084da53e5683cd1a9b3d7bd0ce3ebcda
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010516"
---
# <a name="step-3-configuring-sql"></a>Etapa 3 Configurar o SQL

> [!div class="step-by-step"]
> [« Etapa 2](sp1-step2-configuring-corp-domain.md)
> [Etapa 4 »](sp1-step4-configuring-sharepoint.md)

Antes de você prosseguir com as próximas etapas, certifique-se de que esteja usando o SQL Server 2012 SP1 ou posterior, ou o SQL server 2014. Para servidores ingressados no domínio, faça logon usando a conta MIMAdmin, caso contrário, faça logon como administrador local.
1. Execute o PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a opção 3 do Menu (Configuração do SQL Server)

   Se o servidor ainda não estiver ingressado no domínio PRIV, ele solicitará as credenciais e ingressará o servidor no domínio.
   Depois de ingressar no domínio, o computador será reinicializado. Após a reinicialização bem-sucedida, faça logon no servidor com a conta MIMAdmin.

5. Execute o PowerShell como administrador
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Selecione a opção 3 do Menu (Configuração do SQL Server)

Quando solicitado, forneça a senha para a conta de serviço MIMAdmin e permita que a instalação continue. Após a conclusão, vá para a etapa 4.

> [!div class="step-by-step"]
> [« Etapa 2](sp1-step2-configuring-corp-domain.md)
> [Etapa 4 »](sp1-step4-configuring-sharepoint.md)
