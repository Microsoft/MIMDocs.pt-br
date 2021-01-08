---
title: Configurar o MIM 2016 para o Privileged Access Management | Microsoft Docs
description: O roteiro para instalação do MIM e configuração dele para o Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 64ed3184ee38102f488c61bf0b41bc0fec34b049
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010597"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Configure o ambiente do MIM para o Privileged Access Management

Há sete etapas a concluir ao configurar o ambiente para o acesso entre florestas, instalar e configurar o Active Directory e o Microsoft Identity Manager e demonstrar uma solicitação de acesso just-in-time.

Essas etapas são delineadas para que você possa começar do zero e criar um ambiente de teste. Se estiver aplicando o PAM a um ambiente existente, você poderá usar seus próprios controladores de domínio ou contas de usuário para o domínio *contoso* , em vez de criar novos para corresponder aos exemplos.

1. Prepare o servidor *CORPDC* como um controlador de domínio e *CORPWKSTN* como uma estação de trabalho do membro.

2. Prepare o servidor *PRIVDC* como um controlador de domínio.

3.  Prepare o servidor *PAMSRV* na floresta *PRIV* .

4.  Instale os componentes MIM em *PAMSRV* e os cmdlets em uma estação de trabalho do membro da floresta *CONTOSO* e prepare-os para o gerenciamento de acesso privilegiado.

5.  Estabeleça confiança entre as florestas *PRIV* e *CONTOSO* .

6.  Preparando grupos de segurança com privilégios com acesso a recursos protegidos e contas de membro para gerenciamento de acesso com privilégios just-in-time.

7.  Demonstre a solicitação, o recebimento e o uso de acesso com privilégios elevados a um recurso protegido.

> [!div class="step-by-step"]
> [Iniciar »](step-1-prepare-corp-domain.md)
