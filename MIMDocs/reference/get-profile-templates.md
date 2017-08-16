---
title: Obter modelos de perfil | Microsoft Docs
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Obter modelos de perfil
Obtém uma lista de modelos de perfil para o qual o usuário especificado pode se inscrever. Esse método retorna uma exibição limitada do modelo de perfil. Os dados do modelo de perfil retornados devem ser suficientes para permitir que o usuário solicitante decida para qual modelo de perfil, se houver, ele precisa se registrar. Se nenhum fluxo de trabalho e uma permissão for especificado, todos os modelos de perfil visíveis para o usuário serão retornados.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[usuáriodedestino\] 

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro| Descrição
--------|-------------
targetuser| Opcional. Especifica o usuário de destino para o qual retornar os modelos de perfil. Se não for especificado, a identidade do usuário atual será usada. <br/>**Observação**: atualmente, apenas o usuário atual tem suporte.

###<a name="request-headers"></a>Cabeçalhos de solicitação
Consulte o tópico de serviço para cabeçalhos de solicitação comuns
###<a name="request-body"></a>Corpo da solicitação
nenhuma

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Sem conteúdo
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Consulte o tópico de serviço para cabeçalhos de solicitação comuns.
###<a name="response-body"></a>Corpo da Resposta
Em caso de sucesso, retorna uma lista de objetos ProfileTemplateLimitedView com as seguintes propriedades.

Propriedade| Tipo| Descrição
--------|-----|--------
Nome| cadeia de caracteres| O nome de exibição do modelo de perfil.
Descrição| cadeia de caracteres| A descrição para o modelo de perfil.
Uuid| Guid| O identificador para o modelo de perfil.
IsSmartcardProfileTemplate| bool| Indica se o modelo é um modelo de perfil de cartão inteligente.
IsVirtualSmartcardProfileTemplate| bool| Indica se o modelo de perfil requer um cartão inteligente virtual.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Resposta
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
