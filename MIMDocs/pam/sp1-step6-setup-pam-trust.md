---
title: "Etapa 6 Configurar a relação de confiança do PAM"
description: "Etapa 6 da configuração do PAM usando scripts. Esta seção aborda as configurações de confiança necessárias entre os domínios CORP e PRIV"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 3b232dfa515b42fd42a5606d1beff9d3fe50389c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="step-6-set-up-the-pam-trust"></a>Etapa 6 Configurar a relação de confiança do PAM

>[!div class="step-by-step"]
[«Etapa 5](sp1-step5-configuring-pam.md)
[Etapa 7»](sp1-step7-setup-sidhistory-sidfiltering.md)

**Isso não é necessário para um ambiente somente PRIV** Faça logon no PAMServer com a conta MIMAdmin.

1. Faça logon em PAMServer com a conta MIMAdmin
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 6 do Menu (Configuração de confiança do PAM)

  Quando receber uma solicitação, insira as credenciais para a conta de administrador CORP. Depois de fornecer as credenciais, a relação de confiança será estabelecida e a configuração estará concluída.

>[!div class="step-by-step"]
[«Etapa 5](sp1-step5-configuring-pam.md)
[Etapa 7»](sp1-step7-setup-sidhistory-sidfiltering.md)
