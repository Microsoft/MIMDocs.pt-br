---
title: Etapa 2 Configurando o domínio CORP
description: Esse artigo descreve a segunda etapa necessária para configurar o domínio CORP, que envolve a execução de um script, após copiar sids.txt para o CORPDC
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
ms.openlocfilehash: 1215e0be4d978e879ebc09ecdd99223fb3667a85
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043827"
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
