---
title: Atribuir um cartão inteligente a uma solicitação | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a8acf3defc2087fc6ea08bf8063579058465fcc8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757234"
---
# <a name="assign-a-smart-card-to-a-request"></a>Atribuir um cartão inteligente a uma solicitação
Associa o cartão inteligente especificado à solicitação especificada. Depois que um cartão inteligente é associado, a solicitação só pode ser executada com o cartão especificado.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

### <a name="url-parameters"></a>Parâmetros de URL
nenhuma.

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
requestid | A ID da solicitação a ser associada ao cartão inteligente.
cardid | A cardid do cartão inteligente.
atr | A cadeia de ATR (resposta para redefinição) do cartão inteligente.


## <a name="response"></a>Resposta
Esta seção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
201 | Criado
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de êxito, retorna um URI para o objeto do cartão inteligente recém-criado.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para associar um cartão inteligente.

### <a name="example-request"></a>Exemplo: solicitação

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## <a name="see-also"></a>Veja também

- [Método Microsoft. CLM. Provision. RequestOperations. createsmartcard (cadeia de caracteres, Cadeia de caracteres, solicitação)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
