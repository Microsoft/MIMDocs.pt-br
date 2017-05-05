---
title: Modelo de camada do ambiente do PAM | Microsoft Docs
description: Saiba mais sobre o modelo de camada que separa o seu sistema com base na vulnerabilidade a riscos.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 4c3b43e50403890572e77773191a821cf247269c
ms.lasthandoff: 05/02/2017


---

# <a name="tier-model-for-partitioning-administrative-privileges"></a>Modelo de camada para o particionamento de privilégios administrativos

No atual ambiente de ameaças, a grande questão não é se um invasor terá acesso a seus sistemas, mas quando. Isso significa que a segurança interna é tão importante quanto uma forte defesa do perímetro. Este artigo descreve um modelo de segurança que se destina a proteger contra a elevação de privilégio, segregando atividades de privilégio elevado provenientes de regiões de alto risco. Esse modelo fornece uma boa experiência do usuário, ao mesmo tempo que segue as melhores práticas e os princípios de segurança.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Elevação de privilégio em florestas do Active Directory

As contas de usuário, de serviços ou de aplicativos que receberam privilégios administrativos permanentes em florestas do AD (Active Directory) do Windows Server apresentam uma quantidade significativa de risco à missão e aos negócios da organização. Em geral, essas contas são alvo de invasores, pois, se forem comprometidas, o invasor terá privilégio de se conectar a outros servidores ou aplicativos no domínio.

O modelo de camada cria divisões entre os administradores com base nos recursos que eles gerenciam. Os administradores com controle das estações de trabalho do usuário são separados daqueles que controlam aplicativos ou que gerenciam identidades corporativas. Saiba mais sobre esse modelo em [Securing privileged access reference material](http://aka.ms/tiermodel) (Protegendo materiais de referência de acesso privilegiado).

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

O próximo artigo, [Planning a bastion environment](planning-bastion-environment.md) (Planejando um ambiente de bastiões), descreve como adicionar uma floresta administrativa dedicada para que o Microsoft Identity Manager estabeleça as contas administrativas.

