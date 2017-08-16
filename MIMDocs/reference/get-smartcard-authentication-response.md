---
title: "Obter resposta de autenticação de cartão inteligente | Microsoft Docs"
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>Obter resposta de autenticação de cartão inteligente
Obtém a resposta ao desafio de autenticação do CSP Básico.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
reqid | Obrigatório. O identificador da solicitação (específico para MIM CM).
scid | Obrigatório. O identificador do cartão inteligente (específico para o MIM CM). Obtido por meio do objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx).
###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
---------|------------
atr | Opcional. A cadeia de ATR (resposta para redefinição) do cartão inteligente.
cardid | Obrigatório. A id do cartão.
desafio | Obrigatório. Uma cadeia de caracteres codificada para base 64 representando o desafio emitido pelo cartão inteligente.
diversificado | Obrigatório. Um sinalizador booliano indicando se a chave de admin do cartão inteligente foi diversificada.


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
Em caso de sucesso, retorna um BLOB de byte que representa a resposta do desafio.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>Consulte também

- [Método Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
