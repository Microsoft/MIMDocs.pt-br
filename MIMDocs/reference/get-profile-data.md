---
title: Obter dados de perfil | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62b4c336122376cd7da836b9ec6c6e09eac24729
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757224"
---
# <a name="get-profile-data"></a>Obter dados de perfil
Obtém uma lista de perfis de certificado de software para um usuário. A lista inclui as possíveis operações que podem ser executadas pelo usuário atual. Então pode ser iniciada uma solicitação para qualquer uma das operações especificadas.

>[!IMPORTANT]
>O servidor definirá o PIN somente se a política de modelo de perfil indicar que ele deve ser feito. Caso contrário, o usuário deverá fornecer o PIN.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
id | O identificador (GUID) do perfil para retornar.
requestId | O identificador da solicitação para o qual retornar os perfis.

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
status | Opcional. Indica o status dos perfis para os quais recuperar dados. Os tipos de status possíveis são "ativo", "aprovado", "cancelado," "concluído," "negado", "em execução," "falha," "nenhum" e "pendente". <br/>Se nenhum status for especificado, todos os perfis, independentemente do status, serão retornados.

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
Em caso de êxito, retorna uma lista de objetos [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) serializados em JSON com as seguintes propriedades:

Propriedade | Descrição
---------|------------
AssignedUserUuid | O identificador do usuário ao qual o perfil é atribuído.
Comentário | O comentário que descreve o perfil.
Flags | Os sinalizadores que descrevem o perfil.
ParentProfileUuid | O identificador do perfil antigo que o perfil substituiu.
PrimaryProfileUuid | O identificador do perfil primário.
ProfileOperations | A lista de possíveis operações que podem ser executadas pelo usuário atual no perfil.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e as configurações que controlam o perfil.
ProfileTemplateVersion | A versão do modelo de perfil no momento em que o perfil foi criado.
Status | O status do perfil.
Uuid | O identificador do perfil.


## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter os dados de perfil de um usuário.

### <a name="example-request"></a>Exemplo: solicitação

```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```

### <a name="example-response"></a>Exemplo: resposta

```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```

## <a name="see-also"></a>Veja também

- [Classe Microsoft. CLM. Shared. Profiles. Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
