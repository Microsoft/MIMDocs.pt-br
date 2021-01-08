---
title: Modelo de camada do ambiente do PAM | Microsoft Docs
description: Saiba mais sobre o modelo de camada que separa o seu sistema com base na vulnerabilidade a riscos.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 25bf28731f14331da386ae9f59b041c8c9ee33a1
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010567"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Modelo de camada para o particionamento de privilégios administrativos

Este artigo descreve um modelo de segurança que se destina a proteger contra a elevação de privilégio, segregando atividades de privilégio elevado provenientes de regiões de alto risco.

> [!IMPORTANT]
> O modelo neste artigo destina-se somente a ambientes de Active Directory isoladas usando o PAM do MIM.  Para ambientes híbridos, consulte em vez disso as diretrizes no [modelo de acesso empresarial](/security/compass/privileged-access-access-model).

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Elevação de privilégio em florestas do Active Directory

As contas de usuário, de serviços ou de aplicativos que receberam privilégios administrativos permanentes em florestas do AD (Active Directory) do Windows Server apresentam uma quantidade significativa de risco à missão e aos negócios da organização. Em geral, essas contas são alvo de invasores, pois, se forem comprometidas, o invasor terá direito de se conectar a outros servidores ou aplicativos no domínio.

O modelo de camada cria divisões entre os administradores com base nos recursos que eles gerenciam. Os administradores com controle das estações de trabalho do usuário são separados daqueles que controlam aplicativos ou que gerenciam identidades corporativas.

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Restringindo a exposição de credencial com restrições de logon

Normalmente, a redução do risco de roubo de credenciais de contas administrativas exige a reformulação das práticas administrativas para limitar a exposição aos invasores. Como uma primeira etapa, as organizações são aconselhadas a:

- Limite o número de hosts nos quais as credenciais administrativas são expostas.
- Limite os privilégios de função ao mínimo necessário.
- Certifique-se de que tarefas administrativas não são executadas nos hosts usados para atividades de usuário padrão (por exemplo, email e navegação na Web).

A próxima etapa é implementar restrições de logon e permitir que os processos e as práticas atendam aos requisitos do modelo da camada. O ideal é que a exposição de credenciais também seja reduzida para o privilégio mínimo necessário para a função em cada camada.

Restrições de logon devem ser impostas a fim de garantir que contas altamente privilegiadas não tenham acesso a recursos menos seguros. Por exemplo:

- Os administradores de domínio (camada 0) não pode fazer logon nos servidores corporativos (camada 1) e nas estações de trabalho do usuário padrão (camada 2).
- Os administradores de servidor (camada 1) não podem fazer logon nas estações de trabalho do usuário padrão (camada 2).

>[!NOTE]
> Os administradores do servidor não devem estar no grupo de administradores de domínio. A equipe com a responsabilidade de gerenciar controladores de domínio e servidores corporativos deve receber contas separadas.

Restrições de logon podem ser aplicadas com:

- Restrições de Direitos do Logon de Política de Grupo, incluindo:
    - Negar acesso a este computador pela rede
    - Negar logon como um trabalho em lotes
    - Negar logon como serviço
    - Negar logon localmente
    - Negar logon por meio das configurações da Área de Trabalho Remota  
- Políticas de autenticação e silos, se estiver usando o Windows Server 2012 ou posterior
- Autenticação seletiva, se a conta estiver em uma floresta de administrador dedicada

## <a name="next-steps"></a>Próximas etapas

- O próximo artigo, [Planning a bastion environment](planning-bastion-environment.md) (Planejando um ambiente de bastiões), descreve como adicionar uma floresta administrativa dedicada para que o Microsoft Identity Manager estabeleça as contas administrativas.
- A [proteção de dispositivos](/security/compass/concept-azure-managed-workstation) fornece um sistema operacional dedicado para tarefas confidenciais protegidas contra ataques da Internet e vetores de ameaça.
