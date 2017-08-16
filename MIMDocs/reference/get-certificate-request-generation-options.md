---
title: "Obter opções de geração de solicitação de certificado | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Obter opções de geração de solicitação de certificado

Obtém os parâmetros para a geração de solicitação de certificado do lado do cliente.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
--------|--------------
requestid| Obrigatório. O identificador GUID da solicitação do MIM CM para a qual os parâmetros de geração de solicitação de certificado devem ser recuperados.

###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
nenhum.


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200 | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="response-body"></a>Corpo da Resposta
Em caso de sucesso, retorna a lista de objetos CertificateRequestGenerationOptions. Cada objeto CertificateRequestGenerationOptions corresponde a uma única solicitação de certificado que o cliente precisa gerar e tem as seguintes propriedades:

Propriedade| Descrição
--------|-----------
Exportável | Um valor que especifica se a chave privada criada para a solicitação pode ser exportada.
FriendlyName | O nome de exibição do certificado registrado.
HashAlgorithmName | O algoritmo de hash usado ao criar a assinatura da solicitação de certificado.
KeyAlgorithmName | O algoritmo de chave pública.
KeyProtectionLevel | O nível de proteção de chave forte.
KeySize | O tamanho, em bits, da chave privada a ser gerada.
KeyStorageProviderNames | Uma lista de KSPs (provedores de armazenamento de chave) aceitáveis que podem ser usados para gerar a chave privada. No caso de não ser possível usar o primeiro KSP para gerar a solicitação de certificado, qualquer um dos KSPs especificados pode ser usado até um obter sucesso.
KeyUsages | A operação que pode ser executada com a chave privada criada para esta solicitação de certificado. O valor padrão é Assinatura.
Assunto | O nome da entidade.

**Observação**: saiba mais sobre essas propriedades na descrição da [classe Windows.Security.Cryptography.Certificates.CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx), mas lembre-se de que não há uma correspondência direta entre essa classe e os objetos CertificateRequestGenerationOptions.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Resposta
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
