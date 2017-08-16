---
title: "Status de atualização de cartão inteligente | Microsoft Docs"
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>Atualizar o status do cartão inteligente
Atualiza o status de um cartão inteligente.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
## <a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
reqid | Obrigatório. O identificador da solicitação (específico para MIM CM).
scid | Obrigatório. O identificador do cartão inteligente (específico para o MIM CM). Esta é a propriedade "uuid" do objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx).

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
### <a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
status | O status para o qual definir a solicitação. Por exemplo, "desativado".


## <a name="response"></a>Resposta
### <a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
### <a name="response-body"></a>Corpo da Resposta
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

## <a name="example"></a>Exemplo

### <a name="request"></a>Solicitação
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>Resposta
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
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
## <a name="see-also"></a>Consulte também

- [Microsoft.Clm.Shared.Smartcards.Smartcard Class](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
