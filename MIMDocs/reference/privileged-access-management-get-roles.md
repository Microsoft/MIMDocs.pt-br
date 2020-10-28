---
title: Obter funções do PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f802dc22019f1ad97fc5cce8c9ed2e000349dc58
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757477"
---
# <a name="get-pam-roles"></a>Obter funções do PAM
Usado por uma conta com privilégios para listar as funções de PAM para as quais a conta é um candidato.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/api/pamresources/pamroles

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades de função do PAM em uma expressão de filtro para retornar uma lista filtrada de respostas. Para obter mais informações sobre operadores com suporte, consulte [filtragem em detalhes do serviço da API REST do Pam](privileged-access-management-rest-api-service-details.md#filtering).
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
Uma resposta bem-sucedida contém uma coleção de uma ou mais funções do PAM, cada uma com as seguintes propriedades:

Propriedade | Descrição
--------|-------------
RoleID | O identificador exclusivo (GUID) da função de PAM.
DisplayName | O nome de exibição da função PAM no serviço MIM.
Descrição | Descrição da função PAM no serviço MIM.
TTL | Tempo limite máximo de expiração de direitos de acesso da função em segundos.
AvailableFrom | A hora mais antiga do dia em que uma solicitação é ativada.
AvailableTo | A última hora do dia em que uma solicitação é ativada.
MFAEnabled | Um valor booliano que indica se as solicitações de ativação para essa função precisam de um desafio de MFA.
ApprovalEnabled | Um valor booliano que indica se as solicitações de ativação para essa função exigem aprovação por um proprietário de função.
AvailabilityWindowEnabled | Um valor booliano que indica se a função só pode ser ativada durante um intervalo de tempo especificado.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter as funções do PAM.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /api/pamresources/pamroles HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
