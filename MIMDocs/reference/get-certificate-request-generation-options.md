---
title: Obter opções de geração de solicitação de certificado | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8baab788c07dfb8c009857b45e38eb662f98199
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757230"
---
# <a name="get-certificate-request-generation-options"></a>Obter opções de geração de solicitação de certificado
Obtém os parâmetros para a geração de solicitação de certificado do lado do cliente.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

| Método |                                       URL da solicitação                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions |

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
--------|--------------
requestid| Necessário. O identificador GUID da solicitação do MIM CM para a qual os parâmetros de geração de solicitação de certificado devem ser recuperados.

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
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de sucesso, retorna uma lista de objetos CertificateRequestGenerationOptions. Cada objeto CertificateRequestGenerationOptions corresponde a uma única solicitação de certificado que o cliente precisa gerar. Cada objeto tem as seguintes propriedades:

Propriedade| Descrição
--------|-----------
Exportável | Um valor que especifica se a chave privada criada para a solicitação pode ser exportada.
FriendlyName | O nome de exibição do certificado registrado.
HashAlgorithmName | O algoritmo de hash usado ao criar a assinatura da solicitação de certificado.
KeyAlgorithmName | O algoritmo de chave pública.
KeyProtectionLevel | O nível de proteção de chave forte.
KeySize | O tamanho, em bits, da chave privada a ser gerada.
KeyStorageProviderNames | Uma lista de KSPs (provedores de armazenamento de chave) aceitáveis que podem ser usados para gerar a chave privada. Quando o primeiro KSP não pode ser usado para gerar a solicitação de certificado, qualquer um dos KSPs especificados pode ser usado até obter um sucesso.
KeyUsages | A operação que pode ser executada com a chave privada criada para esta solicitação de certificado. O valor padrão é Assinatura.
Assunto | O nome da entidade.

>[!NOTE]
>Mais informações sobre essas propriedades estão disponíveis na descrição da [classe Windows. Security. Cryptography. Certificates. CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) . Tenha em mente que não há uma correspondência de um para um entre essa classe e objetos CertificateRequestGenerationOptions.
>

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter as opções de geração para uma solicitação de certificado.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```  
