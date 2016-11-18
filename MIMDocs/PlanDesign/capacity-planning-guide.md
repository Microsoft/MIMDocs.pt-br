---
title: Guia de planejamento de capacidade | Microsoft Docs
description: "Use este guia para entender as variáveis que devem ser consideradas antes de implantar o MIM 2016, incluindo níveis de carga e decisões de política."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 33e7d901dcd9d8cf1bcaddcfa521625060bb3dbc


---

# <a name="capacity-planning-guide"></a>Guia de planejamento de capacidade

O MIM (Microsoft Identity Manager) permite criar, atualizar e remover contas de usuário em toda a sua organização. Ele também permite aos usuários finais gerenciar os recursos de autoatendimento de suas próprias contas. Mesmo em um ambiente pequeno, todas essas ações podem se acumular rapidamente.

Antes de começar a usar o MIM, siga este guia em ambientes de teste para entender o escopo apropriado da sua implantação. Este artigo explica muitos fatores comuns que devem ser levados em consideração. No entanto, uma vez que cada implantação é exclusiva, testar seus cenários em um laboratório ainda é a melhor maneira de determinar os servidores, o hardware ou as topologias associados às suas necessidades.

Se você ainda não estiver familiarizado com o MIM 2016 e seus componentes, obtenha mais detalhes sobre o [Microsoft Identity Manager 2016](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016) antes de continuar.

## <a name="overview"></a>Visão geral
Há várias variáveis que podem afetar o desempenho e a capacidade total da sua implantação do Microsoft Identity Manager. As maneiras nas quais você implanta fisicamente os componentes do MIM (topologia), bem como o hardware no qual os componentes são executados, são fatores importantes na determinação do desempenho e da capacidade que você pode esperar da implantação do MIM. O número e a complexidade dos objetos de configuração da política do MIM podem ser menos óbvios, mas eles ainda são fatores significativos a serem considerados no planejamento de capacidade. Finalmente, a escala esperada da implantação, bem como a carga que deve ser colocada nela, são geralmente fatores mais óbvios que afetam o desempenho e a capacidade.

Os principais fatores que afetam a capacidade e o desempenho que podem ser esperados de uma implantação de MIM 2016 são descritos na tabela a seguir.

| Fator de design | Considerações |
| ------------- | -------------- |
| Topologia | A distribuição dos serviços do MIM entre computadores na rede. |
| Hardware | O hardware físico e quaisquer especificações de hardware virtualizadas que você está executando para cada componente do MIM. Isso inclui a CPU, a memória, o adaptador de rede e a configuração do disco rígido. |
| Objetos de configuração de política do MIM | O número e tipo dos objetos de configuração de política do MIM, que inclui os conjuntos, as MPRs (regras de política de gerenciamento) e os fluxos de trabalho. |
| Escala | O número de usuários, grupos, grupos calculados e tipos de objetos personalizados, como computadores a serem gerenciados pelo MIM 2016. Além disso, considere a complexidade de grupos dinâmicos e certifique-se de considerar o aninhamento de grupos. |
| Carregamento | Frequência de uso. Por exemplo, com que frequência você espera que grupos ou usuários novos sejam criados, senhas sejam redefinidas ou o portal seja visitado em um determinado período de tempo. Observe que a carga pode variar durante o curso de uma hora, dias, semanas ou anos. Dependendo do componente, é possível optar por carregamento de pico ou carregamento médio. |


## <a name="hosting-microsoft-identity-manager-components"></a>Componentes de hospedagem do Microsoft Identity Manager

Os componentes do Microsoft Identity Manager não precisam estar localizados no mesmo computador. Pensar nesses componentes, e nas máquinas físicas ou virtuais que os hospedam, é uma parte importante do planejamento de capacidade.

Fatores de hardware podem afetar o desempenho do ambiente do MIM. Por exemplo:
- Qual é a configuração de disco físico do computador que executa o banco de dados SQL de Serviço do MIM 2016? O número de eixos que compõem a configuração de disco ou a distribuição de arquivos de dados e log pode afetar significativamente o desempenho do sistema.

Além disso, pense nos fatores externos em sua configuração. Por exemplo:
- Se você estiver usando uma SAN como a configuração do banco de dados de serviço do MIM 2016, quais os outros aplicativos que estão compartilhando a SAN? Esses aplicativos podem afetar o desempenho do banco de dados e como eles competem por recursos de disco compartilhados na SAN.


## <a name="users-and-groups"></a>Usuários e grupos
O número de usuários e grupos em seu ambiente é uma consideração típica quando você pensa sobre a escala de uma implantação. No entanto, há várias outras considerações relacionadas que você também deve incluir no planejamento.

- Os usuários podem criar grupos? Neste caso, você deve considerar a estimativa de quantos usuários criando grupos afetará o crescimento de grupos em seu ambiente.

- Grupos dinâmicos serão implantados? Calcule quantos e quais tipos de grupos dinâmicos esperar no ambiente.


## <a name="expected-load-levels"></a>Níveis de carga esperados
Você também deve considerar o tipo de carga que será colocada nos componentes do MIM. Provavelmente, essa informação pode ser estimada examinando os aplicativos atuais no ambiente. Algumas questões relevantes a serem consideradas incluem o seguinte:

- Com que frequência você espera uma solicitação para ingressar ou sair de um grupo?

- Com que frequência você espera que um usuário crie um grupo estático ou dinâmico?

- Quantas operações não orientadas pelo usuário você espera, como a sincronização de alterações de sistemas externos? Assegure-se de que você é responsável pela carga que é gerada pela sincronização das informações de identidade com sistemas externos.

- Que tipo de cenários você planeja implantar? Cenários diferentes contribuirão para padrões de carga diferentes. Por exemplo, computadores cliente que têm o cliente do MIM 2016 instalado validado periodicamente se o registro é necessário ao entrar, o que aumenta a carga do sistema.

- Você espera grandes variações em níveis de carga de normal a pico de carga? Por exemplo, há uma tendência de que ocorram muitas redefinições de senha após feriados. Certifique-se de que você trabalha suas agendas de sincronização e manutenção do sistema fora dos picos de uso antecipados. Quando você pensar sobre planejamento de capacidade, certifique-se de considerar os períodos de pico de carga.


## <a name="policy-configuration-objects"></a>Objetos de configuração de política

Os objetos de configuração de política do Microsoft Identity Manager incluem MPRs, conjuntos, fluxos de trabalho e regras de sincronização de uma determinada implantação. As implantações do MIM são exclusivas a cada cliente, pois a configuração da política muda para se adequar às necessidades de cada implantação. Considerações de desempenho chave relacionadas aos objetos de configuração de política do MIM incluem o seguinte:

- **Conjuntos** Cada operação do sistema deve ser avaliada nas atualizações e associações de conjuntos existentes que causam alterações na associação de conjuntos existentes. Por exemplo, uma alteração simples, como alterar o número de construção do escritório de um indivíduo, pode não ter um grande impacto. No entanto, outras alterações podem ter um impacto em cascata, como a alteração de um gerente, o que pode afetar vários objetos em vários níveis.

- **Regras de Política de Gerenciamento** As MPRs gerenciam as regras de controle de acesso e disparam fluxos de trabalho. À media que cria MPRs, talvez seja preciso aumentar o número de conjuntos para que você possa capturar vários estados de transição do objeto. Esses conjuntos adicionais podem disparar fluxos de trabalho adicionais, com cada mapeamento de fluxo de trabalho para solicitações únicas no sistema. Isso torna-se então outro item para incluir ao planejar a capacidade.

A configuração da política do MIM também inclui decisões sobre provisionamento no ambiente. Não se esqueça de considerar os seguintes pontos:

- Princípios de segurança externos serão provisionados por meio de várias florestas do AD DS (Serviços de Domínio do Active Directory)? Isso gerará mais fluxos de trabalho e solicitações, que resulta em uma carga adicional no sistema.

- Você usará o provisionamento sem código? Nesse caso, isso afeta o número de entradas de regras esperadas, bem como solicitações associadas e fluxos de trabalho no sistema.


## <a name="see-also"></a>Consulte também
- [Considerações de topologia para implantação do MIM](topology-considerations.md)
- O [Guia de planejamento de capacidade do FIM (Forefront Identity Manager) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) baixável apresenta mais detalhes sobre um build de teste e resultados de teste de desempenho.



<!--HONumber=Nov16_HO2-->


