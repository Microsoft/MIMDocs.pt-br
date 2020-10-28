---
title: Atualizar o status do cartão inteligente | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fe6d59377ef3218fde0df99365506ef9ec143a6f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757490"
---
# <a name="update-smart-card-status"></a>Atualizar o status do cartão inteligente
Atualiza o status de um cartão inteligente.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
reqid | Necessário. O identificador de solicitação que é específico para o gerenciamento de certificado do MIM (Microsoft Identity Manager) (Gerenciador de certificados).
scid | Necessário. O identificador do cartão inteligente específico para MIM CM. O valor corresponde ao campo "UUID" no objeto [Microsoft. CLM. Shared. Smarts. SmartCard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
status | O status para o qual definir a solicitação, como "desativado".

## <a name="response"></a>Resposta
Esta seção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200     | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de sucesso, retorna um objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) serializado para JSON com as seguintes propriedades:

Name | Descrição
-----|-----------
AssignedUserUuid | O identificador do usuário a quem o cartão inteligente é atribuído.
Atr | A cadeia de ATR (resposta à redefinição) do cartão inteligente para o cartão que atualmente está sendo inicializado.
Comentário | O comentário que descreve o cartão inteligente.
Flags | Os sinalizadores que descrevem o cartão inteligente.
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
Esta seção fornece um exemplo para atualizar o status de um cartão inteligente.

### <a name="example-request"></a>Exemplo: solicitação

```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

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

## <a name="see-also"></a>Veja também

- [Classe Microsoft. CLM. Shared. Smarts. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
