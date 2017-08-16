---
title: "Criar solicitação do PAM | Microsoft Docs"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Criar solicitação do PAM
Usado por uma conta com privilégios para elevar a uma função do PAM.

**Observação**: URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
--------|-------------
Justificação | Opcional. O motivo para a solicitação de elevação fornecido pelo usuário.
RoleId| Obrigatório. O identificador exclusivo (GUID) da função do PAM a elevar.
RequestedTTL| Obrigatório. O tempo de expiração solicitado em segundos.
SolicitaçãoedTime | Opcional. O tempo para elevar os privilégios.  
v | Opcional. A versão da API. Se não estiver incluído, a versão atual (mais recentemente lançada) da API será usada. Para saber mais, confira [Detalhes do serviço de controle de versão da API REST do PAM](privileged-access-management-rest-api-service-details.md#versioning)

**Observação**: Você pode especificar os parâmetros *Justification*, *RoleId*, *RequestedTTL*e *RequestedTime* como propriedades no corpo da solicitação, em vez de como parâmetros de consulta. O parâmetro *v* só pode ser especificado como um parâmetro de consulta.

###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do PAM*.
###<a name="request-body"></a>Corpo da solicitação
Opcional. Conforme observado acima, os parâmetros *Justification*, *RoleId*, *RequestedTTL*e *RequestedTime* podem ser especificados como propriedades de um corpo de solicitação, em vez de como uma cadeia de caracteres de consulta da URL.

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
Uma resposta bem-sucedida contém um objeto de solicitação do PAM com as seguintes propriedades.

Propriedade | Descrição
--------|-------------
SolicitaçãoID | O identificador exclusivo (GUID) para a solicitação de PAM.
CreatorID | O identificador exclusivo (GUID) no serviço do MIM para a conta que criou a solicitação.
Justificação | O motivo para a elevação.
CreationTime | A hora de criação da solicitação.
CreationMétodo | O método usado para criar a solicitação.
ExpirationTime | A hora de expiração da solicitação.
RoleID| O identificador exclusivo (GUID) da função de PAM.
SolicitaçãoedTTL | O tempo limite de expiração solicitado em segundos.
RequestedTime | O tempo solicitado para elevação.
RequestStatus | O status da solicitação. Os valores possíveis são: "Processamento", "Ativo", "Fechado", "Fechamento", "Expirado", "PendingApproval", "PendingMFA" e "Rejeitado".

##<a name="example"></a>Exemplo

###<a name="request-1"></a>Solicitação 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Solicitação 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Resposta 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
