---
title: "Criar solicitação | Microsoft Docs"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Criar solicitação
Crie uma solicitação do MIM CM.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###<a name="url-parameters"></a>Parâmetros de URL
nenhuma

###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
profiletemplateuuid | Obrigatório. O GUID do modelo de perfil para o qual o usuário está criando a solicitação.
datacollection | Obrigatório. Uma coleção de pares de nome-valor representando os dados que devem ser fornecidos pelo registrando. A coleção de dados necessários que devem ser fornecidos pode ser recuperada da política de fluxo de trabalho do modelo de perfil. Uma coleção vazia pode ser especificada.
destino | Opcional. O GUID do usuário de destino para o qual a solicitação será criada. Se não especificado, assume como padrão o usuário atual.
tipo | Obrigatório. O tipo de solicitação que está sendo criada. Os tipos de solicitação disponíveis são: "Enroll", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Renew", "Recover", "RecoverOnBehalf", "Reinstate", "Retire", "Revoke", "TemporaryCards" e "Unblock".<br/>**Observação**: Nem todos os tipos de solicitação têm suporte em todos os modelos de perfil. Por exemplo, você não pode especificar uma operação de desbloqueio em um modelo de perfil de software.
comentar | Obrigatório. Quaisquer comentários que possam ser inseridos pelo usuário. A política de fluxo de trabalho definirá se isso é necessário. Uma cadeia de caracteres vazia pode ser especificada.
prioridade | Opcional. A prioridade da solicitação. Se não for especificado, a prioridade de solicitação padrão, conforme determinada pelas configurações de modelo de perfil, será usada.


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
201     | Criado
403 | Proibido
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="response-body"></a>Corpo da Resposta
Em caso de sucesso, retorna o URI da solicitação recém-criada.
##<a name="example"></a>Exemplo

###<a name="request-1"></a>Solicitação 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Solicitação 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Resposta 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Solicitação 3
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
##<a name="see-also"></a>Consulte também

- [Método Microsoft.Clm.Provision.RequestOperations.InitiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
