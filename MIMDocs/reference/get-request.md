---
title: Obter solicitação | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ad660c562b457890ea75d33325ada8d0f63feb8f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757435"
---
# <a name="get-request"></a>Solicitação GET
Obtém uma ou mais solicitações de gerenciamento de certificado (MIM) de Microsoft Identity Manager especificado.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>Parâmetros de URL

Propriedade| Descrição
---------|--------
id| Opcional. O GUID de uma solicitação para recuperar.

### <a name="query-parameters"></a>Parâmetros de consulta

Propriedade| Descrição
---------|--------
targetuser| Opcional. O usuário de destino da solicitação. Se nenhum destino for especificado, todas as solicitações, independentemente do usuário de destino, serão retornadas. <br/><br/>**Observação** : no momento, há suporte apenas para o valor "Current".
status| Opcional. Indica o status da solicitação a recuperar. Os tipos de status possíveis são "aprovado", "cancelado," "concluído," "negado," "em execução," "falha," "nenhum" e "pendente". <br/>Se nenhum status for especificado, todas as solicitações, independentemente do status, serão retornadas.

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
Em caso de êxito, retorna um ou mais objetos [Request](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) com as seguintes propriedades:

Name | Descrição
-----|------------
Comentário | O comentário associado à solicitação de MIM CM.
Concluído | A hora em que a solicitação do MIM CMM foi concluída.
DataCollection | Os itens de coleta de dados associados à solicitação do MIM CM.
DataCollectionFlags | As opções de coleta de dados para a solicitação de MIM CM.
Flags | As opções associadas à solicitação de MIM CM.
IsDataCollectionComplete | Um valor booliano que indica se a coleta de dados foi concluída para a solicitação de MIM CM.
IsEnrollmentAgent | Um valor booliano que indica se um agente de registro é necessário para executar a solicitação de MIM CM.
IsSmartcard | Um valor booliano que indica se a solicitação de MIM CM é uma solicitação de cartão inteligente ou uma solicitação de perfil de software.
NewProfileUuid | A identidade do novo perfil de software produzida pela solicitação de MIM CM.
NewSmartcardUuid | A identidade do novo cartão inteligente atribuído pela solicitação de MIM CM.
OldProfileUuid | A identidade do perfil de software para o qual a solicitação de MIM CM foi criada.
OldSmartcardUuid | A identidade do cartão inteligente para o qual a solicitação de MIM CM foi criada.
OriginatorUserUuid | A identidade do usuário que originou a solicitação de MIM CM.
Prioridade | A prioridade da solicitação de MIM CM.
ProfileTemplateUuid | A identidade do modelo de perfil para o qual a solicitação de MIM CM foi criada.
RequestType | O tipo de solicitação de MIM CM.
SecurityDescriptor | O descritor de segurança para a solicitação de MIM CM.
Status | O status da solicitação de MIM CM.
Enviado | A hora em que a solicitação de MIM CMM foi enviada.
TargetUserUuid | A identidade do usuário de destino para a solicitação de MIM CM.
Uuid | O identificador para a solicitação de MIM CM.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter as solicitações de MIM CM especificadas.

### <a name="example-request-1"></a>Exemplo: solicitação 1

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: resposta 1

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
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

### <a name="example-request-2"></a>Exemplo: solicitação 2

```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: resposta 2

```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```     

## <a name="see-also"></a>Veja também

- [Método Microsoft. CLM. Provision. Femdoperations. FindRequest](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Enumeração Microsoft. CLM. Shared. RequestPermission](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Classe Microsoft. CLM. Shared. requests. Request](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
