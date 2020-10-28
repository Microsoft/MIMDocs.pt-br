---
title: Obter política de cartão inteligente | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e8e8b2ac7dcf8f72d4989d458272d78b8367d923
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757431"
---
# <a name="get-smart-card-policy"></a>Obter política de cartão inteligente
Obtém a política do modelo de perfil para o fluxo de trabalho especificado. Esses dados são usados durante a criação da solicitação. A política de fluxo de trabalho especifica de quais dados o cliente precisa para criar uma solicitação. Os dados podem incluir vários itens de coleta de dados, comentários de solicitação e uma política de senha de uso único.

>[!NOTE]
>As URLs neste artigo são relativas ao nome de host escolhido durante a implantação da API, como `https://api.contoso.com` .

## <a name="request"></a>Solicitação

Método  |URL da solicitação  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro| Descrição
--------|-------------
id| Necessário. O GUID correspondente ao modelo de perfil do qual extrair a política.
tipo| Necessário. O tipo de política que está sendo solicitada. Os valores possíveis são "Enroll", "Duplicate", "OfflineUnblock", "" OnlineUpdate "," renovar "," Recover "," RecoverOnBehalf, "" reaplicar, "" desativar "," REVOKE "," TemporaryEnroll "e" desbloquear ".

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
403 | Proibido
204 | Sem conteúdo
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [cabeçalhos de solicitação e resposta http](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *detalhes do serviço da API REST cm* .

### <a name="response-body"></a>Corpo da resposta
Em caso de êxito, retorna um Objeto de Política com base no objeto [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx). No mínimo, o objeto de política contém as propriedades na tabela a seguir, mas pode conter propriedades adicionais, dependendo da política solicitada. Por exemplo, uma solicitação de uma política de registro retorna um objeto [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) . Para obter mais informações, consulte a documentação para o objeto de política associado ao parâmetro {type} na solicitação. A documentação para os diferentes tipos de objetos de política pode ser encontrada na documentação do [namespace Microsoft. CLM. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) .

Propriedade | Descrição
---------|------------
ApprovalsNeeded | O número de aprovações necessárias para as solicitações de gerenciamento de certificado (FIM) do Forefront Identity Manager (FIM) para a política.
AuthorizedApprover | O descritor de segurança para os usuários autorizados a aprovar solicitações de FIM CM para a política.
AuthorizedEnrollmentAgent | O descritor de segurança para os usuários que podem atuar como agentes de registro para a política.
AuthorizedInitiator | O descritor de segurança para os usuários que podem iniciar solicitações de FIM CM para a política.
CollectComments | Um valor booliano que indica se a coleção de comentários está habilitada para solicitações de FIM CM para a política.
CollectRequestPriority | Um valor booliano que indica se a coleção de prioridade da solicitação está habilitada para solicitações de FIM CM para a política.
DefaultRequestPriority | A prioridade padrão para solicitações de FIM CM para a política.
Documentos | Os documentos de política que estão configurados para a política.
habilitado | Um valor booliano que indica se a política está habilitada.
EnrollAgentRequired | Um valor booliano que indica se os agentes de registro são necessários para solicitações de FIM CM para a política.
OneTimePasswordPolicy | O método de distribuição para senhas de uso único para solicitações de FIM CM para a política.
Personalização | As opções de personalização de cartão inteligente para a política.
PolicyDataCollection | Os itens de coleta de dados associados à política.
SelfServiceEnabled | Um valor booliano que indica se a iniciação de autoatendimento das solicitações de FIM CM está habilitada para a política.

## <a name="example"></a>Exemplo
Esta seção fornece um exemplo para obter a política de modelo de perfil para um fluxo de trabalho. 

### <a name="example-request-1"></a>Exemplo: solicitação 1

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: resposta 1

```
HTTP/1.1 200 OK

... body coming soon
```       

### <a name="example-request-2"></a>Exemplo: solicitação 2

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: resposta 2

```
HTTP/1.1 200 OK

... body coming soon
```       

## <a name="see-also"></a>Veja também

- [Classe Microsoft. CLM. Shared. ProfileTemplates. ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Namespace Microsoft. CLM. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
