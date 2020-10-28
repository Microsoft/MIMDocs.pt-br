---
title: Detalhes do serviço da API REST do PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00a2f4d9c44747d50139655d368e42b11fbd388c
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/14/2020
ms.locfileid: "92757493"
---
# <a name="pam-rest-api-service-details"></a>Detalhes do serviço da API REST do PAM
As seções a seguir discutem os detalhes da API REST do MIM PAM (Microsoft Identity Manager Privileged Access Management).

<h2 id="http-request-and-response-headers">Cabeçalhos de solicitação HTTP</h2>

As solicitações HTTP enviadas para a API devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Autorização | Necessário. O conteúdo depende do método de autenticação, que é configurável e pode ser baseado em WIA (autenticação integrada do Windows) ou no ADFS.
Tipo de conteúdo | Obrigatório se a solicitação tiver um corpo. Deve ser definido como `application/json`.
Content-Length | Obrigatório se a solicitação tiver um corpo. 
Cookie | O cookie de sessão. Pode ser necessário, dependendo do método de autenticação.

## <a name="http-response-headers"></a>Cabeçalhos de resposta HTTP

As respostas HTTP devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Tipo de conteúdo | A API sempre retorna `application/json` .
Content-Length | O comprimento do corpo da solicitação, se presente, em bytes.

## <a name="versioning"></a>Controle de versão 
A versão atual da API é 1. A versão da API pode ser especificada por meio de um parâmetro de consulta na URL da solicitação, como mostra o exemplo a seguir: `http://localhost:8086/api/pamresources/pamrequests?v=1` Se a versão não for especificada na solicitação, a solicitação será executada em relação à versão lançada mais recentemente da API. 

## <a name="security"></a>Segurança 
O acesso à API requer IWA (autenticação integrada do Windows). Isso deve ser configurado manualmente no IIS antes da instalação do MIM (Microsoft Identity Manager).

Há suporte para HTTPS (TLS), mas deve ser configurado manualmente no IIS. Para obter informações, consulte: **implementar protocolo SSL (SSL) para o portal do fim** na [etapa 9: executar as tarefas de pós-instalação do fim 2010 R2](https://technet.microsoft.com/library/hh322875.aspx) no guia de laboratório de teste do fim 2010 R2 em instalação. 

Você pode gerar um novo certificado do servidor SSL executando o seguinte comando ao prompt de comando do Visual Studio:

```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
O comando cria um certificado autoassinado que pode ser usado para testar um aplicativo Web que usa SSL em um servidor Web no qual a URL é `test.cwap.com` . A OID definida pela `-eku` opção identifica o certificado como um certificado de servidor SSL. O certificado é armazenado em **meu** repositório e está disponível no nível do computador. Você pode exportar o certificado do snap-in de certificados no mmc.exe.

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

## <a name="error-handling"></a>Tratamento de erros 
A API retorna respostas de erro de HTTP para indicar as condições de erro. Os erros são compatíveis com OData. A tabela a seguir mostra os códigos de erro que podem ser retornados a um cliente:

Código de status HTTP | Descrição
-----------------|------------
401 | Não Autorizado 
403 | Proibido 
408 | Tempo Limite da Solicitação   
500 | Erro interno do servidor 
503 | Serviço indisponível 

## <a name="filtering"></a>Filtragem 
As solicitações da API REST do PAM podem incluir filtros para especificar as propriedades que devem ser incluídas na resposta. A sintaxe do filtro se baseia em expressões OData.

Os filtros podem especificar qualquer uma das propriedades de solicitações do PAM, funções do PAM. ou solicitações PAM do pendentes. Por exemplo: *ExpirationTime* , *DisplayName* ou qualquer outra propriedade válida de uma Solicitação do PAM, Função do PAM ou Solicitação Pendente.

A API dá suporte aos operadores a seguir nas expressões de filtro: *And* , *Equal* , *NotEqual* , *GreaterThan* , *LessThan* , *GreaterThenOrEqueal* e *LessThanOrEqual* . 

As solicitações de exemplo a seguir incluem filtros:

- Essa solicitação retorna todas as solicitações do PAM entre datas específicas: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime'2015-01-09T08:26:49.721Z' and ExpirationTime lt datetime'2015-02-10T08:26:49.722Z' `
 
- Essa solicitação retorna a Função do PAM com o nome de exibição "Acesso a arquivo SQL": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq 'SQL File Access' `
