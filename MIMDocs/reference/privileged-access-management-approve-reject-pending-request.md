---
title: Aprovar ou rejeitar uma solicitação pendente do PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 743c28b343c374d3406600751b379e8a4695c2d0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757458"
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Aprovar ou rejeitar uma solicitação pendente do PAM
Usado por uma conta com privilégios para aprovar, fechar ou rejeitar uma solicitação para elevar a uma função de PAM.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
----------|-----------
approvalId | O identificador (GUID) do objeto de aprovação no PAM, especificado como `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
v | Opcional. A versão da API. Se não estiver incluído, a versão atual (lançada mais recentemente) da API será usada. Para obter mais informações, consulte [controle de versão em detalhes do serviço da API REST do Pam](privileged-access-management-rest-api-service-details.md#versioning).


### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) em *detalhes do serviço da API REST do Pam* .

### <a name="request-body"></a>Corpo da solicitação
nenhuma.

## <a name="response"></a>Resposta
Esta seção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
401 | Não Autorizado
403 | Proibido
408 | Tempo Limite da Solicitação   
500 | Erro interno do servidor
503 | Serviço indisponível

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) em *detalhes do serviço da API REST do Pam* .

### <a name="response-body"></a>Corpo da resposta
Nenhum.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para aprovar uma solicitação para elevar a uma função do PAM.

### <a name="example-request"></a>Exemplo: solicitação

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK
```       
