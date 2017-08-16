---
title: "Obter política de fluxo de trabalho | Microsoft Docs"
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
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6bc88fee88bdfeec3ce4ead60bc17fe2ea169402
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="get-workflow-policy"></a>Obter a política de fluxo de trabalho
Obtém a política do modelo de perfil para o fluxo de trabalho especificado. Esses dados são usados durante a criação da solicitação. A política de fluxo de trabalho especifica de quais dados o cliente precisa para criar uma solicitação. Esses dados podem incluir: vários itens de coleta de dados, solicitação de comentários e uma política de senha.

**Observação**: As URLs mostradas neste tópico são relativas ao nome do host escolhido durante a implantação da API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitação


Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{tipo}

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro| Descrição
--------|-------------
id| Obrigatório. O GUID correspondente ao modelo de perfil do qual extrair a política.
tipo| Obrigatório. O tipo de política que está sendo solicitada. Os valores possíveis são: *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*, *Unblock*.

###<a name="request-headers"></a>Cabeçalhos de solicitação
Para os cabeçalhos de solicitação comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="request-body"></a>Corpo da solicitação
nenhuma

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
403 | Proibido
204 | Sem conteúdo
500 | Erro Interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para os cabeçalhos de resposta comuns, confira a seção [Cabeçalhos de solicitação e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers), no artigo *Detalhes do serviço da API REST do MIM CM*.
###<a name="response-body"></a>Corpo da Resposta
Em caso de êxito, retorna um Objeto de Política com base no objeto [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx). No mínimo, o objeto de política conterá as propriedades na tabela a seguir, mas pode conter propriedades adicionais, dependendo da política solicitada. Por exemplo, uma solicitação de política de registro retornará um objeto [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx). Para obter mais informações, consulte a documentação para o objeto de política associado ao parâmetro {type} na solicitação. A documentação dos vários tipos de objetos de política está disponível na documentação do [Namespace Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx).

Propriedade | Descrição
---------|------------
ApprovalsNeeded | O número de aprovações necessárias para solicitações de FIM CM para a política.
AuthorizedApprover | O descritor de segurança para os usuários autorizados a aprovar solicitações de FIM CM para a política.
AuthorizedEnrollmentAgent | O descritor de segurança para os usuários que podem atuar como agentes de registro para a política.
AuthorizedInitiator | O descritor de segurança para os usuários que podem iniciar solicitações de FIM CM para a política.
CollectComments | Um valor booliano que indica se a coleção de comentários está habilitada para solicitações de FIM CM para a política.
CollectSolicitaçãoPriority | Um valor booliano que indica se a coleção de prioridade da solicitação está habilitada para solicitações de FIM CM para a política.
DefaultSolicitaçãoPriority | A prioridade padrão para solicitações de FIM CM para a política.
Documentos | Os documentos de política que estão configurados para a política.
Habilitada | Um valor booliano que indica se a política está habilitada.
EnrollAgentRequired | Um valor booliano que indica se os agentes de registro são necessários para solicitações de FIM CM para a política.
OneTimePasswordPolicy | Obtém o modo como as senhas únicas para solicitações de FIM CM para a política são distribuídas.
Personalização | As opções de personalização de cartão inteligente para a política.
PolicyDataCollection | Os itens de coleta de dados associados à política.
SelfServiceHabilitada | Um valor booliano que indica se a iniciação de autoatendimento das solicitações de FIM CM está habilitada para a política.

##<a name="example"></a>Exemplo

###<a name="request-1"></a>Solicitação 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Solicitação 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Resposta 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Consulte também

- [Classe Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Namespace Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
