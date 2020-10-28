---
title: Terminologia Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Lista abrangente de termos que são referenciados no Microsoft Identity Manager 2016 SP1.
keywords: Terminologia
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/28/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: afe167cdcd6ca548ef34e802f5606bee6ba5b31e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757478"
---
# <a name="microsoft-identity-manager-2016-sp1-terminology"></a>Terminologia do Microsoft Identity Manager 2016 SP1

Este documento é uma lista abrangente de termos que são referenciados no Microsoft Identity Manager 2016 SP1.

## <a name="a"></a>Um

**Validação de grupo de Active Directory** : um procedimento implementado no mim 2016 que garante a exclusividade do nome da conta de um grupo em um domínio armazenado em Active Directory.

**fluxo de trabalho de ação** : um fluxo de trabalho que executa uma ação. Isso inclui enviar um email de notificação e confirmar as alterações no banco de dados de serviço do MIM.

**atividade** : uma atividade de fluxo de trabalho é o bloco de construção básico de fluxos de trabalho do Windows Workflow Foundation (WF). Ele incorpora a lógica que é iniciada em tempo de design e tempo de execução ao criar e executar fluxos de trabalho.

**assembly de atividade** : A. DLL ou um. Arquivo EXE que contém um assembly .NET que implementa a lógica de uma atividade de fluxo de trabalho.

**Anchor** : um ou mais atributos exclusivos de um tipo de objeto que não é alterado e representa um objeto na fonte de dados conectada à qual o objeto de espaço do conector está vinculado (por exemplo, um número de funcionário ou uma ID de usuário).

**aprovação** : uma aprovação é um ponto de decisão do fluxo de trabalho que pode ser usado para obter autorização de uma pessoa antes de continuar no fluxo de trabalho.

**email de aprovação** : se uma solicitação exigir uma aprovação antes de ser confirmada, um email de aprovação será enviado aos aprovadores identificados.

**solicitação de aprovação** : uma solicitação que requer uma aprovação. Por exemplo, uma mensagem de email enviada pelo MIM 2016 para um Aprovador como parte do processamento de uma atividade de aprovação.

**resposta de aprovação** : uma resposta para uma solicitação de aprovação. Ele contém informações sobre se a solicitação foi aprovada ou não. Por exemplo, uma mensagem de email enviada do suplemento do MIM para Outlook em resposta a uma solicitação de aprovação.

**pasta de pesquisa de aprovações** : as pastas de pesquisa criadas pelo suplemento do mim para Outlook que fornece ao usuário uma maneira de ver as aprovações pendentes e concluídas e as atualizações de solicitação de aprovação.

**limite de aprovação** : o número de mensagens de resposta de aprovação positivas necessárias para permitir que uma solicitação continue o processamento.

**Aprovador** : a pessoa que dará a aprovação da solicitação para prosseguir para o próximo estágio. Eles receberão mensagens de solicitação de aprovação se o suplemento do MIM para Outlook for usado. Consulte também a entrada de "Aprovador de escalonamento".

**fluxo de atributos** : define a direção da qual os valores de atributo fluem entre o serviço do mim e outros sistemas externos.

**atividade de autenticação** : uma atividade de fluxo de trabalho que valida a identidade de um usuário. Por exemplo, portão de redefinição de senha e portão de autenticação de cartão inteligente. Consulte também a entrada para "portão de QA" e "portão de bloqueio".

**desafio de autenticação** : uma caixa de diálogo que exige que o usuário forneça uma resposta para autenticar no mim 2016. Por exemplo, perguntas para o usuário responder para redefinir suas senhas.

**atividade de desafio de autenticação** : uma atividade de Windows Workflow Foundation que é usada para configurar um desafio que é emitido para um usuário para se autenticar no mim 2016.

**fluxo de trabalho de autorização** : um fluxo de trabalho com atividades que devem ser concluídas antes que a solicitação seja confirmada no banco de dados. Os exemplos são validação de dados e aprovação.
<br/>

## <a name="c"></a>C

**limpar atributo de registro** : esse atributo limpa o registro associado a um fluxo de trabalho de autenticação. Por exemplo, em um desafio de perguntas e respostas, as respostas são armazenadas no MIM 2016 na forma de dados de registro. Quando a caixa limpar registro está marcada e um fluxo de trabalho é salvo, os dados de registro são excluídos, exigindo que os usuários se registrem novamente.

**membro computado (ou membro)** : um conjunto de recursos somente leitura computado da combinação de membros gerenciados manualmente e um filtro.

**conector** : um objeto no espaço do conector que representa um objeto em uma fonte de dados conectada e está atualmente vinculado a um objeto no metaverso por meio de regras predefinidas. O metadiretório usa objetos de conector para sincronizar valores de atributo entre uma fonte de dados conectada e o metaverso.

**filtro de conector** : uma regra que você usa para impedir que objetos de espaço do conector se vinculem a objetos do metaverso.

**espaço do conector** : uma área de preparação que contém representações de objetos e atributos selecionados em uma fonte de dados conectada. Um objeto de espaço do conector é um objeto no espaço do conector que é criado por uma importação de dados da fonte de dados conectada ou usando regras no MIM para criar novos objetos em diferentes fontes de dados conectadas. Esses objetos contêm valores de atributos que podem ser importados ou exportados de objetos correspondentes na fonte de dados conectada.

**XPath de contagem** : expressão XPath que retorna um valor numérico a ser renderizado entre parênteses após o nome de exibição do recurso.

**membro baseado em critérios** : um conjunto de recursos somente leitura computado da combinação de membros do grupo estático e de um filtro.

**associação baseada em critérios** : um grupo no qual a associação do grupo é determinada por um filtro. Consulte também a entrada de "associação estática".

**membro entre florestas** : membro de um grupo de segurança cuja conta de usuário em uma floresta diferente da conta de grupo.

**cálculo de grupo entre florestas** : uma atividade pronta para uso que coloca em membros da floresta de um grupo no conjunto de entidades de segurança estrangeira (FSP) associado à floresta na qual o grupo reside.


**expressão personalizada** : o idioma descritivo usado na definição de funções ou fluxos de atributos no modo avançado.
<br/>

## <a name="d"></a>D

**conjunto de destino (ou definição de recurso de destino após A solicitação)** : um conjunto no qual um recurso se move devido a uma solicitação que altera os atributos desse recurso.

**atividade de validação de grupo padrão** : uma atividade de fluxo de trabalho integrada que determina se uma solicitação de gerenciamento de grupo violaria a configuração ou a política do mim 2016 ou Active Directory.

**desconectado** : um objeto no espaço do conector que representa um objeto em uma fonte de dados conectada e não está atualmente vinculado a um objeto no metaverso.

**objetos de desconexão** : há três tipos de objetos de desconexão: desconectados, desconectados explícitos e Desconectadores filtrados.

**nome de exibição** : um atributo de um recurso que aparece em uma interface do usuário para identificar esse recurso. Um valor usado em um nome de exibição deve ser inequívoca e legível. É importante fornecer o nome de exibição se um quiser usar o recurso em vários controles do portal do MIM, como o seletor de recursos.

**grupo de distribuição** : uma coleção de recursos que são mais comumente os usuários e outros grupos que podem ser enviados por email ao mesmo tempo enviando email para a caixa de correio do grupo.

**configuração de domínio** : um recurso de configuração usado para modelar Active Directory domínios.

**grupo de domínio local** : um grupo com escopo de domínio local é um grupo de Active Directory que protege os recursos dentro de um domínio específico e que pode conter membros dessa floresta ou de qualquer floresta confiável.

**drop File** : um arquivo de log é um arquivo de registro XML que representa uma possível exportação ou importação.

**valor de atributo dinâmico** : o valor de um atributo que é calculado com base em outros atributos. Por exemplo, um atributo de nome é calculado concatenando o nome fornecido e o sobrenome.

**grupo dinâmico** : um grupo cuja associação é automaticamente determinada e mantida atualizada pelo mim 2016, garantindo que o grupo contenha todos os recursos (como pessoas, grupos, computadores) que se enquadram em condições expressas usando XPath.
<br/>

## <a name="e"></a>E

**Enumeração** : uma lista de recursos retornados pelo serviço mim 2016.

**escalonamento** : se uma aprovação não for concluída dentro do tempo especificado, a aprovação será escalonada e os aprovadores adicionais serão adicionados à aprovação.

**Aprovador de escalonamento** : o usuário que recebe as mensagens de solicitação de aprovação se os aprovadores não responderem. Consulte também a entrada de "aprovador".

**conector explícito** : um objeto no espaço do conector que está vinculado a um objeto no metaverso e não pode ser desconectado por um filtro de conector. Um conector explícito só pode ser criado manualmente com o associador e só pode ser desconectado pelo provisionamento ou usando o associador.

**Desligador explícito** : um objeto no espaço do conector que não está vinculado a um objeto no metaverso e só pode ser Unido usando o joinr. Um objeto se torna um Desconectador explícito desconectando manualmente o objeto usando o Joinr.

**Exportar** : exportar é o processo de enviar alterações por push às fontes de dados conectadas. Uma exportação é sempre uma operação Delta com base na última importação bem-sucedida. O MIM processa as alterações de exportação, mas não processa as alterações no espaço do conector até que as alterações tenham sido confirmadas por uma nova importação. Dependendo do tipo de agente de gerenciamento que você usa, as alterações implementadas por uma exportação podem estar no nível de atributo, objeto ou valor.

**Exportar fluxo de atributo** : o processo de tradução dos atributos de um objeto do metaverso para o espaço do conector. Esse processo pode envolver mapeamentos um-para-um, aplicando extensões de regras para modificar atributos ou definir valores de atributos estáticos. Os atributos exportados são preparados no espaço do conector para a próxima exportação para a fonte de dados conectada.

**Linguagem de marcação de asserção extensível (XAML)** : uma linguagem baseada em XML na qual as definições de fluxo de trabalho são representadas.

**filtro de escopo do sistema externo** : determina os recursos que você identifica e filtra de um diretório de origem com base em uma condição específica.

**tipo de recurso do sistema externo** : esse é o tipo de recurso no sistema externo ao qual os recursos do mim 2016 estão conectados.

**sinalizador de criação de recurso do sistema externo** : um parâmetro de uma regra de sincronização para indicar se um recurso deve ser criado no espaço do conector se, com base nos critérios de relacionamento, esse recurso não existir no sistema externo. Consulte sinalizador de criação de recursos do MIM 2016. <!-- Not sure what this is, should this be a term? -->

**escopo do sistema externo** : um parâmetro de uma regra de sincronização que contém um filtro que apresenta recursos no sistema externo ao qual a regra se aplica.
<br/>

## <a name="f"></a>F

**filtro** : uma expressão que contém condições de filtro. Um filtro corresponde a um recurso se cada uma das condições de filtro contidas no filtro corresponder ao recurso. No MIM 2016, o filtro usa a sintaxe XPath.

**Desconectador filtrado** : um objeto no espaço do conector que é impedido de ser Unido ou projetado para um objeto no metaverso com base nas regras de filtro do conector no agente de gerenciamento associado.

**Agente de gerenciamento do fim** : um agente de gerenciamento que sincroniza entre o serviço do mim 2016 e o serviço de sincronização do mim 2016.

**Serviço cliente de redefinição de senha do fim/mim** : refere-se ao serviço de proxy que reside no computador do usuário final que se comunica com o servidor mim 2016.

**Extensões de redefinição de senha do fim/mim** : isso se refere ao código que reside no computador do usuário final que estende a funcionalidade do logon do Windows para incluir a redefinição de senha de autoatendimento.

**Sinalizador de criação de recursos do fim/mim** : um parâmetro de uma regra de sincronização para indicar se um recurso deve ser criado no banco de dados do mim 2016 se for baseado nos critérios de relacionamento nos quais os recursos não existem. Consulte também a entrada para "sinalizador de criação de recursos do sistema externo".

**Function** : um componente que pode ser incluído em uma regra de sincronização ou uma definição de fluxo de trabalho para processar valores de dados.
<br/>

## <a name="g"></a>G

**portão** : uma atividade de fluxo de trabalho usada na fase de autenticação do processamento de solicitações. Consulte também a entrada para "portão de QA" e "portão de bloqueio".

**aninhamento de grupo** : um campo de uma definição de grupo que especifica se o grupo contém outros grupos como membros do grupo atual.

**escopo do grupo** : um campo de uma definição de grupo, um ou ' local ', ' global ' ou ' Universal '. Para obter mais informações, consulte Active Directory escopo do grupo.
<br/>

## <a name="i"></a>I

**importar** : o processo de mover objetos de fonte de dados conectados para o espaço do conector para fins de criação, modificação, exclusão ou verificação. Uma importação pode ser uma operação Delta ou completa. Para uma importação completa, o MIM solicita todos os objetos designados da fonte de dados conectada e exclui todos os objetos de preparo para os quais um objeto correspondente não foi recebido durante essa importação. Como resultado, essa etapa de perfil de execução é útil para limpar os objetos de preparo no espaço do conector. Os objetos que foram recebidos da fonte de dados conectada são preparados no espaço do conector. Para a importação Delta fornecer os resultados desejados, a fonte de dados conectada deve implementar uma forma de marca-d ' água. A fonte de dados conectada usa a marca-d ' água para indicar quando ocorreu as alterações mais recentes em um objeto. O MIM lê a marca-d ' água para determinar o que incluir na importação Delta. Um exemplo é o Active Directory USN.

**Importar o fluxo de atributos (IAF)** : o fluxo de atributos de importação é o processo de importação de um atributo do espaço do conector para o metaverso. Esse processo pode envolver a aplicação de mapeamentos de atributo um-para-um, usando extensões de regras para modificar atributos ou definir atributos estáticos.

**URL da imagem** : URL para um arquivo de imagem a ser renderizado na interface do usuário do portal do mim 2016.

**fluxo inicial** : um fluxo inicial é um fluxo de valor de atributo que só é aplicado uma vez quando o recurso é criado pela primeira vez. Ou seja, uma senha inicial é criada somente quando você cria uma conta pela primeira vez.

**fluxo de trabalho interativo** : um fluxo de trabalho que requer resposta do usuário solicitando uma alteração, como para executar verificações de autenticação adicionais.
<br/>

## <a name="j"></a>J

**Join** : uma junção é o processo de vinculação de um objeto de espaço do conector com um objeto de metaverso existente. Os valores de atributo fluem somente entre objetos vinculados.

**solicitação de grupo de junção** : uma solicitação para adicionar um usuário a um grupo.
<br/>

## <a name="l"></a>L

**bloqueio** : uma definição de configuração em um recurso Person no banco de dados mim 2016 que restringe essa pessoa de autenticar para o mim 2016 ou executar a redefinição de senha.

**portão de bloqueio** : uma atividade de fluxo de trabalho na fase de autenticação do processamento de solicitações para bloquear um usuário que falhou ao se autenticar. Consulte também a entrada para "bloquear" e para "portão de QA".

**limite de bloqueio** : Este é um controle inteiro que especifica o número de vezes que um usuário pode falhar ao concluir o fluxo de trabalho de autenticação antes que eles sejam bloqueados durante a duração do bloqueio.A configuração padrão para isso é 3. O limite inferior é 0 e o limite superior é de 99.

**duração do bloqueio** : Este é um controle inteiro que especifica a duração em minutos em que o usuário é bloqueado para depois de atingir o limite de bloqueio.A configuração padrão é 15 minutos.O limite inferior para essa configuração é 1 e o limite superior é 9999.O limite superior permite que o administrador defina o limite superior para mais de um dia.

**contagem de limites de bloqueio antes do bloqueio permanente** : esse é um controle de inteiro que permite ao administrador configurar um valor numérico para o número de vezes que um usuário pode atingir o limite de bloqueio antes de ser bloqueado permanentemente.  O bloqueio permanente implica que o usuário deve ser desbloqueado pelo administrador do sistema. Por padrão, isso é definido como 3.O intervalo para essa configuração está entre 1 e 99.
<br/>

## <a name="m"></a>M

**agente de gerenciamento** : um agente de gerenciamento (MA) conecta uma fonte de dados conectada específica ao metadiretório. Ele é responsável por mover dados da fonte de dados conectada para o MIM e as regras que determinariam a qualificação dos dados de identidade para participar no MIM e em qualquer outra fonte de dados conectada. Quando os dados no metadiretório são modificados, o agente de gerenciamento pode exportar os dados para as fontes de dados conectadas para manter a fonte de dados conectada sincronizada com os dados no MIM. Um agente de gerenciamento é usado em vez de ter um conector baseado em agente.

**Regra de política de gerenciamento (MPR)** : regras de política de gerenciamento (MPRS) fornecem um mecanismo para modelar regras de processamento de negócios para solicitações de entrada para o servidor mim 2016. Eles controlam as permissões para solicitar operações nos recursos do MIM 2016 junto com os fluxos de trabalho que são disparados por essas solicitações.
   
   Há dois tipos de MPRs:
   
- Solicitar MPRs: conceder permissões e executar fluxos de trabalho (chamados antes da execução de uma operação solicitada).
- Definir MPRs de transição: executar fluxos de trabalho somente (reação a uma alteração de estado aplicada).
   
  Os principais objetos de design para MPRs são:
   
- Permissões de modelagem
- Mapeamentos de fluxos de trabalho de modelagem
- Transições de modelagem
- Definições reflexivas de modelagem
- Modelando o acesso de usuário não autenticado
- Políticas temporais de modelagem
- Permissões de filtro de modelagem

**membro gerenciado manualmente** : a associação do grupo ou conjunto que consiste em uma lista de usuários, grupos ou outros recursos selecionados manualmente.

**metaverso** : o armazenamento de dados central usado pelo mim para conter as informações de identidade agregadas de várias fontes de dados conectadas, fornecendo uma única exibição global e integrada de todos os objetos combinados. O metaverso não deve ser usado como uma tabela ou exibição de dados de identidade de agregação para qualquer aplicativo que não seja o MIM, pois pode resultar em corrupção.

**caixa de correio monitorada** : uma caixa de correio que o serviço mim 2016 monitora para receber aprovação e solicitar emails do suplemento do mim para Outlook.
<br/>

## <a name="n"></a>N

**atividade de notificação** : uma atividade de fluxo de trabalho na fase de ação do processamento de solicitação no qual o mim 2016 envia emails a um ou mais usuários para notificá-los da solicitação.

**mensagem de notificação** : uma mensagem de email enviada por uma atividade de notificação. Consulte também a entrada de "atividade de notificação".
<br/>

## <a name="o"></a>O

**ObjectId (ResourceId)** : um atributo que contém um GUID (identificador global exclusivo) atribuído pelo mim 2016 a cada recurso quando ele é criado. Isso também é conhecido como ID do recurso.

**identificador de objeto** : uma sequência de números usada como um identificador para um campo em um certificado digital X. 509 ou para um tipo de atributo ou classe de objeto em um serviço de diretório baseado em LDAP. Os identificadores de objeto são normalmente atribuídos por fornecedores de software e órgãos de padrões.

**tipo de operação** : o tipo de operação é solicitado no recurso gerenciado pelo mim 2016 por meio do serviço Web. Isso inclui criar e excluir recursos e ler e modificar atributos de recurso. Além disso, as operações adicionar/remover permitem que você aplique controle adicional à operação modificar para controlar apenas a adição de valores a atributos ou à sua remoção.

**Operator** : um elemento de um filtro que especifica uma comparação ou outra relação entre valores de dados.

**conjunto de origem (ou definição de recurso de destino antes da solicitação)** : um conjunto no qual um recurso pertencia antes de uma alteração nos atributos desse recurso.
<br/>

## <a name="p"></a>P

**parâmetro** : ao provisionar novos recursos, às vezes é possível fornecer valores de atributo de uma fonte externa, como um usuário. Um valor de atributo é passado como um parâmetro para poder criar um novo recurso com êxito.

**partição** : um volume lógico de dados no espaço do conector. Um agente de gerenciamento pode criar uma ou mais partições para dividir logicamente os dados em agrupamentos lógicos separados. Cada volume de dados é processado individualmente durante a sincronização.

**redefinição de senha** : um procedimento pelo qual a senha de um usuário pode ser alterada para um valor conhecido, em situações em que o usuário esqueceu ou perdeu sua senha. Consulte também a entrada para "registro".

**fase** : cada solicitação de criação, atualização ou exclusão de recursos é processada por meio de três fases de fluxo de trabalho. Na fase de autenticação, as verificações de autenticação adicionais do usuário solicitante podem ser executadas. Na fase de autorização, todas as aprovações necessárias são coletadas. Na fase de ação, as atividades são executadas depois que a solicitação para alterar o recurso tiver sido confirmada.

**objetos de espaço reservado** : objetos no espaço do conector que representam um único nível da hierarquia da fonte de dados conectada. Por exemplo, se você quiser sincronizar objetos com uma floresta Active Directory, será necessário importar os contêineres que compõem o caminho para os objetos Active Directory. No exemplo, CN = MikeDan, OU = Users, DC = Microsoft, DC = com, os objetos de espaço reservado seriam criados para DC = com e DC = Microsoft, DC = com. Além disso, um objeto de espaço reservado pode representar um objeto na fonte de dados conectada à qual um valor de atributo de referência importado se refere (por exemplo, o objeto ao qual o atributo Manager se refere a um objeto de usuário). Os objetos de espaço reservado não contêm valores de atributo e não podem ser vinculados ao metaverso.

**Gerenciamento de políticas** : o gerenciamento de políticas no mim 2016 é possibilitado por um console baseado no SharePoint para a criação e a imposição de políticas. Os fluxos de trabalho baseados em Windows Workflow Foundation extensível permitem que os usuários definam, automatizem e imponham políticas de gerenciamento de identidade. O gerenciamento de políticas também inclui a consistência e a sincronização de identidades heterogêneas que são obtidas pela integração de uma ampla gama de sistemas operacionais de rede, email, banco de dados, diretório, aplicativo e acesso a arquivos simples.

**atualização de política (ou executar na atualização de política)** : se um fluxo de trabalho deve ser executado novamente como um efeito de uma alteração nos conjuntos ou MPRS se referir a ele, o sinalizador de atualização de política é usado para indicar isso.

**precedência** : uma ordem de regras de sincronização.

**conjunto principal** : um conjunto usado na regra de política de gerenciamento para especificar o conjunto de recursos (geralmente usuários) que iniciam a avaliação da regra de política de gerenciamento.

**principal definido em relação ao recurso** : é uma propriedade reflexiva. Seu valor é definido em termos de uma das propriedades do recurso. Ele é usado para definir regras de política de gerenciamento dinâmico cujas condições são avaliadas no contexto de cada recurso de destino que está sendo processado.

**projeção** : o processo de criar um objeto no metaverso com base em regras de projeção e, em seguida, vincular automaticamente esse objeto a um objeto existente no espaço do conector.

**provisionar** : o processo de criação, renomeação e desprovisionamento de objetos em espaços de conectores predeterminados com base em alterações em um objeto no metaverso. As regras de provisionamento podem ser configuradas para serem chamadas sempre que um objeto de metaverso for modificado. Essas regras podem executar ações em nível de objeto, como a criação de um novo objeto de espaço conector ou a desconexão de objetos de espaço conector existentes vinculados ao objeto de metaverso.
<br/>

## <a name="q"></a>Q

**Portão de QA** : uma atividade de fluxo de trabalho em uma fase de autenticação, na qual o usuário solicitante deve fornecer respostas para uma ou mais perguntas predeterminadas. Essa atividade normalmente é usada na redefinição de senha, para comprovar o usuário para provar sua identidade, fornecendo ao usuário uma seleção de perguntas predeterminadas para as quais apenas esse usuário saberia, para a qual o usuário deve fornecer a resposta correta. Consulte também a entrada de "portão de bloqueio".

**Desafio de QA** : um desafio que exige que o usuário responda a uma série de perguntas para se autenticar no mim 2016.
<br/>

## <a name="r"></a>R

**configurações de senha aleatória** : uma configuração que determina o número de caracteres necessários para definir uma senha no diretório externo.

**tipo de atributo de referência** : um tipo de atributo no qual os valores do atributo são os valores de atributo ObjectId (identificadores exclusivos globais) de outros recursos no mim 2016.

**integridade referencial** : uma restrição no mim 2016, na qual um atributo de referência não pode ter como valor um ObjectID de um recurso que foi excluído.

**registro** : um procedimento para configurar a redefinição de senha de autoatendimento para um usuário. Consulte também a entrada de "portão de QA".

**novo registro** : a atualização do registro de um desafio de autenticação no mim 2016, normalmente é necessária após uma alteração na política administrativa para o registro de redefinição de senha.

**criação de relação** : sinalizadores de configuração de uma regra de sincronização que determina se os recursos devem ser criados automaticamente no mim 2016 ou no sistema externo, se os recursos estiverem ausentes.

**critérios de relação** : configuração de uma regra de sincronização usada para corresponder recursos no servidor e recursos do mim em sistemas externos.

**terminação de relação** : indica se os recursos relacionados em outros sistemas externos devem ser desconectados (e possivelmente excluídos) quando a regra de sincronização não se aplica mais.

**Gerenciamento de solicitações** : a capacidade de um usuário interagir com e gerenciar solicitações enviadas e fluxos de trabalho associados.

**RMPR (solicitação de política de gerenciamento de solicitações)** : um tipo de regra de política de gerenciamento que é avaliado e aplicado a solicitações de entrada para executar operações. RMPRS são usados principalmente para criar definições de política de acesso no MIM. Em outras palavras, a resposta a como a solicitação é tratada. Quando você configura um RMPR, o solicitante está no conjunto projetado para executar uma operação.

   A arquitetura do MIM define seis operações diferentes para as quais um RMPR pode ser definido:
   
- Criar um recurso.
- Excluir um recurso.
- Ler um recurso.
- Adicione um valor a um atributo de valores.
- Remova um valor de um atributo com valores de multivalor.
- Modifique um atributo de valor único.
   
  Ao definir um RMPR, você deve selecionar pelo menos uma dessas seis operações. As operações são sempre definidas no contexto do solicitante. Cada condição requer a definição de um destino. Uma operação que é aplicada a um destino pode resultar em uma transição de estado do recurso de destino. Um RMPR é sempre chamado antes da execução de uma operação solicitada. Para caracterizar efetivamente o destino de sua condição, você precisa configurar dois Estados diferentes:
   
- Definição de recurso de destino antes da solicitação: o estado do destino antes da solicitação ser aplicada.
- Definição de recurso de destino após a solicitação: o estado do destino após a solicitação ser aplicada.
   
  Se você precisa definir ambos os Estados dependem das operações que seu RMPR está definido para fazer. Em uma operação de criação, o recurso solicitado não tem estado inicial. Portanto, você só precisa configurar a definição de recurso de destino após a solicitação "para uma operação de criação.
   
  Uma operação de leitura ou exclusão não causa uma transição de estado. Para esses dois tipos de operações, você só precisa especificar a definição de recurso de destino antes da solicitação.
   
  Para uma operação de modificação ou todas as outras combinações de operações, você precisa configurar ambos os Estados, que podem ter os mesmos valores se nenhuma transição de estado ocorrer.
   
  Você pode expressar os recursos relacionados relativos ao solicitante (como o objeto de usuário próprio do solicitante, o gerente do usuário de destino ou o proprietário de um grupo de destino).
   
  A forma mais simples de uma resposta a uma condição é conceder permissões para executar a operação solicitada. Além de conceder permissões, você também pode definir outras operações como uma resposta a uma condição em um RMPR. Na arquitetura do MIM, essas operações são definidas na forma de fluxos de trabalho. No momento, quando um determinado RMPR é processado, o sistema pode não ter informações suficientes para conceder permissão. Nesse caso, você pode definir as etapas de autenticação e autorização adicionais em seu RMPR que se aplicam à pessoa que está executando uma determinada solicitação. Por exemplo, para conceder permissão para executar a operação solicitada, você pode exigir a interação manual de um usuário para aprovar a operação.
   
  A figura a seguir descreve a arquitetura completa de um RMPR:
   
  ![Arquitetura do RMPR](media/mim-2016-sp1-terms/8f5054080d3f8bd9c73d93a5e3ed51f9.gif)
   
  Quando um novo objeto de solicitação é criado no MIM, o sistema consulta o RMPRs configurado para objetos correspondentes, comparando as condições de solicitação com as condições configuradas em seu RMPRs de gerenciamento. Se os RMPRs correspondentes estiverem localizados, eles serão aplicados ao objeto de solicitação enfileirado. A figura a seguir descreve esse processo:
   
  ![Os RMPRs correspondentes estão localizados e aplicados ao objeto de solicitação enfileirado](media/mim-2016-sp1-terms/65bdb65aaffa55a90707ff6f7b13632d.gif)
   
  No portal do MIM, as permissões para operações devem ser concedidas explicitamente. Em outras palavras, a menos que seja concedido por um RMPR, todas as operações em recursos são negadas. Cada objeto de solicitação requer pelo menos um RMPR que concede permissão para executar a operação solicitada em um destino.

**objeto de solicitação** : quando um usuário executa uma tarefa no portal do mim ou no suplemento do mim para Outlook, ele é representado como um objeto de solicitação. Os objetos de solicitação representam um mecanismo de relatório conveniente para atividades em seu sistema.

   Cada objeto de solicitação tem estes componentes:
   
- Solicitante: um recurso que solicita a execução de uma operação.
- Operação: a ação que o solicitante deseja executar.
- Destino: o recurso que é o destino da operação solicitada.
   
  Logicamente, um objeto de solicitação é uma implementação da seguinte instrução:
   
  `The requester attempts to perform the following operation on this target...`
      
  A figura a seguir descreve a arquitetura geral de um objeto de solicitação:
   
  ![Solicitar arquitetura do objeto](media/mim-2016-sp1-terms/788e9b3a4fcddb78ff4a05dcb5e61192.gif)
   
  Cada objeto de solicitação tem uma propriedade status para indicar o estado de processamento. Solicitações de processamento podem exigir interação manual para concluir uma solicitação. Por exemplo, o proprietário de um grupo pode ser necessário para aprovar manualmente a solicitação de outro usuário para ingressar em um grupo. Além de uma interação manual, você também pode configurar o MIM para processar automaticamente uma solicitação específica sem a necessidade de interação humana.
   
**modelo de processamento de solicitação** : o modelo de processamento de solicitação no mim é composto por três fases principais:

- Fase 1: autenticação
- Fase 2: autorização
- Fase 3: ação
   
  Os fluxos de trabalho, cada um contendo uma ou mais atividades, podem ser anexados a cada uma dessas fases e executados no contexto da execução de uma única solicitação. Uma solicitação pode iniciar de uma única chamada de usuário para um dos pontos de extremidade de serviço Web ou por meio de um usuário que está criando uma solicitação no portal do MIM.
   
  A figura a seguir mostra a relação dos componentes de processamento de solicitação:
   
  ![Relação dos componentes de processamento de solicitações](media/mim-2016-sp1-terms/0e57445d8b9a3216c31add80eb26493f.gif)
   
  As solicitações são processadas na seguinte ordem:
   
- Criação de objeto de solicitação: o MIM 2016 cria um objeto de solicitação em resposta a uma chamada para um dos pontos de extremidade de serviço Web ou devido a uma solicitação iniciada por meio do portal do MIM.
   
- Avaliação de MPR: os direitos do solicitante para solicitar a ação são validados e a computação dos fluxos de trabalho aplicáveis é executada. A solicitação é verificada em relação aos mapeamentos para quaisquer objetos de MPR. Para mapear para um MPR, todos os campos aplicáveis do MPR para a operação solicitada precisam corresponder. Isso inclui o solicitante, a operação, o recurso de destino e os atributos. Se todas essas condições, incluindo os atributos que estão sendo afetados, forem verdadeiras para uma solicitação de entrada, o MPR apropriado será correspondido à solicitação. Uma solicitação deve mapear para pelo menos um MPR que concede a permissão como parte de sua definição. Se isso for verdadeiro, a solicitação passará pelo estágio de verificação de permissões do processamento de solicitações. Se isso não for verdadeiro, a solicitação falhará. O sistema também determina as transições de conjunto que fazem parte da solicitação e localiza todos os MPRs de conjunto relacionados com base em transição.
   
- Autenticação: o MIM 2016 executa fluxos de trabalho de autenticação um de cada vez em uma ordem não determinística para confirmar a identidade do solicitante.
   
- Autorização: o MIM 2016 confirma a permissão do solicitante para executar a operação solicitada no recurso especificado na solicitação. Todos os fluxos de trabalho de autorização dependentes são executados em paralelo, mas uma solicitação não é confirmada no repositório de objetos do MIM, a menos que todos os fluxos de trabalho tenham sido concluídos e todos tenham êxito.
   
- Processamento: o MIM 2016 executa a operação solicitada no repositório de aplicativos do MIM.
   
- Ação: o MIM 2016 executa todos os processos que devem ocorrer devido à operação solicitada. Todos os fluxos de trabalho de ação são executados em paralelo. As operações de leitura não têm nenhum fluxo de trabalho aplicado ao seu processamento. Isso inclui os fluxos de trabalho configurados no RMPR, bem como os fluxos de trabalho no conjunto MPRs baseado em transição.
   
  >[!NOTE]
  >As solicitações iniciadas pela conta de sincronização ignoram todos os fluxos de trabalho de autenticação e autorização que seriam aplicáveis a eles. Todos os fluxos de trabalho de ação aplicáveis são aplicados.

**solicitante** : a identidade do usuário ou serviço que enviou uma solicitação ao mim 2016.

**escopo do solicitante** : uma coleção configurada de usuários que podem enviar uma solicitação. Pode ser "Everyone" ou um conjunto específico de usuários definido por um filtro.

**recurso** : uma instância de um determinado tipo de recurso no mim 2016. Cada recurso é identificado exclusivamente por seu atributo ObjectID (ResourceId).

**configuração de exibição de controle de recurso (RCDC)** : RCDCs são recursos de configuração que são usados para renderizar a interface do usuário no controle de recursos (RC) para criar um tipo de recurso específico no mim 2016.

**conjunto atual de recursos** : parte da definição de condição MPR (regras de política de gerenciamento). A coleção de recursos de destino no momento em que a solicitação é recebida. Aplica-se aos tipos de operação ler, excluir e modificar.

**conjunto final de recursos** : parte da definição de condição nas regras de política de gerenciamento. A coleção de recursos de destino após o processamento da solicitação. Aplica-se somente a tipos de operações Create e Modify.

**hierarquia de recursos** : em um serviço de diretório, a hierarquia de uma entrada de recurso é a coleção de entradas de diretório entre a base de um contexto de nomenclatura e essa entrada de recurso.

**escopo de recurso** : um conjunto de recursos sobre o qual uma solicitação pode ser enviada.

**tipo de recurso** : uma parte de um esquema que define a representação de um recurso no mim 2016.

**mapeamento de tipo de recurso** : uma relação entre um tipo de recurso usado para representar um recurso no mim 2016 e uma classe de recurso usada para representar esse recurso no metaverso.

**função** : uma entidade de segurança atribuída à organização usada para gerenciar os direitos de acesso.

**extensão de regras** : uma extensão de regras é uma biblioteca de vínculo dinâmico (. dll) que contém um conjunto definido de regras para gerenciar dados. Você pode usar extensões de regras durante sincronizações para estender a funcionalidade. Por exemplo, você pode usar uma extensão de regras para combinar dados de dois atributos de origem e Flow-los para um atributo de destino (por exemplo, `sn` e `givenName` para `displayName` ).

**histórico de execuções** : um conjunto de estatísticas que mostra os resultados de uma única execução de um agente de gerenciamento.

**perfil de execução** : um perfil de execução representa um conjunto de etapas que especificam como executar um agente de gerenciamento e definições de configuração que determinam como um agente de gerenciamento é executado. Um agente de gerenciamento pode ter vários perfis de execução, que são armazenados com o agente de gerenciamento. Um perfil de execução é composto por pelo menos uma etapa de perfil de execução.
<br/>

## <a name="s"></a>S

**pasta de pesquisa** : consulte a entrada para "pasta de pesquisa de aprovações".

**escopo da pesquisa** : especifica as propriedades de um contexto de pesquisa específico que um usuário pode realizar no portal do mim 2016. Por exemplo, um usuário pode selecionar um escopo de pesquisa de uma lista suspensa para "todos os usuários", "todas as listas de distribuição", "Minhas aprovações pendentes" e os resultados da pesquisa seriam restritos a itens que atendem a esses critérios, além de quaisquer termos de pesquisa especificados pelo usuário.

**descritor de segurança** : uma estrutura e dados associados que contêm as informações de segurança para um recurso protegível. Um descritor de segurança identifica o proprietário do recurso e o grupo primário. Ele também pode conter uma DACL que controla o acesso ao recurso e uma SACL que controla o log de tentativas de acessar o recurso.

**entidade de segurança** : uma identidade usada para gerenciamento de segurança, como uma conta de usuário, que pode ser autenticada em um serviço.

**token de segurança** : um elemento de protocolo que transfere informações de autenticação e autorização com base em uma credencial. Nos protocolos de serviços Web, um token de segurança é representado como um elemento XML em um cabeçalho SOAP, conforme definido pelo WS-Security.

**serviço de token de segurança** : um serviço que implementa o protocolo de WS-Trust para gerenciar a confiança entre clientes e serviços com base na troca de tokens de segurança.

**fluxo de trabalho Sequencial** : todos os fluxos de trabalho no mim 2016 são derivados do fluxo de trabalho Sequencial do Windows Workflow Foundation. Ele inclui vários fluxos de trabalho em uma ordem sequencial.

**conta de serviço** : uma conta do Windows atribuída a ser usada por um serviço do Windows, em vez de ser usada por um usuário para fazer logon em um sistema de computador. Representa a conta do sistema do MIM.

**Set** : uma coleção nomeada de recursos. Normalmente, os conjuntos são usados para organizar recursos com base em regras. A associação em um conjunto é manual – gerenciada ou baseada em critérios. Isso significa que você pode adicionar recursos manualmente a um conjunto e pode definir critérios que adicionam automaticamente recursos a um conjunto com base em uma instrução de filtro. Quando um recurso preenche os critérios de filtro, ele é adicionado automaticamente ao conjunto relacionado.

**Definir regra de política de gerenciamento de transição (TMPR)** : uma regra de política de gerenciamento que é aplicada em alterações à associação de um conjunto. Defina fluxos de trabalho de ação aplicar TMPRs quando a transição de objeto para dentro ou para fora de um conjunto especificado no MPR.

   Há dois tipos de TMPRs:
   
- Transição em: um recurso se torna um membro do conjunto de transição.
- Transição para fora: um recurso deixa o conjunto de transição.
   
   >[!NOTE]
   >Quando um conjunto de transição é excluído, o sistema trata a exclusão como um evento de transição para os objetos afetados.
      
  A resposta é uma reação a uma alteração de estado aplicada. Quando o MPR relacionado é invocado, a condição já foi aplicada. Isso significa que os recursos afetados já passaram para dentro ou para fora de um conjunto de transição. Para TMPRs, o objetivo da resposta é não definir a reação a uma operação solicitada, mas para definir a resposta a uma operação aplicada. Em outras palavras, para um conjunto de MPR baseado em transição, é irrelevante como um estado foi atingido. O que é relevante são as consequências de uma alteração de estado.
   
  Quando você configura um conjunto de MPR baseado em transição no MIM, precisa especificar as três configurações a seguir:
   
- Conjunto de transição
- Tipo de transição
- Fluxos de trabalho de política
   
  Os fluxos de trabalho de política são definições dos processos que precisam ser invocados em resposta à alteração de estado. Os casos de uso mais comuns para MPRs com base em estado são a concessão ou a revogação de direitos e provisionamento e desprovisionamento em fontes de dados externas.
   
  A figura a seguir descreve a arquitetura completa de um conjunto de MPR baseado em transição:

  ![Definir arquitetura TMPR](media/mim-2016-sp1-terms/e12154d35b48cec676f160930ff33662.gif)
  
  Definir MPRs com base em transição são ativados por solicitações. Quando uma solicitação é processada e aprovada por um RMPR, o serviço do MIM também determina se uma solicitação aprovada resulta em uma transição de estado e se um MPR baseado em transição de estado que manipula a alteração de estado existe.
  
  A figura a seguir descreve a relação entre uma solicitação e um MPR baseado em transição de conjunto:
  
  ![Relação entre uma solicitação e um conjunto de TMPR](media/mim-2016-sp1-terms/426be3a3a5380720fa1eaa3e7df360df.gif)

**Sid** : um valor exclusivo usado para identificar uma conta de usuário, uma conta de grupo ou uma sessão de logon.

**SOAP** : um protocolo para a troca de informações estruturadas entre componentes de software.

**sincronização** : o processo de manter os dados selecionados em várias fontes de dados no acordo. A sincronização representa operações em objetos somente no MIM. A sincronização pode ser uma operação em todo o conjunto de dados para o qual é definida em um agente de gerenciamento ou uma operação Delta de acordo com as alterações desde a última operação conhecida. A etapa perfil de execução de sincronização define os processos de sincronização de entrada e de saída.

   A etapa do perfil de execução de sincronização tem dois subtipos:
   
- Sincronização delta
- Sincronização total
   
  Durante a sincronização Delta, o MIM processa somente objetos importados, que são os objetos de preparo sinalizados como importação pendente. Essa etapa de perfil de execução é útil para processar apenas os objetos que têm alterações pendentes, mas que não foram processadas durante uma execução de sincronização anterior.
   
  A sincronização Delta é usada em dois perfis de execução predefinidos e comporta-se de forma ligeiramente diferente em cada um. O perfil da primeira execução é a sincronização Delta, em que nenhuma importação de qualquer fonte conectada é executada, mas todos os objetos no espaço do conector são avaliados e todos os objetos com alterações pendentes são processados. O segundo perfil de execução é a importação Delta e a sincronização Delta combinada. Esse perfil de execução importa somente os objetos e atributos da fonte de dados conectada cujos valores foram alterados desde a última vez em que o agente de gerenciamento foi executado. As regras do agente de gerenciamento são reaplicadas somente a objetos que têm alterações pendentes da importação Delta. Objetos sem alterações pendentes dessa importação Delta não são avaliados.
   
  Durante a sincronização completa, o MIM avalia e aplica as regras de sincronização a todos os objetos de preparo em um espaço conector. A sincronização completa deve ser iniciada sempre que as alterações forem aplicadas às regras de um determinado ambiente. Dependendo do número de objetos no espaço do conector, isso pode ser uma operação de tempo e uso intensivo de recursos, portanto, alterações frequentes nas regras de sincronização em seu ambiente de produção devem ser evitadas.

**filtro de sincronização** : um filtro para impedir que recursos no metaverso sejam transferidos para o banco de dados do mim 2016.

**regra de sincronização** : uma regra para o fluxo de informações de recursos entre o servidor mim (incluindo o mecanismo de sincronização do mim) e o sistema externo conectado.
<br/>

## <a name="t"></a>T

**política temporal** : um conjunto de MPR de transição associado a um conjunto temporal. A política é aplicada na passagem de tempo, à medida que os objetos são transferidos para dentro e fora do conjunto com base na definição do conjunto temporal.

**Conjunto temporal** : um tipo de um objeto definido com base em datas relativas. Os conjuntos temporais fornecem um mecanismo que pode automatizar totalmente o processo de transição para dentro ou para fora de um conjunto com base na passagem do tempo. Por exemplo, um conjunto temporal pode ser definido para todos os grupos que expiram uma semana a partir de hoje. O sistema avalia os objetos no sistema automaticamente e os adiciona a esse conjunto diariamente. Outros exemplos permitem definições dinâmicas de uma referência de tempo, como um filtro com base em "x dias a partir de hoje".

**evento cronometrado** : um evento de transição que ocorre depois que um intervalo de tempo configurado tiver decorrido ou uma data e hora específicas forem atingidas.

**tempo limite** : um período de tempo no qual o mim 2016 aguarda respostas de aprovação até que uma atividade seja escalonada.

**conjunto de transição** : um conjunto que é usado em uma definição de uma regra de política de gerenciamento de transição de conjunto. A política é aplicada às alterações na associação set, que pode ser qualquer um dos objetos que entram ou saem do conjunto, dependendo da configuração do TMPR.
<br/>

## <a name="u"></a>U

**grupo desbloqueado** : um grupo no qual a associação do grupo pode ser alterada por usuários que não sejam o proprietário do grupo.

**grupo universal** : um grupo com escopo universal é um grupo de Active Directory que pode conter membros de uma floresta específica. Um grupo universal pode receber permissões em qualquer domínio ou floresta. As listas de distribuição normalmente têm escopo universal.Um grupo de segurança com escopo universal pode proteger recursos dentro da mesma floresta.

**solicitação de atualização** : uma solicitação para alterar os atributos de um recurso.

**palavra-chave Usage** : uma palavra-chave Usage é usada para determinar quais escopos de pesquisa são mostrados para uma página específica na interface do usuário do Portal. Cada página de exibição de lista na interface do usuário Especifica zero ou mais palavras-chave de uso, e a interface do usuário dessa página inclui todos os escopos de pesquisa que contêm palavras-chave correspondentes. Ao criar escopos de pesquisa, os clientes podem especificar zero ou mais palavras-chave por escopo de pesquisa para personalizar quais escopos de pesquisa aparecem para uma determinada página na interface do usuário. Ele também é usado para determinar qual recurso de home page e recurso de barra de navegação é mostrado para o conjunto de usuários. Ele também é usado no gerenciamento de esquema para proteger e rotular elementos de esquema que são necessários para vários componentes do MIM.
<br/>

## <a name="w"></a>W

**portal da Web** : uma interface do usuário implementada por um aplicativo de software por meio de um componente de um servidor Web, como o IIS.

**serviço Web** : uma interface de protocolo para um serviço implementado usando um protocolo baseado em http.

**fluxo de trabalho** : um fluxo de trabalho é um conjunto de unidades elementas chamadas atividades que são armazenadas como um modelo que descreve um processo do mundo real. Os fluxos de trabalho fornecem uma maneira de descrever a ordem e as relações dependentes entre os itens. Esse trabalho passa pelo modelo do início ao fim e as atividades podem ser executadas por pessoas ou por funções do sistema. Em outras palavras, se uma resposta a uma solicitação exigir processamento complexo, as etapas serão encapsuladas em um objeto de fluxo de trabalho. Os fluxos de trabalho são componentes opcionais e fortemente ligados ao MPRs. Os fluxos de trabalho definem uma atividade ou atividades que devem ocorrer durante o processamento de um MPR. O MIM instala vários fluxos de trabalho padrão que podem ser usados como são ou como base para um fluxo de trabalho personalizado.

   Exemplos de atividades de fluxo de trabalho são:

- Enviar uma mensagem de email automatizada para solicitar aprovação.
- Restringir os atributos que um usuário pode ver durante uma pesquisa personalizada.
- Validando um novo grupo em relação às diretrizes AD DS ou MIM.
- Adicionando ou removendo o objeto do escopo de uma regra de sincronização.
   
  Para atender a todos os requisitos de processamento em seu ambiente, a arquitetura do MIM define três tipos de fluxos de trabalho:
   
- Autenticação: executa a validação de identidade de usuário adicional antes de continuar com a solicitação
- Autorização: valida uma solicitação por meio de uma sequência de atividades, como a obtenção de aprovação fora do necessário antes do processamento de uma solicitação.
- Ação: processa quaisquer atividades adicionais depois que a solicitação original tiver sido concluída com êxito.
   
  Esses três fluxos de trabalho fazem parte do modelo de processamento de solicitação.

**definição de fluxo de trabalho** : a definição de fluxo de trabalho é armazenada no formato xoml definido pelo Windows Workflow Foundation (WF). Isso define as atividades, os parâmetros para as atividades e a ordem em que elas devem ser executadas.

**Designer de fluxo de trabalho** : a experiência de tempo de design para a construção de fluxos de trabalho.

**host de fluxo de trabalho** : o componente de servidor que lida com a execução de fluxos de trabalho. No MIM 2016, o serviço MIM 2016 é o host do fluxo de trabalho.

**instância de fluxo de trabalho** : uma instância em execução de uma definição de fluxo de trabalho como um efeito de uma solicitação.

**Gerenciamento de fluxo de trabalho** : um recurso mim 2016 que lida com a criação de fluxos de trabalho, execução e gerenciamento deles. O gerenciamento de fluxo de trabalho consiste no Designer de Fluxo de Trabalho, no gerenciamento de solicitações e no host do fluxo de trabalho.
