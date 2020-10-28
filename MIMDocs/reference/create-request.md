---
title: Criar solicitação | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 51016951f9dec22e2a40b44a2b76a840192b518f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757440"
---
# <a name="create-request"></a>Criar solicitação
Crie uma solicitação de gerenciamento de certificado (MIM) de Microsoft Identity Manager (eu).

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

### <a name="url-parameters"></a>Parâmetros de URL
nenhuma.

### <a name="request-headers"></a>Cabeçalhos de solicitação
Para cabeçalhos de solicitação comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
profiletemplateuuid | Necessário. O GUID do modelo de perfil para o qual o usuário está criando a solicitação.
datacollection | Necessário. Uma coleção de pares de nome-valor representando os dados que devem ser fornecidos pelo registrando. A coleção de dados necessários que devem ser fornecidos pode ser recuperada da política de fluxo de trabalho do modelo de perfil. Uma coleção vazia pode ser especificada.
destino | Opcional. O GUID do usuário de destino para o qual a solicitação será criada. Se não for especificado, o destino padrão será o usuário atual.
tipo | Necessário. O tipo de solicitação que está sendo criada. Os tipos de solicitação disponíveis incluem "registrar", "Duplicar", "OfflineUnblock", "" OnlineUpdate "," renovar "," recuperar "," RecoverOnBehalf "," reaplicar, "" desativar "," revogar, "" TemporaryCards "e" desbloquear ".<br/><br/>**Observação** : nem todos os tipos de solicitação têm suporte em todos os modelos de perfil. Por exemplo, você não pode especificar a operação de desbloqueio em um modelo de perfil de software.
comentário | Necessário. Quaisquer comentários que possam ser inseridos pelo usuário. A política de fluxo de trabalho define se a propriedade de comentário é necessária. Uma cadeia de caracteres vazia pode ser especificada.
priority | Opcional. A prioridade da solicitação. Se não for especificado, a prioridade de solicitação padrão, conforme determinado pelas configurações do modelo de perfil, será usada.


## <a name="response"></a>Resposta
Esta seção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
201 | Criado
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de sucesso, retorna o URI da solicitação recém-criada.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para criar solicitações de registro e desbloqueio.

### <a name="example-request-1"></a>Exemplo: solicitação 1

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### <a name="example-response-1"></a>Exemplo: resposta 1

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### <a name="example-request-2"></a>Exemplo: solicitação 2

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### <a name="example-response-2"></a>Exemplo: resposta 2

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### <a name="example-request-3"></a>Exemplo: solicitação 3

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```

## <a name="see-also"></a>Veja também

- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
