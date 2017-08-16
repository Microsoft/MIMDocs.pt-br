---
title: "Obter Funções do PAM | Microsoft Docs"
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Obter funções do PAM
Usado por uma conta com privilégios para listar as funções de PAM para as quais a conta é um candidato.

**Observação**: URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/api/pamresources/pamroles

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades da Função PAM em uma expressão de filtro para retornar uma lista filtrada de respostas. Para saber mais sobre operadores com suporte, confira a seção [Filtragem, no artigo Detalhes do serviço da API REST do PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Uma resposta bem-sucedida contém uma coleção de uma ou mais funções de PAM, cada uma com as seguintes propriedades.

Propriedade | Descrição
--------|-------------
RoleID | O identificador exclusivo (GUID) da função de PAM.
DisplayName | O nome de exibição da função PAM no serviço MIM.
Descrição | Descrição da função PAM no serviço MIM.
TTL | Tempo limite máximo de expiração de direitos de acesso da função em segundos.
AvailableFrom | A primeira hora do dia em que uma solicitação será ativada.
AvailableTo | A última hora do dia em que uma solicitação será ativada.
MFAEnabled | Um valor booliano que indica se as solicitações de ativação para essa função precisam de um desafio de MFA.
ApprovalEnabled | Um valor booliano que indica se as solicitações de ativação para essa função exigem aprovação por um proprietário de função.
AvailibitlyWemdowEnabled | Um valor booliano que indica se a função só pode ser ativada durante um intervalo de tempo especificado.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Resposta
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
