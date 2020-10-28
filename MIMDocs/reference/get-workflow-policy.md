---
title: Obter política de fluxo de trabalho | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1928bfd4dff75b1484a8a232e41c742842fa01b9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757249"
---
# <a name="get-workflow-policy"></a>Obter política de fluxo de trabalho
Obtém a política de modelo de perfil para um fluxo de trabalho especificado. Os dados são usados durante a criação da solicitação. A política de fluxo de trabalho especifica de quais dados o cliente precisa para criar uma solicitação. Os dados podem incluir vários itens de coleta de dados, comentários de solicitação e uma política de senha de uso único.

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
tipo| Necessário. O tipo de política que está sendo solicitada. Os valores possíveis são "registro", "duplicado", "OfflineUnblock", "OnlineUpdate", "renovar", "recuperar", "RecoverOnBehalf", "" reaplicar "," desativar "," revogar "," TemporaryEnroll "e" desbloquear ".

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
Em caso de sucesso, retorna um objeto de política baseado em um objeto [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) . No mínimo, o objeto de política contém as propriedades na tabela a seguir, mas pode conter propriedades adicionais, dependendo da política solicitada. Por exemplo, uma solicitação de uma política de registro retorna um objeto [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx) . Para obter mais informações, consulte a documentação do objeto de política associado ao parâmetro {Type} na solicitação. A documentação para os diferentes tipos de objetos de política pode ser encontrada na documentação do [namespace Microsoft. CLM. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx) .

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
