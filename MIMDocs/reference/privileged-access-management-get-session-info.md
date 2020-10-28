---
title: Obter informações da sessão do PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2529ac9665344403f798cd2bf708dffb8697876a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757452"
---
# <a name="get-pam-session-info"></a>Obter informações da sessão do PAM
Usado por uma conta com privilégios para obter o nome de usuário da conta que está conectada à sessão.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/api/session/sessioninfo

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
Uma resposta bem-sucedida retorna um objeto de sessão do PAM com as seguintes propriedades:

Propriedade | Descrição
--------|-------------
Nome de Usuário | O nome de usuário da conta que está conectada a essa sessão.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter as informações da sessão do PAM.

### <a name="example-request"></a>Exemplo: solicitação 

```
GET /api/session/sessioninfo/ HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
