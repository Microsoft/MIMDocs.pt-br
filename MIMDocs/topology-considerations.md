---
title: Guia de topologia de implantação | Microsoft Docs
description: Compreender os componentes do MIM 2016 e obter sugestões sobre como implantá-los em seu ambiente.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 42562e92b3fe0daa63110d33d8952a3a1fc3de17
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358073"
---
# <a name="topology-considerations"></a>Considerações de topologia
Você pode implantar os componentes do MIM (Microsoft Identity Manager) no mesmo servidor ou entre vários servidores em várias configurações. A topologia que você seleciona para sua implantação afeta o desempenho que você pode obter do MIM. Este artigo apresenta várias topologias de implantação que você pode considerar.


> [!NOTE]
> Essas opções são aplicáveis a implementações usando somente a Sincronização do MIM, o Serviço do MIM e o Portal do MIM para gerenciamento de identidades.  As implantações com MIM CM, Suite MIM BHOLD ou para gerenciamento de acesso privilegiado têm diferentes opções de implantação.


## <a name="mim-components"></a>Componentes do MIM
Ao projetar a topologia de implantação, é importante saber o que cada componente faz e como todos eles interagem.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**Portal do MIM** - uma interface para operações administrativas, gerenciamento de grupo e redefinições de senha.
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


## <a name="multitier-topology"></a>Topologia de multicamadas
A topologia de multicamadas é a topologia mais comumente normalmente. Ela oferece maior flexibilidade. O Portal do MIM, o Serviço do MIM e os bancos de dados são separados em camadas e implantados em vários computadores. Esta topologia adiciona flexibilidade na colocação em escala dos diferentes componentes do MIM. Por exemplo, você pode dimensionar o Portal do MIM horizontalmente adicionando mais servidores em um cluster de NLB (Balanceamento de Carga de Rede). Da mesma forma, você pode dimensionar o serviço do MIM usando um cluster NLB e aumentando o número de computadores (nós) do cluster conforme necessário.

Na topologia de multicamadas, um computador dedicado para hospedar cada banco de dados SQL (um para o serviço do MIM e outro para o Serviço de Sincronização do MIM) é alocado. A escalabilidade do desempenho dos computadores que hospedam os bancos de dados SQL pode ser aumentada adicionando ou atualizando o hardware, por exemplo, atualizando as CPUs, adicionando mais CPUs, aumentando ou atualizando a memória RAM ou atualizando as configurações de disco rígido para aumentar a leitura e acesso de gravação e diminuir a latência.

![Diagrama de topologia de multicamadas do MIM](media/MIM-topo-multitier.png)

Nessa configuração, o Serviço de Sincronização do MIM e seu banco de dados são hospedados no mesmo computador. No entanto, você deve ser capaz de alcançar um desempenho semelhante, se houver uma conexão de rede dedicada de um gigabit entre o Serviço de Sincronização do MIM e seu banco de dados quando eles são hospedados em computadores separados.


## <a name="multitier-topology-with-multiple-mim-services"></a>Topologia de multicamadas com vários serviços do MIM
A sincronização de dados com sistemas externos pode demorar bastante e acrescentar uma carga considerável ao sistema durante esse período. Se a configuração da sincronização resulta em disparo de políticas com fluxos de trabalho, essas políticas disputam recursos com os fluxos de trabalho do usuário final. Esses problemas podem ser pronunciados com fluxos de trabalho de autenticação, como redefinições de senha, que são feitas em tempo real com um usuário final aguardando a conclusão do processo. Fornecendo uma instância do Serviço do MIM para operações de usuário final e um portal separado para a sincronização de dados administrativos, você pode fornecer melhor capacidade de resposta para operações de usuário final.

![Vários diagramas de topologia de multicamadas do MIM](media/MIM-topo-multitier-multiservice.png)

Como com a topologia padrão de multicamadas, você pode aumentar o desempenho do Portal do MIM usando um cluster NLB e aumentando o número de nós no cluster conforme necessário.

Os computadores que executam o SQL Server e que hospedam o Serviço de Sincronização do MIM e o banco de dados do Serviço do MIM influenciarão drasticamente o desempenho geral da implantação do MIM. Portanto, siga as recomendações na documentação do SQL Server para otimizar o desempenho do banco de dados. Para obter mais informações, consulte os seguintes documentos:

## <a name="see-also"></a>Consulte também
- O [Guia de planejamento de capacidade do FIM (Forefront Identity Manager) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) baixável apresenta mais detalhes sobre um build de teste e resultados de teste de desempenho.
