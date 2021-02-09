---
title: Obter chave de administração diversificada do cartão inteligente | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5e21cd92496fd31ec12044f3b69599bf96bef62a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835602"
---
# <a name="get-smart-card-diversified-admin-key"></a>Obter chave de administração diversificada de cartão inteligente
Obtém a chave de administração diversificada para o cartão inteligente especificado.

>[!IMPORTANT]
>A chave de administração deve ser diversificada somente quando a política de modelo de perfil indicar que deveria ser.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
reqid | Obrigatório. O identificador de solicitação que é específico para o gerenciamento de certificado do MIM (Microsoft Identity Manager) (Gerenciador de certificados).
scid | Obrigatório. O identificador do cartão inteligente específico para MIM CM. O scid é obtido do objeto [Microsoft. CLM. Shared. smarters. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
atr | Opcional. A cadeia de ATR (resposta para redefinição) do cartão inteligente.
cardid | Obrigatório. A ID do cartão.

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
Em caso de sucesso, retorna um BLOB que representa a chave de administração diversificada.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter a chave de administração diversificada para um cartão inteligente.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## <a name="see-also"></a>Confira também

- [Método Microsoft. CLM. Provision. RequestOperations. createsmartcard (cadeia de caracteres, Cadeia de caracteres, solicitação)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
