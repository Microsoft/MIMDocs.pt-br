---
title: Obter solicitações do PAM pendentes | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0ec4388ce5d5f748d9f779819d0a2d0bdec06791
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757457"
---
# <a name="get-pending-pam-requests"></a>Obter solicitações do PAM pendentes
Usado por uma conta com privilégios para retornar uma lista de solicitações pendentes que precisam de aprovação.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades de solicitação do PAM pendentes em uma expressão de filtro para retornar uma lista filtrada de respostas. Para obter mais informações sobre operadores com suporte, consulte [filtragem em detalhes do serviço da API REST do Pam](privileged-access-management-rest-api-service-details.md#filtering).
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
Uma resposta bem-sucedida contém uma lista de objetos de aprovação de solicitação do PAM com as seguintes propriedades:

Propriedade | Descrição
---------|-------------
RoleName | O nome de exibição da função para o qual é necessária a aprovação.
Solicitante | O nome de usuário do solicitante a ser aprovado.
Justificativa | A justificativa fornecida pelo usuário.
RequestedTTL | O tempo de expiração solicitado em segundos.
RequestedTime | O tempo solicitado para elevação.
CreationTime | A hora de criação da solicitação.
FIMRequestID | Contém um único elemento, "value", com o identificador exclusivo (GUID) da solicitação do PAM.
RequestorID | Contém um único elemento, "value", com o identificador exclusivo (GUID) para a conta de Active Directory que criou a solicitação do PAM.
ApprovalObjectID | Contém um único elemento, "value", com o identificador exclusivo (GUID) para o objeto de aprovação.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter as solicitações pendentes do PAM.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
