---
title: "Fechar solicitação do PAM | Microsoft Docs"
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>Encerrar solicitação do PAM
Usado por uma conta com privilégios para fechar uma solicitação que ela iniciou para elevar a uma função de PAM.

**Observação**: URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
----------|-----------
requestId | O identificador (GUID) da solicitação PAM a fechar, especificado da seguinte maneira: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
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
nenhuma
##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

```       
