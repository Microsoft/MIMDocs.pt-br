---
title: "Obter dados de cartão inteligente | Microsoft Docs"
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>Obter dados de cartão inteligente
Obtém uma lista de perfis de cartão inteligente do usuário com uma lista de possíveis operações que podem ser executadas pelo usuário atual. Então pode ser iniciada uma solicitação para qualquer uma das operações especificadas.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>Parâmetros de URL
Propriedade| Descrição
---------|--------
smartcarduuid | Opcional. O UUID conforme indicado pelo cartão inteligente MIM CM. Este é o campo "uuid" do objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx).

###<a name="query-parameters"></a>Parâmetros de consulta
Propriedade| Descrição
---------|--------
cardid | Opcional. O UUID conforme indicado pelo cartão inteligente MIM CM. Este é o campo "uuid" do objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx).


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
Em caso de sucesso, retorna um objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) serializado para JSON com as seguintes propriedades:

Nome | Descrição
-----|-----------
AssignedUserUuid | O identificador do usuário a quem o cartão inteligente é atribuído.
Atr | A cadeia de ATR (resposta à redefinição) do cartão inteligente para o cartão que atualmente está sendo inicializado.
Comentário | O comentário que descreve o cartão inteligente.
Sinalizadores | Os sinalizadores que descrevem o cartão inteligente.
Middleware | O middleware para o cartão inteligente.
ParentSmartcardUuid | O identificador do cartão inteligente antigo que o cartão inteligente substituiu.
PermanentSmartcardUuid | O identificador do cartão inteligente permanente que está associado ao cartão inteligente.
PrimarySmartcardUuid | O identificador do cartão inteligente primário.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e as configurações que controlam o cartão inteligente.
ProfileTemplateVersion | A versão do modelo de perfil no momento em que o perfil de cartão inteligente foi criado.
SerialNumber | O número de série do cartão inteligente.
Status | O status do cartão inteligente.
Uuid | O identificador do perfil do cartão inteligente.

##<a name="example"></a>Exemplo

###<a name="request-1"></a>Solicitação 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###<a name="request-2"></a>Solicitação 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>Resposta 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
##<a name="see-also"></a>Consulte também

-[Classe Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
