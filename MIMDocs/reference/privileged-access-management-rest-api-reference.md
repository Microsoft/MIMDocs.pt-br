---
title: "Referência da API REST do PAM (Privileged Access Management) | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Referência da API REST do PAM (Gerenciamento de Acesso Privilegiado)
O MIM (Microsoft Identity Manager) 2016 adiciona um novo cenário chamado PAM (Gerenciamento de Acesso Privilegiado). O PAM permite que uma organização tenha mais controle sobre os direitos de acesso de contas de usuário com privilégios altos, como administradores de sistema ou serviço, a recursos confidenciais. O PAM controla o acesso da conta de privilégios altos ao fornecer direitos de acesso por tempo limitado, JIT (just-in-time), quando os direitos de acesso são necessários.

Um usuário pode solicitar ao serviço MIM direitos de acesso com privilégios (elevação) de duas maneiras:

- Usando a API REST do PAM
- Usando o cmdlet New-PAMRequest do PowerShell do PAM

Os tópicos neste guia descrevem a API REST do PAM. Para obter mais informações sobre como usar o cmdlet do PowerShell, consulte o guia do laboratório de teste: Demonstrando o Gerenciamento de Acesso Privilegiado, usando o Microsoft Identity Manager, disponível no site do Connect.

##<a name="pam-rest-api-resources-and-operations"></a>Recursos e operações da API REST do PAM
A API REST do PAM funciona com os seguintes recursos
- **Função do PAM**: Uma função de PAM associa uma coleção de usuários com um conjunto de direitos de acesso. Os direitos de acesso são definidos por referência a grupos de segurança.  Cada função do PAM tem uma lista de contas de usuário, chamadas de candidatos, que estão qualificadas para elevar à função do PAM. É possível realizar as seguintes operações em funções do PAM:

    - [Obter funções do PAM](privileged-access-management-get-roles.md)

- **Solicitação do PAM**: Um usuário que deseja elevar os direitos de acesso da função do PAM deve enviar uma solicitação do PAM e obter a aprovação da solicitação para elevar. O objeto de Solicitação do PAM controla o ciclo de vida dessa solicitação no serviço do MIM. É possível realizar as seguintes operações em solicitações do PAM:

    - [Criar solicitação do PAM](privileged-access-management-create-request.md)
    - [Obter solicitações do PAM](privileged-access-management-get-requests.md)
    - [Encerrar solicitação do PAM](privileged-access-management-close-request.md)

- **Solicitação PAM pendente**: Usado para aprovar ou rejeitar solicitações do PAM que tenham sido enviadas pelos usuários. É possível realizar as seguintes operações em solicitações do PAM pendentes:

    - [Obter solicitações do PAM pendentes](privileged-access-management-get-pending-requests.md)
    - [Aprovar ou rejeitar uma solicitação pendente do PAM](privileged-access-management-approve-reject-pending-request.md)

- **Sessão do PAM**: Ao usar a API REST do PAM, o cliente (por exemplo, um navegador da Web) tem uma sessão com o ponto de extremidade da API REST do PAM. Nessa sessão, o cliente é autenticado para o ponto de extremidade da API REST. É possível realizar as seguintes operações em sessões do PAM:

     - [Obter informações de sessão do PAM](privileged-access-management-get-session-info.md)

Para obter informações mais detalhadas sobre o serviço, consulte [Detalhes do serviço da API REST do PAM](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>Portal de exemplo do PAM no GitHub
É uma maneira de aprender a usar a API REST do PAM usando o portal de exemplo do PAM, um aplicativo Web de exemplo que usa a API. Você pode encontrar o código para o portal de exemplo do PAM no [repositório de exemplo do PAM no GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Você pode aprender a implantar o portal de exemplo no Guia de laboratório de teste do PAM.
