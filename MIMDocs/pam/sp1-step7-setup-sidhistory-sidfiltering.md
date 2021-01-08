---
title: Etapa 7 Configurar o histórico de SID/filtragem de SID
description: Etapa 7 da configuração de Microsoft Identity Manager usando scripts. Essa etapa aborda a configuração de filtragem e histórico do SID.
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
ms.openlocfilehash: c09649bbecfb4608391fef1cda3d8da87ded888b
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010635"
---
# <a name="step-7-setup-sid-historysid-filtering"></a>Etapa 7 Configurar o histórico de SID/filtragem de SID

> [!div class="step-by-step"]
> [«Etapa 6](sp1-step6-setup-pam-trust.md)
> [Etapa 8»](sp1-step8-pam-deployment-verification.md)

**Os comandos a seguir não são necessários para um ambiente somente priv** Faça logon no PAMServer com a conta MIMAdmin.

1. Faça logon no controlador de domínio CORP como administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. selecione a opção 8 do menu (Configurar o histórico de SID/filtragem de SID)

Após a execução bem-sucedida, as mensagens a seguir serão exibidas:<br/></br>
Para filtragem de SID: <br/></br>
"Definindo a relação de confiança para não filtrar os SIDs." ou "A filtragem de SID não está habilitada para esta relação de confiança". </br></br>
Para o histórico de SID: </br></br>
“Habilitando histórico de SID para essa relação de confiança” ou “O histórico de SID já está habilitado para essa relação de confiança”.

> [!div class="step-by-step"]
> [«Etapa 6](sp1-step6-setup-pam-trust.md)
> [Etapa 8»](sp1-step8-pam-deployment-verification.md)
