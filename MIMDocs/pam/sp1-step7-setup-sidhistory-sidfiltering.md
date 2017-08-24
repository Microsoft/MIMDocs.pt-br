---
title: "Etapa 7 Configurar o histórico de SID/filtragem de SID"
description: "Esta é a Etapa 7 da configuração do Privileged Identity Manager com scripts. Essa etapa aborda a configuração de filtragem e histórico do SID."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: e3d8578e9d58ccc2f3dc3bb4ff437d124bf15291
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2017
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>Etapa 7 Configurar o histórico de SID/filtragem de SID

>[!div class="step-by-step"]
[«Etapa 6](sp1-step6-setup-pam-trust.md)
[Etapa 8»](sp1-step8-pam-deployment-verification.md)

**Isso não é necessário para um ambiente somente PRIV** Faça logon no PAMServer com a conta MIMAdmin.

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

>[!div class="step-by-step"]
[«Etapa 6](sp1-step6-setup-pam-trust.md)
[Etapa 8»](sp1-step8-pam-deployment-verification.md)
