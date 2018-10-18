---
title: Etapa 2 Configurando o domínio CORP
description: Esse artigo descreve a segunda etapa necessária para configurar o domínio CORP, que envolve a execução de um script, após copiar sids.txt para o CORPDC
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
ms.openlocfilehash: b703dcfb4a4d59704fd4c33f4b891df54f9cdf77
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332724"
---
# <a name="step-2-configuring-the-corp-domain"></a>Etapa 2 Configurando o domínio CORP

> [!div class="step-by-step"]
> [« Etapa 1](sp1-step1-configuring-priv-domain.md)
> [Etapa 3 »](sp1-step3-installing-configuring-sql.md)

Após o SIDs.txt ser copiado para CORPDC **não é necessário para implantações PRIVOnly**

1. Faça logon em CORPDC como Administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. selecione a Opção 2 do Menu (Configuração de Floresta CORP)

> [!div class="step-by-step"]
> [« Etapa 1](sp1-step1-configuring-priv-domain.md)
> [Etapa 3 »](sp1-step3-installing-configuring-sql.md)
