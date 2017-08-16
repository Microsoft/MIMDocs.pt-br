---
title: "Detalhes do serviço da API REST do MIM CM | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Detalhes do serviço da API REST do CM
As seções a seguir discutem os detalhes da API REST do MIM CM (Microsoft Identity Manager Certificate Management).

## <a name="architecture"></a>Arquitetura 
As chamadas da API REST do MIM CM são manipuladas pelos controladores. A tabela a seguir mostra a lista completa de controladores e exemplos do contexto no qual eles podem ser usados.

Controlador| Rota de exemplo
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates
OperationsController| /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| /api/v1.0/profiles/{id} <br/> /api/v1.0/Profiles <br/> /api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| /api/v1.0/requests/{id} <br/> /api/v1.0/requests
SmartcardsController| /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>

## <a name="http-request-and-response-headers"></a>Cabeçalhos de solicitação e resposta HTTP

As solicitações HTTP enviadas para a API devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Authorization | Obrigatório. Os conteúdos dependerão do método de autenticação, que pode ser configurado e pode ser baseado em WIA (autenticação integrada do Windows) ou ADFS.
Content-Type | Obrigatório se a solicitação tiver um corpo. Deve ser "application/json".
Content-Length | Obrigatório se a solicitação tiver um corpo. 
Cookie | O cookie de sessão. Pode ser necessário, dependendo do método de autenticação.
<br/>
As respostas HTTP incluirão os cabeçalhos a seguir (a lista não é exaustiva):

parâmetro | Descrição
-------|------------
Content-Type | A API sempre retorna "application/json".
Content-Length | O comprimento do corpo da solicitação, se presente, em bytes.


## <a name="api-versioning"></a>Controle de versão de API 
A versão atual da API REST do CM é 1.0. A versão é especificada no segmento diretamente após o segmento `/api` no URI: `/api/v1.0`. No futuro, o número de versão mudará caso haja grandes alterações na interface da API.


## <a name="enabling-the-api"></a>Habilitando a API 
A seção `<ClmConfiguration>` do arquivo Web.config foi estendida com uma nova chave:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Essa chave determina se a API REST do CM é exposta aos clientes. Se a chave for definida como "false", o mapeamento de rota para a API não será executado na inicialização do aplicativo. Isso significa que as solicitações subsequentes para pontos de extremidade da API retornarão um código de erro HTTP 404. Por padrão, a chave é definida como "desabilitada".
Depois de alterar esse valor para "true", lembre-se de executar **iisreset** no servidor.

## <a name="enabling-tracing-and-logging"></a>Habilitando rastreamento e registro em log 
A API de REST do MIM CM emite os dados de rastreamento para cada solicitação HTTP enviada a ela. Você pode definir o nível de detalhamento das informações de rastreamento emitidas ajustando o valor de configuração a seguir:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Tratamento de erro e solução de problemas 
Quando ocorrerem exceções ao processar uma solicitação, a API REST do CM MIM retorna um código de status HTTP para o cliente web. Para erros comuns, a API retorna um código de status HTTP apropriado e um código de erro. 

Exceções sem tratamento são convertidas para uma `HttpResponseException` com o Código de Status HTTP 500 ("Erro interno") e rastreadas no log de eventos e no arquivo de rastreamento do MIM CM. Cada exceção sem tratamento é gravada no log de eventos com uma ID de correlação correspondente. A ID de correlação também é enviada para o consumidor da API na mensagem de erro. Para solucionar o erro, o administrador pode pesquisar no log de eventos os detalhes da ID de correlação e do erro correspondentes.

Devido a preocupações com segurança, rastreamentos de pilha correspondendo a erros gerados como resultado do consumo da API REST do MIM CM não são enviados ao cliente.
