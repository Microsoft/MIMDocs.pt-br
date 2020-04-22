---
title: Guia de conceitos do Microsoft BHOLD Suite | Microsoft Docs
description: Comece a usar os componentes do MIM 2016 instalando e configurando o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: f120709e517d82d4f94e72f4d0a44361f5552a1c
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042297"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guia de conceitos do Microsoft BHOLD Suite

O MIM (Microsoft Identity Manager) 2016 permite que as organizações gerenciem todo o ciclo de vida das identidades de usuário e das credenciais associadas. Ele pode ser configurado para sincronizar identidades, gerenciar senhas e certificados de forma centralizada e provisionar usuários entre sistemas heterogêneos. Com o MIM, as organizações de TI podem definir e automatizar os processos usados para gerenciar identidades desde a criação até a desativação.

O Microsoft BHOLD Suite estende esses recursos do MIM, adicionando o controle de acesso baseado em função. O BHOLD permite que as organizações definam funções de usuário e controlem o acesso a aplicativos e dados confidenciais. O acesso baseia-se naquilo que é apropriado para essas funções. O BHOLD Suite inclui serviços e ferramentas que simplificam a modelagem dos relacionamentos de função na organização. O BHOLD mapeia essas funções para direitos e para verificar se as definições de função e os direitos associados estão aplicados corretamente aos usuários. Esses recursos são totalmente integrados ao MIM, fornecendo uma experiência perfeita aos usuários finais e também à equipe de TI.

Este guia ajuda a entender como o BHOLD Suite funciona com o MIM e abrange os seguintes tópicos:

- Controle de acesso baseado em funções
- Atestado
- Análises
- Relatórios
- Conector de Gerenciamento de Acesso
- Integração ao MIM

## <a name="role-based-access-control"></a>Controle de acesso baseado em funções

O método mais comum para controlar o acesso de usuário a dados e aplicativos é por meio do DAC (controle de acesso discricionário). Nas implementações mais comuns, cada objeto significativo tem um proprietário identificado. O proprietário tem a capacidade de conceder ou negar acesso ao objeto para outras pessoas com base na identidade individual ou na associação a um grupo. Na prática, o DAC normalmente resulta em uma grande quantidade de grupos de segurança, alguns que refletem a estrutura organizacional, outros que representam agrupamentos funcionais (como tipos de trabalho ou atribuições de projeto) e outros que consistem em coleções provisórias de usuários e dispositivos que são vinculados para fins mais temporários. Conforme as organizações crescem, a associação a esses grupos se torna cada vez mais difícil de gerenciar. Por exemplo, se um funcionário é transferido de um projeto para outro, os grupos usados para controlar o acesso aos ativos dos projetos precisam ser atualizados manualmente. Nesses casos, não é incomum a ocorrência de erros que podem prejudicar a segurança ou a produtividade do projeto.

O MIM inclui recursos que ajudam a reduzir esse problema fornecendo o controle automatizado sobre a associação a um grupo e a uma lista de distribuição. No entanto, isso não resolve a complexidade intrínseca da proliferação de grupos que não estão necessariamente relacionados entre si de uma maneira estruturada.

Uma maneira de reduzir significativamente essa proliferação é implantando o RBAC (controle de acesso baseado em função). O RBAC não substitui o DAC.  O RBAC baseia-se no DAC, fornecendo uma estrutura para classificar os usuários e os recursos de TI. Assim, é possível deixar explícitos o relacionamento entre eles e os direitos de acesso apropriados de acordo com essa classificação. Por exemplo, ao atribuir a um usuário atributos que especificam o cargo do usuário e as atribuições do projeto, o usuário pode receber acesso às ferramentas necessárias para o trabalho do usuário e aos dados que ele precisa para contribuir em um projeto específico. Quando o usuário assume um trabalho diferente e atribuições de projeto diferentes, alterar os atributos que especificam o cargo e os projetos do usuário bloqueia automaticamente o acesso aos recursos que eram necessários apenas para a posição anterior do usuário.

Como as funções podem estar contidas dentro de funções de maneira hierárquica, (por exemplo, as funções de gerente de vendas e de representante de vendas podem estar contidas na função mais geral de vendas), é fácil atribuir os direitos apropriados a funções específicas e ainda assim fornecer direitos apropriados para todas as pessoas que também compartilham a função mais abrangente. Por exemplo, em um hospital, toda a equipe de assistência médica pode receber o direito de exibir os registros dos pacientes, mas somente os médicos (uma subfunção da função de assistência médica) podem receber o direito de inserir indicações para o paciente. Da mesma forma, os usuários que pertencem à função administrativa podem ter o acesso negado aos registros de pacientes, exceto os administradores de cobrança (uma subfunção da função administrativa), que podem receber acesso às partes dos registros de pacientes que são necessárias para cobrar o paciente pelos serviços fornecidos pelo hospital.

Um benefício adicional do RBAC é a capacidade de definir e impor a SoD (separação de obrigações). Isso permite que a organização defina combinações de funções que concedem permissões que não devem ser mantidas pelo mesmo usuário, por exemplo, para que um usuário específico não possa receber funções que permitam iniciar um pagamento e também autorizar um pagamento. O RBAC fornece a capacidade de impor essa política automaticamente em vez de precisar avaliar a implementação de política efetiva para cada usuário.

### <a name="bhold-role-model-objects"></a>Objetos de modelo de função do BHOLD

Com o BHOLD Suite, você pode especificar e organizar funções em sua organização, mapear usuários a funções e mapear as permissões apropriadas às funções. Essa estrutura é chamada de modelo de função. Ela contém e conecta cinco tipos de objetos: 

- Unidades organizacionais
- Usuários
- Funções
- Permissões
- Aplicativo

#### <a name="organizational-units"></a>Unidades organizacionais

As OrgUnits (unidades organizacionais) são o principal meio pelo qual os usuários são organizados no modelo de função do BHOLD. Cada usuário deve pertencer a pelo menos uma OrgUnit. (Na verdade, quando um usuário é removido da última unidade organizacional no BHOLD, o registro de dados do usuário é excluído do banco de dados do BHOLD.)

> [!Important]
> As unidades organizacionais no modelo de função do BHOLD não devem ser confundidas com as unidades organizacionais no AD DS (Active Directory Domain Services). Normalmente, a estrutura da unidade organizacional do BHOLD baseia-se na organização e nas políticas da sua empresa, não nos requisitos da sua infraestrutura de rede.

Embora não seja necessário, na maioria dos casos, as unidades organizacionais são estruturadas no BHOLD para representar a estrutura hierárquica da organização real, semelhante à mostrada abaixo:

![](media/bhold-concepts-guide/org-chart.png)

Neste exemplo, o modelo de função conteria uma orgunit para a empresa como um todo (representada pelo presidente, porque o presidente não faz parte de uma orgunit mais específica) ou a unidade organizacional raiz do BHOLD (que sempre existe) poderia ser usada para essa finalidade. As OrgUnits que representam as divisões corporativas chefiadas pelos vice-presidentes seriam colocadas na unidade organizacional corporativa. Em seguida, as unidades organizacionais, correspondentes aos diretores de marketing e vendas seriam adicionadas às unidades organizacionais de marketing e vendas, e as unidades organizacionais que representam os gerentes de vendas regionais seriam colocadas na unidade organizacional do gerente de vendas da região leste. Os vendedores, que não gerenciam outros usuários, seriam colocados como membros da unidade organizacional do gerente de vendas de região leste. Observe que os usuários podem ser membros de uma unidade organizacional em qualquer nível. Por exemplo, um assistente administrativo, que não é gerente e se reporta diretamente a um vice-presidente, seria membro da unidade organizacional do vice-presidente.

Além de representar a estrutura organizacional, as unidades organizacionais também podem ser usadas para agrupar usuários e outras unidades organizacionais de acordo com critérios funcionais, por exemplo, para projetos ou especialização. O diagrama a seguir mostra como as unidades organizacionais seriam usadas para agrupar os vendedores de acordo com o tipo de cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

Neste exemplo, cada vendedor pertenceria a duas unidades organizacionais: uma que representaria o local do vendedor na estrutura de gerenciamento da organização e uma que representaria a base de clientes do vendedor (varejo ou corporativa). Cada unidade organizacional pode receber funções diferentes que, por sua vez, podem receber permissões diferentes para acessar os recursos de TI da organização. Além disso, as funções podem ser herdadas de unidades organizacionais pai, simplificando o processo de propagação de funções para usuários. Por outro lado, as funções específicas podem ser impedidas de ser herdadas, garantindo que uma função específica seja associada somente às unidades organizacionais apropriadas.

As OrgUnits podem ser criadas no BHOLD Suite usando o portal da Web do BHOLD Principal ou usando o Gerador de Modelo do BHOLD.

#### <a name="users"></a>Usuários

Conforme observado acima, cada usuário deve pertencer a pelo menos uma OrgUnit (unidade organizacional). Como as unidades organizacionais são o mecanismo principal para associar um usuário a funções, na maioria das organizações, um determinado usuário pertence a várias OrgUnits para facilitar a associação de funções a esse usuário. Em alguns casos, no entanto, pode ser necessário associar uma função a um usuário, além das OrgUnits às quais ele já pertença. Consequentemente, um usuário pode ser atribuído diretamente a uma função e também obter as funções das OrgUnits às quais pertence.

Quando um usuário não está ativo na organização (durante um período de licença médica, por exemplo), ele pode ser suspenso, o que revoga todas as permissões do usuário sem remover o usuário do modelo de função. Ao retornar às suas obrigações, o usuário pode ser reativado, o que restaura todas as permissões concedidas pelas funções do usuário.

Os objetos para os usuários podem ser criados individualmente no BHOLD por meio do portal da Web do BHOLD Principal ou podem ser importados em massa usando o Gerador de Modelo do BHOLD ou usando o Conector de Gerenciamento de Acesso com o Serviço de Sincronização do FIM para importar as informações do usuário de fontes como o Active Directory Domain Services ou aplicativos de recursos humanos.

Os usuários podem ser criados diretamente no BHOLD sem usar o Serviço de Sincronização do FIM. Isso pode ser útil para a modelagem de funções em um ambiente de pré-produção ou de teste. Você também pode permitir que usuários externos (como funcionários de um subcontratado) sejam atribuídos a funções e, portanto, recebam acesso a recursos de TI sem serem adicionados ao banco de dados de funcionários. No entanto, esses usuários não poderão usar os recursos de autoatendimento do BHOLD.

#### <a name="roles"></a>Funções

Como mencionado anteriormente, no modelo RBAC (controle de acesso baseado em função), as permissões são associadas a funções e não a usuários individuais. Isso permite conceder a cada usuário as permissões necessárias para executar suas obrigações alterando as funções do usuário e não concedendo ou negando separadamente as permissões do usuário. Como consequência, a atribuição de permissões não exige mais a participação do departamento de TI, ela pode ser executada como parte do gerenciamento do negócio. Uma função pode agregar permissões para acessar diferentes sistemas, seja diretamente ou usando subfunções, o que reduz ainda mais a necessidade de envolvimento de TI no gerenciamento de permissões de usuário.

É importante observar que as funções são um recurso do próprio modelo de RBAC. Normalmente, as funções não são provisionadas para aplicativos de destino. Isso permite que o RBAC seja usado junto com os aplicativos existentes que não são projetados para usar funções ou para alterar as definições de função a fim de atender às mudanças nas necessidades dos modelos de negócios, sem precisar modificar os aplicativos em si. Se um aplicativo de destino for projetado para usar funções, você poderá associar as funções no modelo de função do BHOLD com as funções do aplicativo correspondentes, tratando as funções específicas do aplicativo como permissões.

No BHOLD, você pode atribuir uma função a um usuário por meio de dois mecanismos principais:

- Atribuindo uma função a uma orgunit (unidade organizacional) da qual o usuário é membro
- Atribuindo uma função diretamente a um usuário

Uma função atribuída a uma unidade organizacional pai pode ser herdada opcionalmente pelas suas unidades organizacionais membros. Quando uma função é atribuída ou herdada por uma unidade organizacional, ela pode ser designada como uma função efetiva ou proposta. Se ela for uma função efetiva, todos os usuários na unidade organizacional receberão a função. Se for uma função proposta, ela deverá ser ativada para cada usuário ou unidade organizacional membro para tornar-se efetiva para esse usuário ou para os membros da unidade organizacional. Isso torna possível atribuir usuários um subconjunto das funções associadas a uma unidade organizacional, em vez de atribuir automaticamente todas as funções da unidade organizacional a todos os membros. Além disso, as funções podem receber datas de início e término e é possível colocar limites no percentual de usuários em uma unidade organizacional para a qual uma função pode ser efetiva.

O diagrama a seguir ilustra como um usuário individual pode receber uma função no BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Neste diagrama, a função A é atribuída a uma unidade organizacional como uma função herdável e, portanto, é herdada por suas unidades organizacionais membros e por todos os usuários nessas unidades organizacionais. A função B é atribuída como uma função proposta para uma unidade organizacional. Ela precisa ser ativada para que um usuário na unidade organizacional possa ser autorizado com as permissões da função. A função C é uma função efetiva, portanto, suas permissões aplicam-se imediatamente a todos os usuários na unidade organizacional. A função D está vinculada diretamente ao usuário, portanto, suas permissões aplicam-se imediatamente a esse usuário.

Além disso, uma função pode ser ativada para um usuário com base nos atributos do usuário. Para obter mais informações, consulte Autorização baseada em atributo.

#### <a name="permissions"></a>Permissões

Uma permissão no BHOLD corresponde a uma autorização importada de um aplicativo de destino. Ou seja, quando o BHOLD é configurado para funcionar com um aplicativo, ele recebe uma lista de autorizações que pode vincular a funções. Por exemplo, quando o AD DS (Active Directory Domain Services) é adicionado ao BHOLD como um aplicativo, ele recebe uma lista de grupos de segurança que, como permissões do BHOLD, podem ser vinculados a funções no BHOLD.

As permissões são específicas dos aplicativos. O BHOLD fornece uma exibição de permissões unificada e exclusiva para que as permissões possam ser associadas a funções sem precisar de gerenciadores de função para entender os detalhes de implementação das permissões. Na prática, sistemas diferentes podem impor permissões diferentes. O conector específico do aplicativo do Serviço de Sincronização do FIM para o aplicativo determina como as alterações de permissão para um usuário são fornecidas para esse aplicativo. 

#### <a name="applications"></a>Aplicativo

O BHOLD implementa um método para a aplicação de RBAC (controle de acesso baseado em função) a aplicativos externos. Ou seja, quando o BHOLD recebe usuários e permissões de um aplicativo, ele pode associar essas permissões aos usuários atribuindo funções aos usuários e, em seguida, vinculando as permissões às funções. O processo em segundo plano do aplicativo pode mapear as permissões corretas para seus usuários com base no mapeamento de função/permissão no BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Desenvolvendo o modelo de função do BHOLD Suite

Para ajudá-lo a desenvolver seu modelo de função, o BHOLD Suite fornece o Gerador de Modelo, uma ferramenta fácil de usar e abrangente.

Antes de usar o Gerador de Modelo, você deve criar uma série de arquivos que definem os objetos que o Gerador de Modelo usa para construir o modelo de função. Para obter informações de como criar esses arquivos, consulte a Microsoft BHOLD Suite Technical Reference (Referência técnica do Microsoft BHOLD Suite).

O primeiro passo para usar o Gerador de Modelo do BHOLD é importar esses arquivos para carregar os blocos de construção básicos no Gerador de Modelo. Depois que os arquivos forem carregados com êxito, você poderá especificar os critérios que o Gerador de Modelo usará para criar várias classes de funções:

- As funções de associação que são atribuídas a um usuário com base nas OrgUnits (unidades organizacionais) às quais o usuário pertence
- As funções de atributo que são atribuídas a um usuário com base nos atributos do usuário no banco de dados do BHOLD
- As funções propostas que são vinculadas a uma unidade organizacional, mas que devem ser ativadas para usuários específicos
- As funções de propriedade que concedem a um usuário o controle de unidades organizacionais e funções para as quais não há um proprietário especificado nos arquivos importados

> [!Important]
> Ao carregar os arquivos, marque a caixa de seleção **Reter Modelo Existente** apenas em ambientes de teste. Em ambientes de produção, você deve usar o Gerador de Modelo para criar o modelo de função inicial. Você não pode usá-lo para modificar um modelo de função existente no banco de dados do BHOLD.

Depois que o Gerador de Modelo criar essas funções no modelo de função, você poderá exportar o modelo de função para o banco de dados BHOLD no formato de um arquivo XML.

### <a name="advanced-bhold-features"></a>Recursos avançados do BHOLD

As seções anteriores descreveram os recursos básicos de RBAC (controle de acesso baseado em função) no BHOLD. Esta seção descreve os recursos adicionais do BHOLD que podem oferecer maior segurança e flexibilidade para a implementação de RBAC na sua organização. Esta seção fornece uma visão geral dos recursos do BHOLD a seguir:

- Cardinalidade
- Separação de obrigações
- Permissões adaptáveis ao contexto
- Autorização baseada em atributo
- Tipos de atributo flexíveis


#### <a name="cardinality"></a>Cardinalidade

*Cardinalidade* refere-se à implementação de regras de negócio que são criadas para limitar o número de vezes que duas entidades podem ser relacionadas entre si. No caso do BHOLD, as regras de cardinalidade podem ser estabelecidas para usuários, funções e permissões.

Você pode configurar uma função para limitar o seguinte:

- O número máximo de usuários para os quais uma função proposta pode ser ativada
- O número máximo de subfunções que podem ser vinculadas à função
- O número máximo de permissões que podem ser vinculadas à função

Você pode configurar uma permissão para limitar o seguinte:

- O número máximo de funções que podem ser vinculadas à permissão
- O número máximo de usuários que podem receber a permissão

Você pode configurar um usuário para limitar o seguinte:

- O número máximo de funções que podem ser vinculadas ao usuário
- O número máximo de permissões que podem ser atribuídas ao usuário por meio de atribuições de função

#### <a name="separation-of-duties"></a>Separação de obrigações

A SoD (Separação de Funções) é um princípio de negócios que busca impedir que indivíduos obtenham a capacidade de executar ações que não devem estar disponíveis para a mesma pessoa. Por exemplo, um funcionário não deve ter a capacidade de solicitar um pagamento e também de autorizar o pagamento. O princípio de SoD permite que as organizações implementem um sistema de verificações e balanços para minimizar sua exposição ao risco de erro ou má conduta de funcionário.

O BHOLD implementa a SoD, permitindo que você defina permissões incompatíveis. Quando essas permissões são definidas, o BHOLD impõe a SoD, impedindo a criação de funções vinculadas a permissões incompatíveis, sejam elas vinculadas diretamente ou por meio da herança, e impedindo que os usuários recebam várias funções que, quando combinadas, possam conceder permissões incompatíveis, novamente, por atribuição direta ou herança. Opcionalmente, os conflitos podem ser substituídos.

#### <a name="context-adaptable-permissions"></a>Permissões adaptáveis ao contexto

Criando permissões que podem ser modificadas automaticamente com base em um atributo de objeto, você pode reduzir o número total de permissões que precisa gerenciar. As CAPs (permissões adaptáveis ao contexto) permitem que você defina uma fórmula como um atributo de permissão que modifica como a permissão é aplicada pelo aplicativo associado à permissão. Por exemplo, você pode criar uma fórmula que altere as permissões de acesso a uma pasta de arquivo (por meio de um grupo de segurança associado à lista de controle de acesso da pasta) com base em se um usuário pertence a uma orgunit (unidade organizacional) que contém funcionários em tempo integral ou funcionários com outro tipo de contrato. Se o usuário for movido de uma unidade organizacional para outra, a nova permissão será aplicada automaticamente e a permissão antiga será desativada. 

A fórmula de CAP pode consultar os valores dos atributos que foram aplicados a aplicativos, unidades organizacionais, usuários e permissões.

#### <a name="attribute-based-authorization"></a>Autorização baseada em atributo

Uma maneira de controlar se uma função que está vinculada a uma orgunit (unidade organizacional) está ativada para um usuário específico na unidade organizacional é usar a ABA (autorização baseada em atributo). Usando a ABA, você pode ativar automaticamente uma função somente quando certas regras com base nos atributos do usuário são atendidas. Por exemplo, você pode vincular uma função a uma unidade organizacional que se tornará ativa para um usuário somente se o cargo do usuário corresponder ao nome do cargo na regra da ABA. Isso elimina a necessidade de ativar manualmente uma função proposta para um usuário. Nesse caso, uma função poderá ser ativada para todos os usuários em uma unidade organizacional que tiverem um valor de atributo que satisfaça a regra de ABA da função. As regras podem ser combinadas para que uma função seja ativada apenas quando os atributos de um usuário satisfizerem a todas as regras de ABA especificadas para a função.

É importante observar que os resultados dos testes de regras de ABA são limitados pelas configurações de cardinalidade. Por exemplo, se a configuração de cardinalidade de uma regra especificar que no máximo dois usuários podem receber uma função e uma regra de ABA ativar uma função para quatro usuários, a função será ativada somente para os dois primeiros usuários que passarem no teste de ABA.

#### <a name="flexible-attribute-types"></a>Tipos de atributo flexíveis

O sistema de atributos no BHOLD é altamente extensível. Você pode definir novos tipos de atributo para esses objetos como usuários, orgunits (unidades organizacionais) e funções. Os atributos podem ser definidos com valores inteiros, boolianos (sim/não), alfanuméricos, de data, hora e endereços de email. Os atributos podem ser especificados como valores únicos ou como uma lista de valores.

## <a name="attestation"></a>Atestado

O BHOLD Suite fornece ferramentas que você pode usar para verificar se usuários individuais receberam as permissões adequadas para realizar suas tarefas de negócios. O administrador pode usar o portal fornecido pelo módulo Atestado do BHOLD para criar e gerenciar um processo de atestado.

O processo de atestado é realizado por meio de campanhas nas quais os administradores recebem a oportunidade e os meios de verificar se os usuários pelos quais eles são responsáveis têm o acesso apropriado aos aplicativos gerenciados pelo BHOLD e as permissões corretas nesses aplicativos. O proprietário de uma campanha é designado para supervisionar a campanha e para verificar se a campanha está sendo executada corretamente. Uma campanha pode ser criada para ocorrer uma única vez ou de forma recorrente.

Normalmente, o administrador de uma campanha será um gerente que atestará os direitos de acesso dos usuários que pertencem a uma ou mais unidades organizacionais pelas quais o gerente é responsável. Os administradores podem ser selecionados automaticamente para os usuários que estão sendo atestados em uma campanha com base nos atributos do usuário, ou os administradores de uma campanha podem ser definidos por meio de uma listagem em um arquivo que mapeie cada usuário que está sendo atestado na campanha a um administrador.

Quando uma campanha é iniciada, o BHOLD envia uma mensagem de notificação por email aos proprietários e administradores da campanha e, em seguida, envia lembretes periódicos para ajudá-lo a manter o andamento da campanha. Os administradores são direcionados a um portal de campanha no qual é exibida uma lista dos usuários pelos quais eles são responsáveis e as funções atribuídas a esses usuários. Assim, os administradores podem confirmar se eles são responsáveis por todos os usuários listados e aprovar ou negar os direitos de acesso de todos os usuários listados.

Os proprietários de campanha também usam o portal para monitorar o andamento da campanha e as atividades da campanha são registradas para que proprietários da campanha possam analisar as ações que foram executadas no decorrer da campanha.

## <a name="analytics"></a>Análises

Uma das considerações importantes ao implementar um sistema abrangente de RBAC (controle de acesso baseado em direitos) é o equilíbrio entre manter o controle de acesso restrito e evitar barreiras de acesso desnecessárias (ou pior, inesperadas). O esforço necessário para manter esse equilíbrio geralmente resulta em uma estrutura de controle de acesso tão complexa que interações inesperadas entre as políticas são praticamente inevitáveis.

Por esse motivo, é importante ser capaz de analisar os efeitos das políticas de controle de acesso antes que elas realmente entrem em vigor. O módulo de análise do BHOLD Suite oferece a capacidade de executar essa análise por permitir que você desenvolva as regras que representam suas políticas e, em seguida, mostrar os usuários cujas permissões estão em conformidade ou em conflito com as regras. Com base nessa análise, você pode ajustar a política ou modificar as funções e permissões para eliminar os conflitos com a política.

O portal de Análise do BHOLD oferece a capacidade de construir conjuntos de regras que consistem em uma ou mais regras que você cria para testar uma política específica ou um grupo de políticas. Uma regra consiste nas seguintes partes principais:

- Um título e uma descrição que permitem identificar e descrever a regra
- Um status que indica se a regra está pronta para análise, sendo examinada ou aprovada
- Um conjunto de elementos (como usuários ou permissões) que a regra foi criada para testar
- Filtros de subconjunto opcionais que são expressões que você pode usar para selecionar um subgrupo apropriado do elemento a ser examinado
- Um ou mais filtros de regra são expressões que representam a política que está sendo testada.

Uma regra pode testar qualquer um dos seguintes conjuntos de elemento:

- Usuários
- Unidades Organizacionais
- Funções
- Permissões
- Aplicativo
- Contas

O diagrama a seguir ilustra uma regra simples que consiste em duas regras de subconjunto e em duas regras de filtro:

![](media/bhold-concepts-guide/rules.png)

Observe a diferença no efeito da falha de um filtro de subconjunto e da falha de um filtro de regra: a falha de um filtro de subconjunto remove o objeto de elemento do teste dos próximos filtros, enquanto a falha de um filtro de regra faz com que o objeto seja relatado como fora de conformidade. Apenas os objetos que passam por todos os filtros de subconjunto e por todos os filtros de regra são relatados como em conformidade.

Cada filtro consiste em um tipo, um operador (que depende do tipo), uma chave (um dos elementos) e um valor usado pelo operador para testar a chave. Por exemplo, o filtro a seguir testa se o número de usuários em um subconjunto de elemento excede 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Número de   |
| **Chave:**  | Usuários  |
| **Operador**  | >  |
| **Valor:** | 10 |

Os filtros de regras podem ser de três tipos e usar operadores específicos para seu tipo, conforme indicado:

- Atributo
  - < and >
  - = and !=
  - **Contém**
  - **Não contém**
- Número de
  - < and >
  - = and !=
- Restritiva
  - **Deve ter qualquer um e Deve ter todos**
  - **Não pode ter nenhum e Não pode ter todos**
  - **Pode apenas ter qualquer um e Pode apenas ter todos**
  - **Ter exclusivamente qualquer um e Ter exclusivamente todos**

> [!Note]
> Os filtros restritivos podem usar os operadores indicados para testar uma chave em relação a um conjunto de vários valores.

Por exemplo, se você quiser testar a implementação de uma política de SoD (separação de obrigações) que indique que nenhum usuário que tenha a permissão Solicitar pagamento também tenha a permissão Aprovar pagamento, construa uma regra semelhante à seguinte:

|   |  |
|---|--|
|Nome:| Teste de SoD de pagamento|
|Elemento:| Usuários|
|Filtro de subconjunto:| Ter qualquer permissão Solicitar Pagamento|
|Filtro de regra: | Não pode ter nenhuma permissão Aprovar Pagamento|

Quando você executar esta regra, o módulo de Análise do BHOLD exibirá o número de usuários no subconjunto selecionado (o número de usuários com a permissão Solicitar Pagamento), o número de usuários que estão em conformidade com a regra e o número de usuários que não estão em conformidade com a regra. É possível exibir os usuários que estão em conformidade para que você possa corrigir a inconsistência.

Além de exibir os resultados, também é possível salvar o relatório como um arquivo CSV (valor separado por vírgula) ou XML para permitir que você analise os resultados mais tarde. Você também pode personalizar o relatório resultante para mostrar informações adicionais que podem ajudá-lo a entender melhor o impacto. Por exemplo, ao testar os usuários, será possível exibir (ou relatar) as contas dos usuários em conformidade ou fora de conformidade para que você possa ver quais aplicativos estão envolvidos.

Como uma regra pode conter vários filtros, você pode se conectar os filtros para testar se existe uma determinada combinação de condições. Por padrão, o resultado é o produto de um teste booliano AND de todos os filtros, mas você pode especificar que seja executado um teste OR da combinação de filtros.

Por exemplo, se sua política de negócios exige que os gerentes tenham a permissão Modificar Pagamento ou a permissão Aprovar Pagamento, você pode testar a conformidade com a política criando uma regra como a seguinte:


|  |  |
|--|--|
|Nome: | Modificar teste de SoD de pagamento|
|Elemento: | Usuários |
|Filtro de subconjunto: | Ter qualquer função de Gerente|
| Filtros de regra: |Deve ter qualquer permissão Modificar Pagamento </br> Deve ter qualquer permissão Aprovar Pagamento|

Por padrão, qualquer usuário que seja gerente e tenha as permissões Modificar Pagamento e Aprovar Pagamento será relatado como em conformidade. No entanto, a política requer que um gerente tenha uma das permissões, não necessariamente as duas. Para testar a conformidade real com a política, você deve usar o operador booliano OR com a regra para determinar se existe algum gerente que não tenha nem a permissão Modificar Pagamento nem a permissão Aprovar Pagamento.

Ao contrário de outros operadores, os operadores **Ter exclusivamente qualquer um** e **Ter exclusivamente todos** indicam conformidade para objetos que, caso contrário, seriam excluídos por um filtro de subconjunto. Por exemplo, para testar uma política em que todos os gerentes (e apenas os gerentes) devem ter a permissão Aprovar Revisões, você pode construir uma regra da seguinte maneira:

|  |  |
|--|--|
|Nome: | Teste de aprovação da análise|
|Elemento: | Usuários|
| Filtro de subconjunto: | Ter qualquer função de Gerente
|Filtro de regra: | Ter exclusivamente qualquer permissão Aprovar Revisões|

Essa regra relatará como em conformidade os usuários que são gerentes e têm a permissão Aprovar Revisões e os usuários que não são gerentes e que não têm a permissão Aprovar Revisões. Por outro lado, os gerentes que não têm essa permissão e os usuários que não são gerentes mas têm essa permissão serão relatados como fora de conformidade.

Conforme observado anteriormente, você pode combinar regras em um conjunto de regras, facilitando a categorização e o gerenciamento de regras para atender às suas necessidades de negócios.

Você também pode definir um conjunto de filtros globais que, quando habilitados, se aplicam a qualquer regra testada. Se com frequência você precisa excluir um subconjunto de registros específico ao testar regras em diferentes conjuntos de regras, especifique filtros globais que você poderá habilitar ou desabilitar, conforme o necessário.

## <a name="reporting"></a>Relatórios

O módulo de Relatório do BHOLD oferece a capacidade de exibir informações no modelo de função por meio de uma variedade de relatórios. O módulo de Relatório do BHOLD fornece um amplo conjunto de relatórios internos, além de incluir um assistente que você pode usar para criar relatórios personalizados básicos e avançados. Ao executar um relatório, você pode exibir os resultados imediatamente ou salvá-los em um arquivo do Microsoft Excel (.xlsx). Para exibir esse arquivo usando o Microsoft Excel 2000, o Microsoft Excel 2002 ou o Microsoft Excel 2003, você pode baixar e instalar o Pacote de Compatibilidade do Microsoft Office para os Formatos de Arquivo Word, Excel e PowerPoint.


O módulo de Relatório do BHOLD é usado principalmente para produzir relatórios baseados nas informações de função atuais. Para gerar relatórios para auditar alterações nas informações de identidade, o Forefront Identity Manager 2010 R2 tem uma funcionalidade de relatório para o serviço do FIM que é implementado no Data Warehouse do System Center Service Manager. Para obter mais informações sobre os relatórios do FIM, consulte a documentação do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 na biblioteca técnica do Forefront Identity Management.

As categorias cobertas pelos relatórios internos incluem as seguintes:

- Administração
- Atestado
- Controles
- Controle de acesso interno
- Registrando em log
- Modelo
- Estatísticas
- Fluxo de Trabalho

Você pode criar relatórios e adicioná-los a essas categorias ou definir suas próprias categorias para colocar relatórios internos e personalizados.

Ao criar um relatório, o assistente o orientará o processo fornecendo os seguintes parâmetros:

- Informações de identificação, incluindo nome, descrição, categoria, uso e audiência
- Campos a serem exibidos no relatório
- Consultas que especificam quais itens devem ser analisados
- Ordem na qual as linhas devem ser classificadas
- Campos usados para dividir o relatório em seções
- Filtros para refinar os elementos que são retornados no relatório

Em cada etapa do assistente, você pode visualizar o relatório com as definições realizadas até o momento e salvá-lo caso não precise especificar outros parâmetros. Também é possível retornar às etapas anteriores para alterar os parâmetros que você já especificou no assistente.

## <a name="access-management-connector"></a>Conector de Gerenciamento de Acesso

O módulo do Conector de Gerenciamento de Acesso do BHOLD Suite oferece suporte para sincronização inicial e contínua de dados no BHOLD. O Conector de Gerenciamento de Acesso funciona com o Serviço de Sincronização do FIM para mover dados entre o banco de dados do BHOLD Principal, o metaverso do MIM e os aplicativos e repositórios de identidade de destino.

As versões anteriores do BHOLD exigiam vários MAs para controlar o fluxo de dados entre o MIM e as tabelas de banco de dados intermediárias do BHOLD. No BHOLD Suite SP1, o Conector de Gerenciamento de Acesso permite definir MAs (agentes de gerenciamento) no MIM que fornecem a transferência de dados diretamente entre o BHOLD e o MIM.

## <a name="mim-integration"></a>Integração ao MIM

Um recurso importante e poderoso do Forefront Identity Manager 2010 e o do Forefront Identity Manager 2010 R2 é o portal de autoatendimento que permite que os usuários finais exibam e gerenciem suas informações de identidade e de associação. A integração do MIM estende o Portal do MIM com o gerenciamento de função de autoatendimento. Por exemplo, usando os recursos do BHOLD no Portal do MIM, o usuário pode solicitar atribuição de função e exibir as funções ativas e as solicitações pendentes. É possível conceder recursos adicionais aos administradores delegados, como a capacidade de solicitar atribuições de função para outros usuários.

É importante observar que as extensões do BHOLD para o Portal do MIM dão suporte para gerenciamento e relatórios de função e de fluxo de trabalho de autoatendimento. Outras funções de administração do BHOLD, além de atestado, são fornecidas pelos portais da Web dos módulos do BHOLD, que são hospedados separadamente do Portal do MIM.

## <a name="next-steps"></a>Próximas etapas

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)
