---
title: Referência da API REST do Privileged Access Management | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8a318ae5f12fd49e2ee949d81aefd86a7221e120
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757449"
---
# <a name="privileged-access-management-rest-api-reference"></a>Referência da API REST do Privileged Access Management
O MIM (Microsoft Identity Manager) 2016 adiciona um novo cenário chamado PAM (Gerenciamento de Acesso Privilegiado). O PAM permite que uma organização tenha mais controle sobre os direitos de acesso de contas de usuário com privilégios altos, como administradores de sistema ou serviço, a recursos confidenciais. O PAM controla o acesso da conta de privilégios altos ao fornecer direitos de acesso por tempo limitado, JIT (just-in-time), quando os direitos de acesso são necessários.

Um usuário pode solicitar ao serviço MIM direitos de acesso com privilégios (elevação) de duas maneiras:

- Usando a API REST do PAM.
- Usando o cmdlet New-PAMRequest do PowerShell do PAM.

Os tópicos neste guia descrevem a API REST do PAM. Para obter mais informações sobre como usar o cmdlet do PowerShell, consulte _o guia de laboratório de teste: demonstrando Privileged Access Management usando Microsoft Identity Manager_ , disponível no site do Connect.

## <a name="pam-rest-api-resources-and-operations"></a>Recursos e operações da API REST do PAM
A API REST do PAM opera nos seguintes recursos:
- **Função do Pam** : uma função do Pam associa uma coleção de usuários a uma coleção de direitos de acesso. Os direitos de acesso são definidos por referência a grupos de segurança.  Cada função do PAM tem uma lista de contas de usuário, chamadas de candidatos, que estão qualificadas para elevar à função do PAM. É possível realizar as seguintes operações em funções do PAM:

    - [Obter funções do PAM](privileged-access-management-get-roles.md)

- **Solicitação do Pam** : um usuário que deseja elevar os direitos de acesso da função do Pam precisa enviar uma solicitação do Pam e obter aprovação para a solicitação ser elevada. O objeto de Solicitação do PAM controla o ciclo de vida dessa solicitação no serviço do MIM. É possível realizar as seguintes operações em solicitações do PAM:

    - [Criar solicitação do PAM](privileged-access-management-create-request.md)
    - [Obter solicitações do PAM](privileged-access-management-get-requests.md)
    - [Encerrar solicitação do PAM](privileged-access-management-close-request.md)

- **Solicitação do Pam pendente** : usada para aprovar ou rejeitar solicitações do Pam que foram enviadas pelos usuários. É possível realizar as seguintes operações em solicitações do PAM pendentes:

    - [Obter solicitações do PAM pendentes](privileged-access-management-get-pending-requests.md)
    - [Aprovar ou rejeitar uma solicitação pendente do PAM](privileged-access-management-approve-reject-pending-request.md)

- **Sessão do Pam** : ao usar a API REST do Pam, o cliente (por exemplo, um navegador da Web) tem uma sessão com o ponto de extremidade da API REST do Pam. Nesta sessão, o cliente é autenticado para o ponto de extremidade da API REST. É possível realizar as seguintes operações em sessões do PAM:

     - [Obter informações de sessão do PAM](privileged-access-management-get-session-info.md)

Para obter informações mais detalhadas sobre o serviço, consulte [detalhes do serviço da API REST do Pam](privileged-access-management-rest-api-service-details.md).

## <a name="pam-sample-portal-on-github"></a>Portal de exemplo do PAM no GitHub
Uma maneira de aprender a usar a API REST do PAM é usando o portal de exemplo do PAM, um aplicativo Web de exemplo que usa a API. Você pode encontrar o código para o portal de exemplo do PAM no [repositório de exemplo do PAM no GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Você pode aprender a implantar o portal de exemplo no Guia de laboratório de teste do PAM.
