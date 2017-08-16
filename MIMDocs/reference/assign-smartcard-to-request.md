---
title: "Atribuir cartão inteligente a uma solicitação | Microsoft Docs"
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Atribuir cartão inteligente a uma solicitação
Associa o cartão inteligente especificado à solicitação especificada. Uma vez associado, a solicitação só poderá ser executada com esse cartão.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###<a name="url-parameters"></a>Parâmetros de URL
nenhum.
###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
requestid | A ID da solicitação à qual o cartão inteligente deve estar associado.
cardid | A cardid do cartão inteligente.
atr | A cadeia de ATR (resposta para redefinição) do cartão inteligente.


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
201     | Criado
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="response-body"></a>Corpo da Resposta
Em caso de êxito, retorna um URI para o objeto do cartão inteligente recém-criado.
##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Resposta
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Consulte também

- [Método Microsoft.Clm.Provision.RequestOperations.CreateSmartcard (Cadeia de caracteres, Cadeia de caracteres, Solicitação)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
