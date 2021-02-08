---
title: Obter perfis de cartão inteligente | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 605dfe1359b5706b27682a086f039f53a5ddaa05
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835681"
---
# <a name="get-smart-card-profiles"></a>Obter perfis de cartão inteligente
Obtém uma lista de perfis de cartão inteligente para um usuário. A lista inclui as possíveis operações que podem ser executadas pelo usuário atual. Então pode ser iniciada uma solicitação para qualquer uma das operações especificadas.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


### <a name="url-parameters"></a>Parâmetros de URL

Propriedade| Descrição
---------|--------
smartcarduuid | Opcional. O UUID do cartão inteligente, conforme indicado pelo gerenciamento de certificados (MIM) da Microsoft Identity Manager (eu). O valor corresponde ao campo "UUID" no objeto [Microsoft. CLM. Shared. Smarts. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Parâmetros de consulta

Propriedade| Descrição
---------|--------
cardid | Opcional. O UUID do cartão inteligente, como indicado por MIM CM. O valor corresponde ao campo "UUID" no objeto [Microsoft. CLM. Shared. Smarts. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm*.

### <a name="request-body"></a>Corpo da solicitação
Nenhum.

## <a name="response"></a>Resposta
Esta seção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm*.

### <a name="response-body"></a>Corpo da resposta
Em caso de sucesso, retorna um objeto [Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) serializado para JSON com as seguintes propriedades:

Nome | Descrição
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
Esta seção fornece um exemplo para obter perfis de cartão inteligente para um usuário.

### <a name="example-request-1"></a>Exemplo: solicitação 1

```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: resposta 1

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

### <a name="example-request-2"></a>Exemplo: solicitação 2

```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: resposta 2

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

## <a name="see-also"></a>Consulte também

- [Classe Microsoft. CLM. Shared. Smarts. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
