---
title: Etapa 3 Configurar o SQL
description: Esse artigo representa a etapa 3 da série de artigos que abordam como configurar o Privileged Identity Manager usando scripts e explica as etapas de configuração do servidor SQL.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: b4fdc645f867fd3c3a20d9737690cf29eadaf50e
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332996"
---
# <a name="step-3-configuring-sql"></a>Etapa 3 Configurar o SQL

> [!div class="step-by-step"]
> [« Etapa 2](sp1-step2-configuring-corp-domain.md)
> [Etapa 4 »](sp1-step4-configuring-sharepoint.md)

Antes de você prosseguir com as próximas etapas, certifique-se de que esteja usando o SQL Server 2012 SP1 ou posterior, ou o SQL server 2014. Para servidores ingressados em um domínio, faça logon usando a conta MIMAdmin, caso contrário, faça logon como administrador local
1. Execute o PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a opção 3 do Menu (Configuração do SQL Server)

   Se o servidor ainda não estiver ingressado no domínio PRIV, ele solicitará as credenciais e ingressará o servidor no domínio.
   Depois de ingressar no domínio, o computador será reinicializado. Após a reinicialização, faça logon no servidor com a conta MIMAdmin.

5. Execute o PowerShell como administrador
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Selecione a opção 3 do Menu (Configuração do SQL Server)

Quando solicitado, forneça a senha para a conta de serviço MIMAdmin e permita que a instalação continue. Após a conclusão, vá para a etapa 4.

> [!div class="step-by-step"]
> [« Etapa 2](sp1-step2-configuring-corp-domain.md)
> [Etapa 4 »](sp1-step4-configuring-sharepoint.md)
