---
title: "Obter o PIN proposto do cartão inteligente | Microsoft Docs"
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Obter o PIN proposto do cartão inteligente
Obtém o PIN do usuário gerado pelo servidor.

**Observação**: O servidor definirá o PIN somente se a política de modelo de perfil indicar que isso deve ser feito. Caso contrário, o usuário deve fornecer.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpem

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
id | O identificador do cartão inteligente (específico para o MIM CM). Obtido por meio do objeto Microsft.Clm.Shared.Smartcard.
###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
---------|------------
atr | A atr do cartão.
cardid | A id do cartão.
desafio | Uma cadeia de caracteres codificada para base 64 representando o desafio emitido pelo cartão inteligente.

###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
nenhuma

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="response-body"></a>Corpo da Resposta
Em caso de êxito, retorna uma cadeia de caracteres que representa o PIN proposto pelo servidor.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

... body coming soon
```       
