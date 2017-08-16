---
title: "Cancelar, abandonar ou concluir uma solicitação | Microsoft Docs"
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Cancelar, abandonar ou concluir uma solicitação
Marque uma solicitação de MIM CM como concluída, cancelada ou abandonada.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>Parâmetros de URL
Propriedade| Descrição
---------|--------
id| Obrigatório. O GUID da solicitação a concluir.


###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
O corpo da solicitação contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
status | O status para o qual definir a solicitação. Os valores possíveis são: "Concluído", "Cancelado" ou "Abandonado".


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="response-body"></a>Corpo da Resposta
Em caso de êxito, retorna um objeto [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) com as seguintes propriedades que descrevem a solicitação de MIM CM marcada como concluída:

Nome | Descrição
-----|------------
Comentário | O comentário associado à solicitação de MIM CM.
Concluído | A hora em que a solicitação do MIM CMM foi concluída.
DataCollection | Os itens de coleta de dados associados à solicitação do MIM CM.
DataCollectionSinalizadores | As opções de coleta de dados para a solicitação de MIM CM.
Sinalizadores | As opções associadas à solicitação de MIM CM.
IsDataCollectionComplete | Um valor booliano que indica se a coleta de dados foi concluída para a solicitação de MIM CM.
IsEnrollmentAgent | Um valor booliano que indica se um agente de registro é necessário para executar a solicitação de MIM CM.
IsSmartcard | Um valor booliano que indica se a solicitação de MIM CM é uma solicitação de cartão inteligente ou uma solicitação de perfil de software.
NewProfileUuid | A identidade do novo perfil de software produzida pela solicitação de MIM CM.
NewSmartcardUuid | A identidade do novo cartão inteligente atribuído pela solicitação de MIM CM.
OldProfileUuid | A identidade do perfil de software para o qual a solicitação de MIM CM foi criada.
OldSmartcardUuid | A identidade do cartão inteligente para o qual a solicitação de MIM CM foi criada.
OrigematorUserUuid | A identidade do usuário que originou a solicitação de MIM CM
Prioridade | A prioridade da solicitação de MIM CM.
ProfileTemplateUuid | A identidade do modelo de perfil para o qual a solicitação de MIM CM foi criada.
SolicitaçãoType | O tipo de solicitação de MIM CM.
SecurityDescriptor | O descritor de segurança para a solicitação de MIM CM.
Status | O status da solicitação de MIM CM.
Enviado | A hora em que a solicitação de MIM CMM foi enviada.
TargetUserUuid | A identidade do usuário de destino para a solicitação de MIM CM.
Uuid | O identificador para a solicitação de MIM CM.

##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>Consulte também

- [Método Microsoft.Clm.Provision.ExecuteOperations.Complete](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
