---
title: "Detalhes do serviço da API REST do PAM | Microsoft Docs"
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Detalhes do serviço da API REST do PAM
As seções a seguir discutem os detalhes da API REST do MIM PAM (Microsoft Identity Manager Privileged Access Management).

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

## <a name="versioning"></a>Versão 
A versão atual da API é 1. A versão da API pode ser especificada por meio de um parâmetro de consulta na URL da solicitação, como mostra o exemplo a seguir: `http://localhost:8086/api/pamresources/pamrequests?v=1` Se a versão não for especificada na solicitação, a solicitação será executada em relação à versão lançada mais recentemente da API. 

## <a name="security"></a>Segurança  
O acesso à API requer IWA (autenticação integrada do Windows). Isso deve ser configurado manualmente no IIS antes da instalação do MIM (Microsoft Identity Manager).

Há suporte para HTTPS (TLS), mas deve ser configurado manualmente no IIS. Para saber mais, confira a seção **Implementar o protocolo SSL para o Portal do FIM** em [Etapa 9: Executar tarefas pós-instalação do FIM 2010 R2] (https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx), na instalação do Guia de Test Lab do FIM 2010 R2. 

Você pode gerar um novo certificado do servidor SSL executando o seguinte comando ao prompt de comando do Visual Studio:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Esse comando cria um certificado autoassinado que pode ser usado para testar um aplicativo Web que use SSL em um servidor Web cuja URL seja test.cwap.com. O OID definido pela opção -eku identifica o certificado como um certificado do servidor SSL. O certificado é armazenado no meu repositório e está disponível no nível do computador, assim, você pode exportá-lo do snap-in Certificados em mmc.exe

## <a name="cross-domain-access-cors"></a>CORS (acesso entre domínios) 
Há suporte para CORS, mas deve ser configurado manualmente no IIS. Adicione os seguintes elementos ao arquivo web.config da API implantado para configurar a API para permitir chamadas entre domínios: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>Tratamento de erro 
A API retorna respostas de erro de HTTP para indicar as condições de erro. Os erros são compatíveis com OData. A tabela a seguir mostra os códigos de erro que podem ser retornados ao cliente.

Código de Status HTTP | Descrição
-----------------|------------
401 | Não autorizado 
403 | Proibido 
408 | Tempo Limite da Solicitação   
500 | Erro Interno do Servidor 
503 | Serviço Indisponível 
<br/>

## <a name="filtering"></a>Filtragem 
As solicitações da API REST do PAM podem incluir filtros para especificar as propriedades que devem ser incluídas na resposta. A sintaxe do filtro se baseia em expressões OData.

Os filtros podem especificar qualquer uma das propriedades de solicitações do PAM, funções do PAM. ou solicitações PAM do pendentes. Por exemplo: *ExpirationTime*, *DisplayName*ou qualquer outra propriedade válida de uma solicitação do PAM, função do PAM ou solicitação pendente.

A API dá suporte para os seguintes operadores em expressões de filtro: *And*, *Equal*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*e *LessThanOrEqual*. 

As solicitações de exemplo a seguir incluem filtros:

- Essa solicitação retorna todas as solicitações do PAM entre datas específicas: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Essa solicitação retorna a Função do PAM com o nome de exibição "Acesso a arquivo SQL": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
