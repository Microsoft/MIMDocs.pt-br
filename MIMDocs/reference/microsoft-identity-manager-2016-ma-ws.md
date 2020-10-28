---
title: Visão geral do conector de serviço Web genérico | Microsoft Docs
description: Visão geral da configuração e dos requisitos para o conector de serviço Web genérico.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: b0d4cc9ac9b9ac080038d632df0c02c4c244e3d6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757462"
---
# <a name="overview-of-the-generic-web-service-connector"></a>Visão geral do conector de serviço Web genérico

O conector de serviço da Web integra identidades por meio de operações de serviço da Web com o MIM (Microsoft Identity Manager) 2016 SP1. O conector requer o arquivo de projeto de serviço Web para se conectar à fonte de dados correta. Este projeto pode ser baixado do [centro de download da Microsoft](https://go.microsoft.com/fwlink/?LinkID=235883) junto com a [documentação](https://www.microsoft.com/en-us/download/details.aspx?id=29943) para usar o conector com o Oracle eBusiness, Oracle PeopleSoft e SAP. Você também pode criá-lo usando a ferramenta de configuração do serviço Web.

Quando o serviço de sincronização do MIM invoca o conector de serviço Web, ele carrega seu arquivo de projeto configurado (arquivo **WsConfig** ). Esse arquivo ajuda a reconhecer o ponto de extremidade da fonte de dados que deve ser usado para estabelecer uma conexão. O arquivo também informa ao fluxo de trabalho para ser executado a fim de implementar uma operação do MIM. Para executar os fluxos de trabalho configurados, o conector de serviço Web aproveita o mecanismo de tempo de execução do .NET 4 Workflow Foundation.

![Fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws/workflow.png)

## <a name="web-service-layers"></a>Camadas de serviço Web

Duas camadas principais são usadas para implementar a solução de agente de gerenciamento de serviço da Web (MA): 

- Ferramenta de configuração do serviço Web
- Conector de tempo de execução implementado com o fluxo de trabalho .NET 4,0

## <a name="supported-data-sources-for-web-service-discovery"></a>Fontes de dados com suporte para descoberta de serviço Web

A ferramenta de configuração do serviço Web implementa as seguintes funcionalidades:

- Descoberta de SOAP: permite que o administrador insira o caminho WSDL exposto pelo serviço Web de destino. A descoberta produzirá uma estrutura de árvore de seus serviços Web hospedados com seus pontos de extremidade internos/Operations junto com a descrição de metadados da operação. Não há nenhum limite para o número de operações de descoberta que podem ser realizadas (passo a passo). As operações descobertas são usadas posteriormente para configurar o fluxo de operações que implementam as operações do conector em relação à fonte de dados (como importação/exportação/senha).

- Descoberta REST: permite que o administrador insira detalhes do serviço RESTful, ou seja, os detalhes do ponto de extremidade de serviço, caminho do recurso, método e parâmetro. Um usuário pode adicionar um número ilimitado de serviços RESTful. As informações dos serviços REST serão armazenadas no ```discovery.xml``` arquivo do ```wsconfig``` projeto. Eles serão usados posteriormente pelo usuário para configurar a atividade de serviço Web REST no fluxo de trabalho.

- Configuração de esquema de espaço do conector: permite que o administrador configure o esquema de espaço do conector. A configuração do esquema incluirá uma lista de tipos de objeto e atributos para uma implementação específica. O administrador pode especificar os tipos de objeto que terão suporte do serviço Web MA. O administrador também pode escolher aqui os atributos que serão parte do esquema do espaço do conector.

- Configuração de fluxo de operação: IU do designer de fluxo de trabalho para configurar a implementação de operações de FIM (importação/exportação/senha) por tipo de objeto por meio de funções de operações de serviço da Web expostas, como:

    - Atribuição de parâmetros do espaço do conector para funções de serviço Web.
    - A atribuição de parâmetros de funções de serviço Web ao espaço do conector.

## <a name="resources-generated-by-the-web-service-configuration-tool"></a>Recursos gerados pela ferramenta de configuração do serviço Web

A ferramenta de configuração de serviço Web gera os recursos necessários para configurar um MA de serviço Web totalmente funcional, que inclui:

- Esquema de espaço do conector: um arquivo binário que inclui a configuração do esquema. O arquivo será importado pelo MIM por meio da ```Get Schema``` interface quando o ma for configurado usando a interface do usuário de sincronização do fim. Em seguida, ele é convertido no objeto de formato de esquema ECMA2.

- Fluxos de trabalho: uma série de definições de fluxo de trabalho. Eles são usados pelo serviço Web MA em tempo de execução para executar uma operação apropriada.

- Arquivo de configurações do WCF: um arquivo de configuração que é produzido pela operação de descoberta. O arquivo inclui as informações de associação e pontos de extremidade necessários para invocar uma operação de serviço Web em relação à fonte de dados.

- Assembly de contrato de dados: como o conector de serviço Web agora dá suporte ao SOAP e ao serviço REST, os contratos de dados gerados serão diferentes no arquivo de generated.dll.

- Assembly SOAP: ao analisar a entrada WSDL, a ferramenta de configuração do serviço Web gera tipos de contrato de dados, que são estruturas de dados usadas pelas operações de serviço Web para se comunicar com o serviço remoto. Esses tipos de contrato também são usados para expor entidades de fonte de dados remotas para mapeamento de atributo de tipo de objeto.

- Assembly REST: ao analisar o exemplo de solicitação-resposta para o serviço Web REST, a ferramenta de configuração irá gerar tipos (classes), que serão usados no fluxo de trabalho para se comunicar com o serviço Web por meio da atividade de chamada de serviço Web. Cada solicitação/resposta será definida em seu próprio namespace. O namespace tem uma sintaxe como \< ServiceName \> . \< ResourceName \> . \< MethodName \> . [ Solicitação/resposta]. Envolver cada solicitação/resposta em um namespace separado ajudará a reduzir os problemas devido ao nome de tipo duplicado (classe).

![Fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws/workflow2.png)

### <a name="project-file-type"></a>Tipo de arquivo do projeto

O serviço Web MA é salvo em um arquivo compactado (formato ZIP) com o nome especificado pelo usuário e a extensão de arquivo "WsConfig". A extensão de arquivo "WsConfig" é registrada e associada à ferramenta de configuração de serviço Web pelo instalador. Os projetos de MA existentes podem ser abertos, modificados e salvos. Eles podem ser salvos na pasta extensões do serviço de sincronização do FIM ou em qualquer outro local. As alterações relacionadas ao tipo de objeto e atributos exigem sincronização no lado do FIM.  A ferramenta de configuração é um aplicativo de várias instâncias projetado para criar e modificar MA (s).

## <a name="supported-security-modes"></a>Modos de segurança com suporte

O aplicativo de serviço Web REST/SOAP pode ser protegido por meio de um servidor Web como o IIS. O aplicativo permite que o usuário selecione o modo de segurança, conforme mostrado na figura a seguir. Os modos de segurança incluem básico, Digest, certificado, Windows ou nenhum.

![Modos de segurança](media/microsoft-identity-manager-2016-ma-ws/security-mode.png)

## <a name="supported-data-types"></a>Tipos de dados com suporte

Há suporte para os seguintes tipos de dados:

- SOAP (Herdado): o tipo de dados SOAP tem suporte, conforme descrito neste [artigo do MSDN](https://msdn.microsoft.com/library/ms995800.aspx). O suporte é fornecido apenas para a pilha de BAPI (interface de programação de aplicativo de negócios). Os modelos SOAP de exemplo estão disponíveis no [centro de download da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).
- REST (não ODATA): um conector/Web baseado em protocolo HTTP.

## <a name="next-steps"></a>Próximas etapas 

- [Instalar a ferramenta de configuração do serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implantação de SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração de MA do serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
