---
title: Obter PIN proposto do cartão inteligente | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9d4f1444efa490f980e29440342844ac246d8a20
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757250"
---
# <a name="get-smart-card-proposed-pin"></a>Obter PIN proposto do cartão inteligente
Obtém o PIN do usuário gerado pelo servidor.

>[!IMPORTANT]
>O servidor só definirá o PIN se a política de modelo de perfil indicar que ele deve ser feito. Caso contrário, o usuário deverá fornecer o PIN.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
id | A ID do cartão inteligente que é específica para o gerenciamento de certificado do MIM (Microsoft Identity Manager). A ID é obtida do objeto Microsft. CLM. Shared. SmartCard.

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
atr | A cadeia de ATR (resposta para redefinição) do cartão inteligente.
cardid | A ID do cartão inteligente.
desafio | Uma cadeia de caracteres codificada em base 64 que representa o desafio emitido pelo cartão inteligente.

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
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de sucesso, retorna uma cadeia de caracteres que representa o PIN proposto pelo servidor.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter o PIN do usuário gerado pelo servidor.

### <a name="example-request"></a>Exemplo: solicitação

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

... body coming soon
```       
