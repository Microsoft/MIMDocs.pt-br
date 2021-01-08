---
title: Etapa 5 Instalar/configurar o PAM
description: Esta é a etapa 5 de configurar Microsoft Identity Manager usando scripts e abrange as etapas de implantação no servidor do PAM.
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
ms.openlocfilehash: 01fe9cd7704674f408e0b9b5a27673989d0eaecf
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010578"
---
# <a name="step-5-installingconfiguring-pam"></a>Etapa 5 Instalar/configurar o PAM

> [!div class="step-by-step"]
> [« Etapa 4](sp1-step4-configuring-sharepoint.md)
> [Etapa 6 »](sp1-step6-setup-pam-trust.md)

Para um PAMServer ingressado no domínio, faça logon como MIMAdmin, caso contrário, faça logon como um administrador local.
1. Execute o PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Selecione a opção 5 do menu (Configuração do PAM do MIM)

>[!NOTE]
>Se o computador ainda não estiver ingressado em domínio no PRIV, ele solicitará as credenciais. Depois de ingressar no domínio, o computador será reinicializado.

Após a reinicialização do PAMServer, faça logon novamente no computador com a conta MIMAdmin.

1. Execute o PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a opção 5 do menu (Configuração do PAM do MIM)

   Quando receber a solicitação, digite a senha da Conta de Monitor do MIM, da Conta do Componente do MIM, da Conta de MA do MIM, da Conta de Serviço de MIM, da Conta de Administrador do MIM e da Conta do SharePoint.
   Após a conclusão da instalação, o computador será reiniciado.

> [!div class="step-by-step"]
> [« Etapa 4](sp1-step4-configuring-sharepoint.md)
> [Etapa 6 »](sp1-step6-setup-pam-trust.md)
