---
title: "Obter solicitações do PAM | Microsoft Docs"
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>Obter solicitações do PAM
Usado por uma conta com privilégios para retornar um histórico de solicitações do PAM lançadas anteriormente.

**Observação**: URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades da Solicitação PAM em uma expressão de filtro para retornar uma lista filtrada de respostas. Para saber mais sobre operadores com suporte, confira a seção [Filtragem, no artigo Detalhes do serviço da API REST do PAM](privileged-access-management-rest-api-service-details.md#filtering)
v | Opcional. A versão da API. Se não estiver incluído, a versão atual (mais recentemente lançada) da API será usada. Para saber mais, confira [Detalhes do serviço de controle de versão da API REST do PAM](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do PAM*.
###<a name="request-body"></a>Corpo da solicitação
nenhuma

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200 | OK
401 | Não autorizado
403 | Proibido
408 | Tempo Limite da Solicitação   
500 | Erro Interno do Servidor
503 | Serviço Indisponível

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do PAM*.
###<a name="response-body"></a>Corpo da Resposta
Uma resposta bem-sucedida contém uma lista de objetos de solicitação do PAM com as seguintes propriedades.

Propriedade | Descrição
--------|-------------
SolicitaçãoID | O identificador exclusivo (GUID) para a solicitação de PAM.
CreatorID | Um identificador exclusivo (GUID) para a conta do Active Directory que criou a solicitação do PAM.
Justificação | O motivo para a elevação.
DisplayName | O nome de exibição da solicitação PAM em MIM.
CreationTime | A hora de criação da solicitação.
CreationMétodo | O método usado para criar a solicitação.
ExpirationTime | A hora de expiração da solicitação.
RoleID| O identificador exclusivo (GUID) da função de PAM.
SolicitaçãoedTTL | O tempo limite de expiração solicitado em segundos.
RequestedTime | O tempo solicitado para elevação.
SolicitaçãoedStatus | O status da solicitação. Os valores possíveis são: "Ativo", "Fechado", "Fechando", "Expirado", "Aprovação pendente" e "Rejeitado".

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
