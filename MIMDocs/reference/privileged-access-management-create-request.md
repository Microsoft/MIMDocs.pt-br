---
title: Criar solicitação do PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58155232b3f99f32df9cb6b4c2e344d0cbad278
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757456"
---
# <a name="create-pam-request"></a>Criar solicitação do PAM
Usado por uma conta com privilégios para elevar a uma função do PAM.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
POST     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
--------|-------------
Justificativa | Opcional. O motivo para a solicitação de elevação fornecido pelo usuário.
RoleId | Necessário. O identificador exclusivo (GUID) da função do PAM a elevar.
RequestedTTL | Necessário. O tempo de expiração solicitado em segundos.
RequestedTime | Opcional. O tempo para elevar os privilégios.  
v | Opcional. A versão da API. Se não estiver incluído, a versão atual (lançada mais recentemente) da API será usada. Para obter mais informações, consulte [controle de versão em detalhes do serviço da API REST do Pam](privileged-access-management-rest-api-service-details.md#versioning).

>[!NOTE]
>Você pode especificar os parâmetros **Justification** , **RoleId** , **RequestedTTL** e **RequestedTime** como propriedades no corpo da solicitação, em vez de como parâmetros de consulta. O parâmetro **v** só pode ser especificado como um parâmetro de consulta.

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) em *detalhes do serviço da API REST do Pam* .

### <a name="request-body"></a>Corpo da solicitação
Opcional. Os parâmetros de **justificativa** , **RoleID** , **RequestedTTL** e **requestedstate** podem ser especificados como propriedades de um corpo de solicitação em vez de especificá-los na cadeia de caracteres de consulta de URL.

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
Uma resposta bem-sucedida contém um objeto de solicitação do PAM com as seguintes propriedades:

Propriedade | Descrição
--------|-------------
RequestID | O identificador exclusivo (GUID) para a solicitação de PAM.
CreatorID | O identificador exclusivo (GUID) no serviço do MIM para a conta que criou a solicitação.
Justificativa | O motivo para a elevação.
CreationTime | A hora de criação da solicitação.
CreationMethod | O método usado para criar a solicitação.
ExpirationTime | A hora de expiração da solicitação.
RoleID| O identificador exclusivo (GUID) da função de PAM.
RequestedTTL | O tempo limite de expiração solicitado em segundos.
RequestedTime | O tempo solicitado para elevação.
RequestStatus | O status da solicitação. Os valores possíveis são "processamento", "ativo", "fechado", "" fechando "," expirado "," PendingApproval "," "PendingMFA" e "rejeitado".

## <a name="example"></a>Exemplo
Esta seção fornece exemplos para criar uma solicitação do PAM.

### <a name="example-request-1"></a>Exemplo: solicitação 1

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: resposta 1

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

### <a name="example-request-2"></a>Exemplo: solicitação 2

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: resposta 2

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
