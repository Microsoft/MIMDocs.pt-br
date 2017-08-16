---
title: "Obter solicitações pendentes do PAM | Microsoft Docs"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Obter solicitações do PAM pendentes
Usado por uma conta com privilégios para retornar uma lista de solicitações pendentes que precisam de aprovação.

**Observação**: URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades da Solicitação do PAM pendente em uma expressão de filtro para retornar uma lista filtrada de respostas. Para saber mais sobre operadores com suporte, confira a seção [Filtragem, no artigo Detalhes do serviço da API REST do PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Uma resposta bem-sucedida contém uma lista de objetos de aprovação de solicitação do PAM com as seguintes propriedades.

Propriedade | Descrição
---------|-------------
RoleName | O nome de exibição da função para o qual é necessária a aprovação.
Solicitante | O nome de usuário do solicitante a ser aprovado.
Justificação | A justificativa fornecida pelo usuário.
RequestedTTL | O tempo de expiração solicitado em segundos.
SolicitaçãoedTime | O tempo solicitado para elevação.
CreationTime | A hora de criação da solicitação.
FIMSolicitaçãoID | Contém um único elemento, "Valor", com o identificador exclusivo (GUID) da solicitação do PAM.
SolicitaçãoorID | Contém um único elemento, "Valor", com o identificador exclusivo (GUID) para a conta do Active Directory que criou a solicitação do PAM.
ApprovalObjectID | Contém um único elemento, "Valor", com o identificador exclusivo (GUID) para o objeto de aprovação.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Resposta
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
