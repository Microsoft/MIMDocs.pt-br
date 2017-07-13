---
title: "Guia de topologia de implantação | Microsoft Docs"
description: "Compreender os componentes do MIM 2016 e obter sugestões sobre como implantá-los em seu ambiente."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/21/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f7e4dc737444df70de3a8a78eb518e9e6f26aadc
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# Considerações de topologia
<a id="topology-considerations" class="xliff"></a>
Você pode implantar os componentes do MIM (Microsoft Identity Manager) no mesmo servidor ou entre vários servidores em várias configurações. A topologia que você seleciona para sua implantação afeta o desempenho que você pode obter do MIM. Este artigo apresenta várias topologias de implantação que você pode considerar.

## Componentes do MIM
<a id="mim-components" class="xliff"></a>
Ao projetar a topologia de implantação, é importante saber o que cada componente faz e como todos eles interagem.

- **Portal do MIM** - uma interface para operações administrativas, gerenciamento de grupo e redefinições de senha.
<a id="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations" class="xliff"></a>
    -
- **Serviço do MIM** -um serviço Web que implementa a funcionalidade de gerenciamento de identidade do MIM 2016.
- **Serviço de Sincronização do MIM** -sincroniza os dados com outros sistemas de identidade.
- **Microsoft SQL Server** - o Serviço do MIM e o Serviço de Sincronização do MIM armazenam seus dados nos bancos de dados SQL.

A tabela a seguir mostra as opções para hospedar cada um dos componentes do MIM. Eles podem ser hospedados no mesmo computador ou distribuídos entre vários servidores e clusters.

| | Portal MIM | Serviço MIM | Serviço de sincronização do MIM | SQL Server |
| --- | --- | --- | --- | --- |
| Mesmo computador | Sim | Sim | Sim | Sim |
| Servidor separado | Sim | Sim | Sim | Sim |
| Cluster de balanceamento de carga de rede | Sim | Sim | | |
| Cluster de servidores | | | | Sim |


## Topologia de multicamadas
<a id="multitier-topology" class="xliff"></a>
A topologia de multicamadas é a topologia mais comumente normalmente. Ela oferece maior flexibilidade. O Portal do MIM, o Serviço do MIM e os bancos de dados são separados em camadas e implantados em vários computadores. Esta topologia adiciona flexibilidade na colocação em escala dos diferentes componentes do MIM. Por exemplo, você pode dimensionar o Portal do MIM horizontalmente adicionando mais servidores em um cluster de NLB (Balanceamento de Carga de Rede). Da mesma forma, você pode dimensionar o serviço do MIM usando um cluster NLB e aumentando o número de computadores (nós) do cluster conforme necessário.

Na topologia de multicamadas, um computador dedicado para hospedar cada banco de dados SQL (um para o serviço do MIM e outro para o Serviço de Sincronização do MIM) é alocado. A escalabilidade do desempenho dos computadores que hospedam os bancos de dados SQL pode ser aumentada adicionando ou atualizando o hardware, por exemplo, atualizando as CPUs, adicionando mais CPUs, aumentando ou atualizando a memória RAM ou atualizando as configurações de disco rígido para aumentar a leitura e acesso de gravação e diminuir a latência.

![Diagrama de topologia de multicamadas do MIM](media/MIM-topo-multitier.png)

Nessa configuração, o Serviço de Sincronização do MIM e seu banco de dados são hospedados no mesmo computador. No entanto, você deve ser capaz de alcançar um desempenho semelhante, se houver uma conexão de rede dedicada de um gigabit entre o Serviço de Sincronização do MIM e seu banco de dados quando eles são hospedados em computadores separados.


## Topologia de multicamadas com vários serviços do MIM
<a id="multitier-topology-with-multiple-mim-services" class="xliff"></a>
A sincronização de dados com sistemas externos pode demorar bastante e acrescentar uma carga considerável ao sistema durante esse período. Se a configuração da sincronização resulta em disparo de políticas com fluxos de trabalho, essas políticas disputam recursos com os fluxos de trabalho do usuário final. Esses problemas podem ser pronunciados com fluxos de trabalho de autenticação, como redefinições de senha, que são feitas em tempo real com um usuário final aguardando a conclusão do processo. Fornecendo uma instância do Serviço do MIM para operações de usuário final e um portal separado para a sincronização de dados administrativos, você pode fornecer melhor capacidade de resposta para operações de usuário final.

![Vários diagramas de topologia de multicamadas do MIM](media/MIM-topo-multitier-multiservice.png)

Como com a topologia padrão de multicamadas, você pode aumentar o desempenho do Portal do MIM usando um cluster NLB e aumentando o número de nós no cluster conforme necessário.

Os computadores que executam o SQL Server e que hospedam o Serviço de Sincronização do MIM e o banco de dados do Serviço do MIM influenciarão drasticamente o desempenho geral da implantação do MIM. Portanto, siga as recomendações na documentação do SQL Server para otimizar o desempenho do banco de dados. Para obter mais informações, consulte os seguintes documentos:

## Consulte também
<a id="see-also" class="xliff"></a>
- O [Guia de planejamento de capacidade do FIM (Forefront Identity Manager) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) baixável apresenta mais detalhes sobre um build de teste e resultados de teste de desempenho.
