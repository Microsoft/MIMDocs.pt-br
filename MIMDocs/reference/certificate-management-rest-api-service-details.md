---
title: Detalhes do serviço da API REST do gerenciamento de certificados | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a2dd84e121217772a8831653b2e4790436c32ec
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/14/2020
ms.locfileid: "92757495"
---
# <a name="certificate-management-rest-api-service-details"></a>Detalhes do serviço da API REST do gerenciamento de certificados
As seções a seguir descrevem os detalhes da API REST do gerenciamento de certificado (MIM) de Microsoft Identity Manager.

## <a name="architecture"></a>Arquitetura 
As chamadas da API REST do MIM CM são manipuladas pelos controladores. A tabela a seguir mostra a lista completa de controladores e exemplos do contexto no qual eles podem ser usados.


|                  Controller                   |                                                                                                                                                           Rota de exemplo                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificateDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificateRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            CertificatesController             |                                                                                                                /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates                                                                                                                 |
|             OperationsController              |                                                                                                                  /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations                                                                                                                   |
|              PoliciesController               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              ProfilesController               |                                                                                                                                                     /api/v1.0/profiles/{id}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequestsController               |                                                                                                                                         /api/v1.0/requests/{id} <br/> /api/v1.0/requests                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## <a name="http-request-and-response-headers"></a>Cabeçalhos de solicitação e resposta HTTP
As solicitações HTTP enviadas para a API devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Autorização | Necessário. O conteúdo depende do método de autenticação. O método é configurável e pode ser baseado na autenticação integrada do Windows (WIA) ou Serviços de Federação do Active Directory (AD FS) (ADFS).
Tipo de conteúdo | Obrigatório se a solicitação tiver um corpo. Deve ser `application/json`.
Content-Length | Obrigatório se a solicitação tiver um corpo. 
Cookie | O cookie de sessão. Pode ser necessário, dependendo do método de autenticação.


As respostas HTTP incluem os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Tipo de conteúdo | A API sempre retorna `application/json` .
Content-Length | O comprimento do corpo da solicitação, se presente, em bytes.


## <a name="api-versioning"></a>Controle de versão de API 
A versão atual da API REST do CM é 1.0. A versão é especificada no segmento diretamente após o segmento `/api` no URI: `/api/v1.0`. O número de versão muda quando as principais modificações são feitas na interface de API.


## <a name="enable-the-api"></a>Habilitar a API 
A seção `<ClmConfiguration>` do arquivo Web.config foi estendida com uma nova chave:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

Essa chave determina se a API REST do CM é exposta aos clientes. Se a chave for definida como "false", o mapeamento de rota para a API não será executado na inicialização do aplicativo. As solicitações subsequentes para pontos de extremidade de API retornam um código de erro HTTP 404. Por padrão, a chave é definida como "desabilitada".

>[!NOTE]
>Depois de alterar esse valor para "true", lembre-se de executar **iisreset** no servidor.

## <a name="enable-tracing-and-logging"></a>Habilitar rastreamento e registro em log 
A API de REST do MIM CM emite os dados de rastreamento para cada solicitação HTTP enviada a ela. Você pode definir o nível de detalhamento das informações de rastreamento emitidas ajustando o valor de configuração a seguir:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## <a name="error-handling-and-troubleshooting"></a>Tratamento de erros e solução de problemas 
Quando ocorrerem exceções ao processar uma solicitação, a API REST do CM MIM retorna um código de status HTTP para o cliente web. Para erros comuns, a API retorna um código de status HTTP apropriado e um código de erro. 

Exceções sem tratamento são convertidas para uma `HttpResponseException` com o Código de Status HTTP 500 ("Erro interno") e rastreadas no log de eventos e no arquivo de rastreamento do MIM CM. Cada exceção sem tratamento é gravada no log de eventos com uma ID de correlação correspondente. A ID de correlação também é enviada para o consumidor da API na mensagem de erro. Para solucionar o erro, o administrador pode pesquisar no log de eventos os detalhes da ID de correlação e do erro correspondentes.

>[!NOTE]
>Os rastreamentos de pilha que correspondem a erros, que são gerados como resultado do consumo da API REST do MIM CM, não são enviados de volta ao cliente. Os rastreamentos não são enviados de volta ao cliente devido a questões de segurança.
