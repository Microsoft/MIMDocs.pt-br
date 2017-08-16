---
title: Obter dados de perfil | Microsoft Docs
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Obter dados do perfil
Obtém uma lista de perfis de certificado de software do usuário com uma lista de possíveis operações que podem ser executadas pelo usuário atual. Então pode ser iniciada uma solicitação para qualquer uma das operações especificadas.

**Observação**: O servidor definirá o PIN somente se a política de modelo de perfil indicar que isso deve ser feito. Caso contrário, o usuário deve fornecer.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
id | O identificador (GUID) do perfil para retornar.
requestId | O identificador da solicitação para o qual retornar os perfis.

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
---------|------------
status | Opcional. Indica o status dos perfis para o qual recuperar dados. Os tipos de status possíveis são: "Ativo", "Aprovado", "Cancelado", "Concluído", "Negado", "Em execução", "Não", "Nenhum" e "Pendente". <br/>Se nenhum status for especificado, serão retornados todos os perfis, independentemente do status.
###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
nenhuma

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
Em caso de êxito, retorna uma lista de objetos [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) serializados em JSON com as seguintes propriedades:

Propriedade | Descrição
---------|------------
AssignedUserUuid | O identificador do usuário ao qual o perfil é atribuído.
Comentário | O comentário que descreve o perfil.
Sinalizadores | Os sinalizadores que descrevem o perfil.
ParentProfileUuid | O identificador do perfil antigo que o perfil substituiu.
PrimaryProfileUuid | O identificador do perfil primário.
ProfileOperations | A lista de possíveis operações que podem ser executadas pelo usuário atual no perfil.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e as configurações que controlam o perfil.
ProfileTemplateVersion | A versão do modelo de perfil no momento em que o perfil foi criado.
Status | O status do perfil.
Uuid | O identificador do perfil.


##<a name="example"></a>Exemplo

###<a name="request"></a>Solicitação
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Resposta
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
##<a name="see-also"></a>Consulte também

- [Classe Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
