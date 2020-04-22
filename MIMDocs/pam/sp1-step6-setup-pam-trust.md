---
title: Etapa 6 Configurar a relação de confiança do PAM
description: Etapa 6 da configuração do PAM usando scripts. Esta seção aborda as configurações de confiança necessárias entre os domínios CORP e PRIV
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
ms.openlocfilehash: baab111f1b8f0f290611b9b63bb6981d8ae9538a
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043759"
---
# <a name="step-6-set-up-the-pam-trust"></a>Etapa 6 Configurar a relação de confiança do PAM

> [!div class="step-by-step"]
> [«Etapa 5](sp1-step5-configuring-pam.md)
> [Etapa 7»](sp1-step7-setup-sidhistory-sidfiltering.md)

**Isso não é necessário para um ambiente somente PRIV** Faça logon no PAMServer com a conta MIMAdmin.

1. Faça logon em PAMServer com a conta MIMAdmin
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 6 do Menu (Configuração de confiança do PAM)

   Quando receber uma solicitação, insira as credenciais para a conta de administrador CORP. Depois de fornecer as credenciais, a relação de confiança será estabelecida e a configuração estará concluída.

> [!div class="step-by-step"]
> [«Etapa 5](sp1-step5-configuring-pam.md)
> [Etapa 7»](sp1-step7-setup-sidhistory-sidfiltering.md)
