---
title: Obter modelos de perfil | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9f14853b3895dece1098270d35439d6c4999be2b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757220"
---
# <a name="get-profile-templates"></a>Obter modelos de perfil
Obtém uma lista de modelos de perfil para os quais o usuário especificado pode se registrar. Esse método retorna uma exibição limitada do modelo de perfil. Os dados do modelo de perfil retornados devem ser suficientes para permitir que o usuário solicitante decida para qual modelo de perfil, se houver, ele precisa se registrar. Se nenhum fluxo de trabalho e permissão forem especificados, todos os modelos de perfil visíveis para o usuário serão retornados.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[usuáriodedestino\] 

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro| Descrição
--------|-------------
targetuser| Opcional. Especifica o usuário de destino para o qual retornar os modelos de perfil. Se não for especificado, a identidade do usuário atual será usada. <br/><br/>**Observação** : atualmente, apenas o usuário atual tem suporte.

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="request-body"></a>Corpo da solicitação
nenhuma.

## <a name="response"></a>Resposta
Esta seção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
204 | Sem conteúdo
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de sucesso, retorna uma lista de objetos ProfileTemplateLimitedView com as seguintes propriedades:

Propriedade| Type| Descrição
--------|-----|--------
Nome| cadeia de caracteres| O nome de exibição do modelo de perfil.
Descrição| string| A descrição para o modelo de perfil.
Uuid| Guid| O identificador para o modelo de perfil.
IsSmartcardProfileTemplate| bool| Indica se o modelo é um modelo de perfil de cartão inteligente.
IsVirtualSmartcardProfileTemplate| bool| Indica se o modelo de perfil requer um cartão inteligente virtual.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter a lista de modelos de perfil para o usuário especificado.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]
```       
