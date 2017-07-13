---
title: "Etapa 2 Configurando o domínio CORP"
description: "Esse artigo descreve a segunda etapa necessária para configurar o domínio CORP, que envolve a execução de um script, após copiar sids.txt para o CORPDC"
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
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# Etapa 2 Configurando o domínio CORP
<a id="step-2-configuring-the-corp-domain" class="xliff"></a>

>[!div class="step-by-step"]
[« Etapa 1](sp1-step1-configuring-priv-domain.md)
[Etapa 3 »](sp1-step3-installing-configuring-sql.md)

Após o SIDs.txt ser copiado para CORPDC **não é necessário para implantações PRIVOnly**

1. Faça logon em CORPDC como Administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. selecione a Opção 2 do Menu (Configuração de Floresta CORP)

>[!div class="step-by-step"]
[« Etapa 1](sp1-step1-configuring-priv-domain.md)
[Etapa 3 »](sp1-step3-installing-configuring-sql.md)
