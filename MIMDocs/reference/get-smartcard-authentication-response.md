---
title: Obter resposta de autenticação de cartão inteligente | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62811bd5d225f981ded6a6439584c2b7730251c1
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835584"
---
# <a name="get-smart-card-authentication-response"></a>Obter resposta de autenticação de cartão inteligente
Obtém a resposta a um desafio de autenticação do CSP (provedor de serviços de criptografia) base.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
reqid | Obrigatório. O identificador de solicitação que é específico para o gerenciamento de certificado do MIM (Microsoft Identity Manager) (Gerenciador de certificados).
scid | Obrigatório. O identificador do cartão inteligente específico para MIM CM. O scid é obtido do objeto [Microsoft. CLM. Shared. smarters. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
atr | Opcional. A cadeia de ATR (resposta para redefinição) do cartão inteligente.
cardid | Obrigatório. A ID do cartão inteligente.
desafio | Obrigatório. Uma cadeia de caracteres codificada em base 64 que representa o desafio emitido pelo cartão inteligente.
diversificado | Obrigatório. Um sinalizador booliano que indica se a chave de administrador do cartão inteligente foi diversificada.

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
Em caso de sucesso, retorna um BLOB de byte que representa a resposta do desafio.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter a resposta a um desafio de autenticação CSP base.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       

## <a name="see-also"></a>Consulte também

- [Microsoft.Clm.Provision.Exeo método cuteOperations. Getbasecspresposta](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
