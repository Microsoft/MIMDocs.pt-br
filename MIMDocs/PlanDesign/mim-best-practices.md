---
title: "Práticas recomendadas do Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# Práticas recomendadas do Microsoft Identity Manager 2016 | Microsoft Docs
<a id="microsoft-identity-manager-2016-best-practices" class="xliff"></a>

Este tópico descreve as práticas recomendadas para implantar e operar o Microsoft Identity Manager 2016 (MIM)

## Configuração do SQL
<a id="sql-setup" class="xliff"></a>
>[!NOTE]
Estas recomendações para configurar um servidor que executa o SQL partem do princípio de que há uma instância do SQL dedicada ao FIMService e outra dedicada ao banco de dados FIMSynchronizationService. Caso o FIMService seja executado em um ambiente consolidado, será necessário fazer os ajustes apropriados à configuração.

A configuração do servidor de linguagem SQL é essencial para que o sistema tenha um desempenho otimizado. Em implementações em grande escala, o desempenho otimizado do MIM depende da aplicação das práticas recomendadas em um servidor que executa o SQL. Para obter mais informações, consulte os seguintes tópicos sobre as práticas recomendadas do SQL:

-   [As 10 Melhores Práticas Recomendadas de Armazenamento](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Otimização do Desempenho de tempdb](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [Artigo de Práticas Recomendadas do SQL Server](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizar e Recompilar Índices](http://go.microsoft.com/fwlink/?LinkID=188269)

### Arquivos de dados dimensionados previamente e de log
<a id="presize-data-and-log-files" class="xliff"></a>

Não confie no crescimento automático. Em vez disso, gerencie o crescimento desses arquivos manualmente. É possível utilizar o crescimento automático por razões de segurança, mas é necessário gerenciar de maneira proativa o crescimento dos arquivos de dados. Para tamanhos de amostra do banco de dados do MIM, consulte o [Guia de Planejamento de Capacidade do FIM](http://go.microsoft.com/fwlink/?LinkID=185246).

### Dimensionar previamente os dados do SQL e os arquivos de log
<a id="to-presize-sql-data-and-log-files" class="xliff"></a>

1.  Inicie o SQL Server Management Studio.

2.  Acesse o banco de dados FIMService, clique nele com o botão direito do mouse e, em seguida, clique em Propriedades.

3.  Na página Arquivos, aumente os arquivos do banco de dados para o tamanho necessário.

### Isolar o log dos arquivos de dados
<a id="isolate-log-from-data-files" class="xliff"></a>

Siga as práticas recomendadas do SQL Server para isolar os arquivos de transação e de log dos bancos de dados em discos físicos separados.

Criar mais arquivos tempdb

Para obter um desempenho otimizado, recomenda-se a criação de um arquivo de dados por núcleo de CPU no arquivo tempdb.

### Criar mais arquivos tempdb
<a id="to-create-additional-tempdb-files" class="xliff"></a>

1.  Inicie o SQL Server Management Studio.

2.  Acesse o banco de dados tempdb nos Bancos de Dados do Sistema, clique em tempdb com o botão direito do mouse e, em seguida, clique em Propriedades.

3.  Na página Arquivos, crie um arquivo de dados para cada núcleo da CPU. Verifique se os arquivos tempdb de dados e log estão separados em unidades e eixos diferentes.

### Assegure-se de que há espaço suficiente para os arquivos de log
<a id="ensure-adequate-space-for-log-files" class="xliff"></a>

É importante entender os requisitos de disco do modelo de recuperação. O modo de recuperação simples pode ser apropriado durante o carregamento inicial do sistema para limitar o uso de espaço em disco, mas os dados criados após o último backup ficará exposto à perda de dados. Ao utilizar o modo de recuperação completa, é necessário gerenciar o uso do disco por meio de backups, o que inclui backups frequentes do log de transações para evitar o uso excessivo do espaço em disco. Para obter mais informações, consulte [Visão geral do Modelo de Recuperação](http://go.microsoft.com/fwlink/?LinkID=185370).

### Limitar a memória do SQL Server
<a id="limit-sql-server-memory" class="xliff"></a>

Dependendo da quantidade de memória do SQL Server e se ele é compartilhado com outros serviços (ou seja, o Serviço do MIM 2016 e o Serviço de Sincronização do MIM 2016), talvez você queira restringir o consumo de memória do SQL. Isso pode ser feito por meio das etapas a seguir.

1.  Inicie o SQL Server Enterprise Manager.

2.  Selecione Nova Consulta.

3.  Execute a seguinte consulta:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  Este exemplo reconfigura o SQL Server para não usar mais do que 12 gigabytes (GB) de memória.

4.  Verifique a configuração usando a seguinte consulta:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### Configuração de backup e recuperação
<a id="backup-and-recovery-configuration" class="xliff"></a>

No geral, os backups de banco de dados devem ser executados de acordo com a política de backup da empresa. Caso os backups de log incremental não sejam planejados, o banco de dados deverá ser configurado no modo de recuperação simples. Assegure-se de que você compreendeu os efeitos dos diferentes modelos de recuperação antes de implementar uma estratégia de backup, bem como os requisitos de espaço em disco desses modelos. O modo de recuperação completa exige backups de log frequentes para evitar o uso excessivo do espaço em disco. Para obter mais informações, consulte [Visão Geral do Modelo de Recuperação](http://go.microsoft.com/fwlink/?LinkID=185370) e [Guia de Backup e Restauração do FIM 2010](http://go.microsoft.com/fwlink/?LinkID=165864).

## Criar uma conta de Administrador de Backup para o FIMService após a instalação
<a id="create-a-backup-administrator-account-for-the-fimservice-after-installation" class="xliff"></a>


>[!IMPORTANT]
Os membros do conjunto de Administradores do FIMService têm permissões exclusivas essenciais para a operação de implantação do FIM. Se não for possível fazer logon como parte do conjunto de Administradores, a única solução será reverter para um backup anterior do sistema. Para atenuar essa situação, é recomendável adicionar outros usuários para ao conjunto Administrativo do FIM, como parte da configuração pós-instalação.

## Serviço FIM
<a id="fim-service" class="xliff"></a>


### Configurar o serviço de caixa de correio do Exchange do Serviço FIM
<a id="configuring-fim-service-service-exchange-mailbox" class="xliff"></a>

Estas são as práticas recomendadas para configurar o Microsoft Exchange Server na conta de serviço do MIM 2016.

- Configure a conta de serviço para aceitar mensagens apenas de endereços de email internos. Especificamente, a caixa de correio de conta de serviço nunca deve receber mensagens de servidores SMTP externos.

#### Para configurar a conta de serviço
<a id="to-configure-the-service-account" class="xliff"></a>

1.  No Console de Gerenciamento do Exchange, selecione a **conta de serviço do Serviço FIM**.

2.  Selecione Propriedades, Configurações de Fluxo de Mensagens e, em seguida, **Restrições de Entrega de Mensagens.**

3.  Selecione a caixa de seleção **Exigir que todos os remetentes sejam autenticados**.

Para obter mais informações, consulte [Configurar Restrições de Entrega de Mensagens](http://go.microsoft.com/fwlink/?LinkID=183625).

-   Configure a conta de serviço para rejeitar mensagens com tamanhos maiores que 1 MB. Siga as práticas recomendadas para [Configurar Limites de Tamanho de Mensagem](http://go.microsoft.com/fwlink/?LinkID=183626) de uma Caixa de Correio ou uma Pasta Pública Habilitada para Email.

-   Configure a conta de serviço para ter uma cota de armazenamento de caixa de correio de 5 GB. Para obter resultados otimizados, siga as práticas recomendadas listadas na [Configurar as Cotas de Armazenamento de uma Caixa de Correio](http://go.microsoft.com/fwlink/?LinkID=156929).

## Portal MIM
<a id="mim-portal" class="xliff"></a>


### Desabilitar a indexação do SharePoint
<a id="disable-sharepoint-indexing" class="xliff"></a>

É recomendável desabilitar a indexação do Microsoft Office SharePoint®. Nenhum documento precisa ser indexado, a indexação gera muitas entradas de log de erro e possíveis problemas de desempenho com o FIM 2010. Desabilitar a indexação do SharePoint

1.  No servidor que hospeda o Portal do MIM 2016, clique em Iniciar.

2.  Clique em Todos os Programas.

3.  Na lista de Todos os Programas, clique em Ferramentas Administrativas.

4.  Em Ferramentas Administrativas, clique em Administração Central do SharePoint.

5.  Na página de Administração Central, clique em Operações.

6.  Na página Operações, em Configuração Global, clique nas Definições de Trabalho do Timer.

7.  Na página Definições de Trabalho do Timer, clique em Atualização da Pesquisa do SharePoint Services.

8.  Na página Editar Trabalho do Timer, clique em Desabilitar.

## Carregamento de Dados Inicial do MIM 2016
<a id="mim-2016-initial-data-load" class="xliff"></a>

Esta seção lista uma série de etapas para aumentar o desempenho do carregamento de dados inicial do sistema externo para o FIM 2010. É importante entender que várias dessas etapas são temporárias durante o preenchimento inicial do sistema e devem ser redefinidas após serem concluídas. Essa operação é realizada uma única vez e não é uma sincronização contínua.

>[!NOTE]
Para obter mais informações sobre a sincronização de usuários entre o FIM 2010 e os Active Directory Domain Services (AD DS), consulte [Como Sincronizar Usuários do Active Directory para o FIM](http://go.microsoft.com/fwlink/?LinkID=188277) na documentação do FIM.

>[!IMPORTANT]
Verifique se as práticas recomendadas abordadas na seção de configuração do SQL deste guia foram aplicadas.                                                                                                                                                      |

### Etapa 1: configurar o SQL Server para o carregamento de dados inicial
<a id="step-1-configure-the-sql-server-for-initial-data-load" class="xliff"></a>
Ao planejar o carregamento inicial de uma grande quantidade de dados, é possível reduzir o tempo necessário para preencher o banco de dados desativando temporariamente a pesquisa de texto completo e ativando novamente após a conclusão da exportação no agente de gerenciamento do MIM 2016 (FIM MA).

Desligar temporariamente a pesquisa de texto completo:

1.  Inicie o SQL Server Management Studio.

2.  Selecione Nova Consulta.

3.  Execute as seguintes instruções SQL:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

É importante compreender os requisitos de disco do modelo de recuperação do SQL Server. De acordo com o cronograma de backup, considere utilizar o modo de recuperação simples durante o carregamento inicial do sistema para limitar o uso do espaço em disco, mas é necessário entender as consequências em uma perspectiva de perda de dados.
Ao utilizar o modo de recuperação completa, é necessário gerenciar o uso do disco por meio de backups, o que inclui backups frequentes do log de transações para evitar o uso excessivo do espaço em disco.

>[!IMPORTANT]
Não implementar esses procedimentos pode gerar um uso excessivo do espaço em disco, o que pode fazer com que ele acabe. É possível encontrar mais detalhes sobre este tópico em [Visão Geral do Modelo de Recuperação](http://go.microsoft.com/fwlink/?LinkID=185370). O [Guia de Backup e Restauração do FIM](http://go.microsoft.com/fwlink/?LinkID=165864) contém mais informações.

### Etapa 2: aplicar a configuração de MIM mínima necessária durante o processo de carregamento
<a id="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process" class="xliff"></a>

Durante o processo de carregamento inicial, apenas a configuração mínima necessária deve ser aplicada à configuração do FIM das regras de gerenciamento de política (MPRs) e definições de configuração. Após o carregamento de dados, crie os conjuntos adicionais necessários para a implantação. Use a configuração Executar na Atualização da Política nos fluxos de trabalho da ação para aplicar essas políticas retroativamente aos dados carregados.

### Etapa 3: configurar e preencher o Serviço FIM com os dados de identidade externa
<a id="step-3-configure-and-populate-the-fim-service-with-external-identity-data" class="xliff"></a>


Neste ponto, é necessário seguir os procedimentos descritos em Como Sincronizar Usuários do Active Directory

Guia dos Serviços de Domínio para o FIM para configurar e sincronizar seu sistema com usuários do Active Directory. Se for necessário sincronizar informações de Grupo, os procedimentos desse processo serão descritos em Como Sincronizar Grupos, no Guia dos Active Directory Domain Services para o FIM.

#### Sequências de sincronização e exportação
<a id="synchronization-and-export-sequences" class="xliff"></a>

Para otimizar o desempenho, execute uma exportação após uma execução de sincronização que resulta em um grande número de operações de exportação pendentes em um espaço conector.

Em seguida, execute uma confirmação de importação no agente de gerenciamento associado ao espaço conector afetado. Por exemplo, quando for necessário executar perfis de execução de sincronização em diversos agentes de gerenciamento como parte de um carregamento de dados inicial, execute uma exportação seguida de uma importação delta após cada sincronização individual executada.

Para cada agente de gerenciamento de origem que faz parte do ciclo de inicialização, execute as seguintes etapas:

1.  Importação completa em um agente de gerenciamento de origem.

2.  Sincronização completa no agente de gerenciamento de origem.

3.  Exportação em todos os agentes de gerenciamento de destino afetados com operações de exportação em etapas.

4.  Importação delta em todos os agentes de gerenciamento de destino afetados com operações de exportação em etapas.

### Etapa 4: aplicar a configuração completa do MIM
<a id="step-4-apply-your-full-mim-configuration" class="xliff"></a>


Quando o carregamento de dados inicial for concluído, aplique a configuração completa do MIM à implantação.

De acordo com os cenários, pode-se incluir a criação de conjuntos adicionais, MPRs e fluxos de trabalho. Para todas as políticas que devem ser aplicadas retroativamente a todos os objetos existentes no sistema, utilize a configuração Executar na Atualização da Política nos fluxos de trabalho da ação para aplicar essas políticas retroativamente aos dados carregados.

### Etapa 5: reconfigurar o SQL com as configurações anteriores
<a id="step-5-reconfigure-sql-to-previous-settings" class="xliff"></a>


Lembre-se de alterar a configuração do SQL para suas configurações normais. Isso inclui:

-   Ativar a pesquisa de texto completo

-   Atualizar a política de backup de acordo com a política da empresa

Após a conclusão do carregamento de dados inicial, é necessário ativar novamente a pesquisa de texto completo. Execute as seguintes instruções SQL para ativar novamente a pesquisa de texto completo:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Caso seja necessário mudar para o modo de recuperação simples, reconfigure o cronograma de backup de acordo com a política de backup da empresa. Mais detalhes sobre os cronogramas de backup do FIM estão disponíveis no [Guia de Backup e Restauração do FIM](http://go.microsoft.com/fwlink/?LinkID=165864).

## Migração da Configuração
<a id="configuration-migration" class="xliff"></a>


### Evite alterar nomes de exibição
<a id="avoid-changing-display-names" class="xliff"></a>

Para muitos tipos de objeto, como MPRs, o script syncproduction.ps1 usa o nome de exibição como o único atributo de âncora entre dois sistemas. Consequentemente, uma alteração no nome de exibição de uma MPR existente resulta na sua exclusão, seguida pela criação de uma nova MPR. Isso acontece porque o processo de migração não pode associar com êxito as MPRs cujos critérios de associação foram alterados. Para evitar esse problema, é possível associar um atributo personalizado a todos os tipos de objeto de configuração e usar esse atributo como critérios de associação. Assim, é possível modificar os nomes de exibição sem afetar o processo de migração.

### Evite alterar o conteúdo dos arquivos intermediários
<a id="avoid-changing-the-content-of-intermediate-files" class="xliff"></a>

Embora o formato de arquivo e a interface de programação de aplicativo (API) dos objetos de baixo nível sejam públicos e haja suporte dos desenvolvedores para as manipulações, não é recomendado alterar os conteúdos dos formatos intermediários durante a migração. No entanto, pode ser necessário remover todo o ImportObjects do changes.xml ou executar operações de localizar e substituir no pilot.xml para substituir números de versão ou informações do Sistema de Nome de Domínio (DNS) por informações de produção do DNS.

### Verifique se o número de versão está correto no pilot.xml durante a migração entre versões
<a id="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions" class="xliff"></a>

Apesar de as migrações entre números de versão não serem recomendadas e não terem suporte, é possível realizá-las por meio da substituição do número de versão piloto pelo número de versão de produção no pilot.xml. Especificamente, objetos WorkflowDefinition e

ActivityInformationConfiguration exigem que o número de versão se refira com precisão às atividades de fluxo de trabalho no ambiente de produção. A falha ao substituir o número de versão faz com que o cmdlet Compare-FIMConfig identifique diferenças entre os atributos de Extensible Object Markup Language (XOML) em WorkflowDefinitions e migre o número de versão piloto. O Serviço FIM de produção pode falhar ao iniciar atividades de fluxo de trabalho com o número de versão incorreto.

### Evite referências cíclicas
<a id="avoid-cyclic-references" class="xliff"></a>

Em geral, as referências cíclicas não são recomendadas em uma configuração de MIM.
No entanto, às vezes os ciclos ocorrem quando um conjunto A se refere ao conjunto B e o conjunto B também se refere ao conjunto A. Para evitar problemas com referências cíclicas, altere a definição de um conjunto A ou B para que um não faça referência ao outro. Então, reinicie o processo de migração. Se houver referências cíclicas e o cmdlet Compare-FIMConfig tiver um erro como resultado, será necessário interromper o ciclo manualmente. Como o cmdlet Compare-FIMConfig gera uma lista de alterações em ordem de precedência, é necessário que não exista nenhum ciclo entre as referências de objetos de configuração.

## Segurança 
<a id="security" class="xliff"></a>

### Conta do MIM MA
<a id="mim-ma-account" class="xliff"></a>

A conta do MIM MA não é considerada uma conta de serviço e deve ser uma conta de usuário normal. As contas devem fazer logon localmente para que a conta de serviço do Serviço de Sincronização do FIM possa representá-lo.

Habilitar o logon local na conta do MIM MA

1.  Clique em Iniciar, Ferramentas Administrativas e em Política de Segurança Local.

2.  Abra o nó Políticas Locais e, em seguida, clique em Atribuição de Direitos de Usuário.

3.  Na política Permitir Logon Local, verifique se a conta do FIM MA está especificada explicitamente ou a adicione a um dos grupos que têm acesso.

### Contas do Serviço de Sincronização FIM e dos Serviços FIM
<a id="fim-synchronization-service-and-fim-services-accounts" class="xliff"></a>

Para configurar os servidores que executam os componentes do servidor MIM de maneira segura, as contas de serviço devem ser restritas. Usando o procedimento anterior para ativar a conta do MIM MA, defina as seguintes restrições nas contas do Serviço de Sincronização FIM e dos Serviços FIM:

-   Negar logon como um trabalho em lotes

-   Negar logon localmente

-   Negar acesso a este computador pela rede

As contas de serviço não devem ser membros do grupo local de administradores.

A conta de serviço do Serviço de Sincronização do FIM não deve ser um membro dos grupos de segurança usados para controlar o acesso ao Serviço de Sincronização do FIM (grupos que iniciam com FIMSync, por exemplo, FIMSyncAdmins e assim por diante).

>[!IMPORTANT]
 Se as opções de usar a mesma conta para ambas as contas de serviço forem selecionadas e o Serviço de Sincronização FIM e os Serviços FIM forem separados, não será possível negar acesso a esse computador na rede no servidor de Serviço de Sincronização mms. Se o acesso for negado, o Serviço FIM seria proibido de contatar o Serviço de Sincronização FIM para alterar configurações e gerenciar senhas.

### A redefinição de senha implantada em computadores do tipo quiosque deve configurar a segurança local para limpar o arquivo de paginação da memória virtual
<a id="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile" class="xliff"></a>

Ao implantar a redefinição de senha do FIM em uma estação de trabalho destinada a ser um quiosque, recomenda-se que a política de segurança local Encerrar: Limpar do arquivo de paginação da memória virtual seja ativada para garantir que informações confidenciais da memória do processo não sejam disponibilizadas para usuários não autorizados.

### Implementando o SSL para o Portal do FIM
<a id="implementing-ssl-for-the-fim-portal" class="xliff"></a>

É altamente recomendável usar o protocolo SSL (Secure Sockets Layer) no servidor do Portal do FIM para proteger o tráfego entre os clientes e o servidor.

Para implementar o SSL:

1.  No servidor do Portal do MIM, abra o Gerenciador do IIS.

2.  Clique no nome do computador local.

3.  Clique em Certificados do Servidor.

4.  Clique em Criar uma Solicitação de Certificado.

5.  Na caixa de texto Nome Comum, digite o nome do servidor.

6.  Clique em Avançar e em Avançar novamente.

7.  Salve o arquivo em qualquer local. Será necessário acessar esse local nas próximas etapas.

8.  No Internet Explorer®, acesse https://servername/certsrv. Substitua servername pelo nome do servidor que emite certificados.

9.  Clique em Solicitar um Novo Certificado.

10. Clique em Enviar uma Solicitação Avançada.

11. Clique em Enviar uma Solicitação de Certificado Usando um Codificado em Base 64.

12. Cole os conteúdos do arquivo salvo na etapa anterior.

13. Em Modelo de Certificado, selecione Servidor Web.

14. Clique em Enviar.

15. Salve o certificado na área de trabalho.

16. No Gerenciador do IIS, clique em Concluir Solicitação de Certificado.

17. Aponte o Gerenciador do IIS para o certificado salvo na área de trabalho.

18. Em Nome Amigável, digite o nome do servidor.

19. Clique em Sites e, em seguida, selecione SharePoint – 80.

20. Clique em Associações e, em seguida, clique em Adicionar.

21. Selecione https.

22. Selecione o certificado que tem o mesmo nome do servidor (este é o certificado que você acabou de importar).

23. Clique em OK.

24. Remova a associação de HTTP.

25. Clique em Configurações de SSL e marque Exigir SSL.

26. Salve as configurações.

27. Clique em Iniciar, em Ferramentas Administrativas e em Central Administrativa do SharePoint 3.0.

28. Clique em Operações e, em seguida, clique em Mapeamentos Alternativos de Acesso.

29. Clique em http://servername.

30. Alterar http://servername para https://servername e, em seguida, clique em OK.

31. Clique em Iniciar, clique em Executar, digite iisreset e clique em OK.

## Desempenho
<a id="performance" class="xliff"></a>

Para uma configuração de desempenho otimizada:

-   Aplique as práticas recomendadas de configuração do SQL, conforme descrito na seção de configuração do SQL deste documento.

-   Desligue a indexação do SharePoint no site do Portal do FIM 2010 R2. Para obter mais informações, consulte a seção Desabilitar a Indexação do SharePoint neste documento.

## Práticas Recomendadas para Recursos Específicos (Quero remover e recolher esta seção e ver apenas os recursos específicos nos níveis de cabeçalho 2 e 3)
<a id="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3" class="xliff"></a>


### Gerenciamento de Solicitações
<a id="request-management" class="xliff"></a>

Por padrão, o MIM 2016 limpa objetos expirados do sistema, o que inclui solicitações concluídas com aprovações associadas, respostas de aprovação e instâncias de fluxo de trabalho em um intervalo de 30 dias. Caso a empresa necessite de um histórico de solicitação maior, exporte as solicitações do MIM e armazene-as em um banco de dados auxiliar para preservá-las além dos 30 dias. Embora a janela de 30 dias para a exclusão de solicitações possa ser configurada, prolongá-la pode afetar negativamente o desempenho devido aos outros objetos do sistema.

### Regras de Política de Gerenciamento
<a id="management-policy-rules" class="xliff"></a>

#### Use o tipo de MPR apropriado
<a id="use-the-appropriate-mpr-type" class="xliff"></a>

O MIM oferece dois tipos de MPRs, Solicitação e Definir Transição:

-   MPR de Solicitação (RMPR)

 - É utilizada para definir a política de controle de acesso (autenticação, autorização e ação) para as operações de Criar, Ler, Atualizar ou Excluir (CRUD) em recursos.
 - É aplicada quando uma operação CRUD é feita em um recurso de destino no FIM.
   - É projetada pelos critérios de correspondência definidos na regra, ou seja, a quais solicitações CRUD a regra se aplica.


-   MPR Definir Transição (TMPR)
 - É utilizada para definir políticas, independentemente de como o objeto entrou no estado atual representado pelo Conjunto de Transição. Use a TMPR para criar políticas de direito.
 - É aplicada quando um recurso entra ou sai de um conjunto associado.
 - É projetada para os membros do conjunto.

>[OBSERVAÇÃO] Para obter mais detalhes, consulte [Criar Regras de Política de Negócios](http://go.microsoft.com/fwlink/?LinkID=183691).

#### Habilite as MPRs conforme o necessário
<a id="only-enable-mprs-as-necessary" class="xliff"></a>

Use o princípio de menos privilégios ao aplicar a configuração. As MPRs controlam a política de acesso à implantação do FIM. Habilite apenas os recursos usados pela maioria dos usuários. Por exemplo, nem todos os usuários usam o FIM para gerenciamento de grupo, assim, as MPRs associadas de gerenciamento de grupo devem ser desabilitadas. Por padrão, o FIM é fornecido com a maioria das permissões de não administrativas desabilitadas.

#### Duplicar MPRs internas em vez de modificá-las diretamente
<a id="duplicate-built-in-mprs-instead-of-directly-modifying" class="xliff"></a>

Caso precise modificar as MPRs internas, crie uma nova MPR com a configuração necessária e desligue a MPR interna. Isso garante que alterações futuras de MPRs internas introduzidas pelo processo de atualização não afetem negativamente a configuração do sistema.

#### As permissões do usuário final devem utilizar listas de atributo explícitas projetadas para as necessidades dos usuários da empresa
<a id="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs" class="xliff"></a>

Usar listas de atributo explícitas ajuda a evitar a concessão acidental de permissões a usuários sem privilégios quando os atributos são adicionados aos objetos.
Os administradores devem ter uma necessidade explícita para conceder acesso a novos atributos em vez de tentar remover o acesso.

O acesso aos dados deve estar alinhado com as necessidades empresariais dos usuários. Por exemplo, os membros do grupo não devem ter acesso ao atributo de filtro do seu grupo ao qual pertencem. O filtro pode revelar de modo acidental dados organizacionais a que o usuário não teria acesso normalmente.

#### As MPRs devem refletir as permissões efetivas no sistema
<a id="mprs-should-reflect-effective-permissions-in-the-system" class="xliff"></a>

Evite conceder permissões a atributos que o usuário nunca poderá usar. Por exemplo, não conceda permissão para modificar atributos de recursos de núcleo, como o objectType. Apesar da MPR, qualquer tentativa de modificar o tipo do recurso depois de sua criação será negada pelo sistema.

#### Ao usar atributos explícitos em MPRs, as permissões de Leitura devem ser separadas das permissões de Modificação e Criação
<a id="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs" class="xliff"></a>

Ao listar os atributos de forma explícita em MPRs, os atributos necessários para Criação e Modificação são normalmente distintos dos disponíveis para leitura. Por exemplo, a permissão de Leitura pode ser concedida em atributos do Sistema como Criador ou objectId, enquanto Criação ou Modificação não podem ser especificadas para atributos do Sistema.

#### Ao usar atributos explícitos em regras, as permissões de Criação devem ser separadas das permissões de Modificação
<a id="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules" class="xliff"></a>

A operação de Criação exige que o usuário selecionar o objectType como parte de sua operação. Este é um atributo do sistema de núcleo que não pode ser modificado após uma operação de Criação.

#### Use uma MPR de solicitação para todos os atributos com os mesmos requisitos de acesso
<a id="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements" class="xliff"></a>

Para atributos com os mesmos requisitos de acesso que não devem ser alterados, é possível combiná-los em uma única MPR de solicitação para maior eficiência.

#### Evite fornecer acesso irrestrito até mesmo para grupos de entidades selecionadas
<a id="avoid-giving-unrestricted-access-even-to-selected-principal-groups" class="xliff"></a>

No FIM, as permissões são definidas como uma asserção positiva. Como o FIM não dá suporte às permissões de Negação, conceder acesso irrestrito a um recurso complica a distribuição de exclusões nas permissões. Como prática recomendada, conceda somente as permissões necessárias.

>[!NOTE]
A seguir, consulte a seção de direitos. Estou pensando em como mesclá-los para evitar a criação de cabeçalhos de cinco níveis
#### Use TMPRs para definir direitos personalizados
<a id="use-tmprs-to-define-custom-entitlements" class="xliff"></a>

Use MPRs Definir Transição (TMPRs) em vez de RMPRs para definir direitos personalizados.
As TMPRs oferecem um modelo baseado em estado para atribuir ou remover direitos com base na associação em Conjuntos de Transição definidos ou funções, e nas atividades de fluxo de trabalho que os acompanham. As TMPRs sempre devem ser definidas em pares, um para a transição de recursos de entrada e outro para recursos de transição de saída. Além disso, cada MPR de transição deve conter fluxos de trabalho separados para atividades de provisionamento e desprovisionamento.

>[!NOTE]
Qualquer fluxo de trabalho de desprovisionamento deve garantir que o atributo Executar na Atualização da Política está definido como true.

#### Habilite a MPR Definir Transição de Entrada por último
<a id="enable-the-set-transition-in-mpr-last" class="xliff"></a>

Ao criar um par de TMPRs, ative a MPR Definir Transição de Entrada por último. Essa ordem garante que nenhum recurso fique com o direito se for adicionado e removido do conjunto enquanto a MPR de Entrada está ativada, mas antes de a MPR de Saída ser ativada.

#### Os fluxos de trabalho da TMPR devem verificar o estado do recurso de destino primeiro
<a id="workflows-in-tmpr-should-check-target-resource-state-first" class="xliff"></a>

Os fluxos de trabalho de provisionamento devem ser verificados primeiro para determinar se o recurso de destino já foi provisionado de acordo com o direito. Em caso afirmativo, nada acontecerá.

Os fluxos de trabalho de desprovisionamento devem ser verificados primeiro para determinar se o recurso de destino foi provisionado. Em caso afirmativo, será necessário desprovisionar o recurso de destino.
Caso contrário, nada acontecerá.

#### Selecione Executar na Atualização da Política de TMPRs
<a id="select-run-on-policy-update-for-tmprs" class="xliff"></a>

Assim, garante-se que o comportamento de provisionamento correto seja aplicado quando as atualizações de política forem implementadas e o sinalizador de Executar na Atualização da Política seja usado em fluxos de trabalho de ação associados às TMPRs. Assim, garante-se que as alterações nas definições da política apliquem os fluxos de trabalho de ação a novos membros do Conjunto de Transição.

#### Evite associar o mesmo direito a dois Conjuntos de Transição diferentes
<a id="avoid-associating-the-same-entitlement-with-two-different-transition-sets" class="xliff"></a>

Associar o mesmo direito a dois Conjuntos de Transição diferentes pode causar uma revogação desnecessária e uma nova concessão de direitos caso o recurso seja movido de um conjunto para outro. Como prática recomendada, certifique-se de que um conjunto contém todos os recursos que exigem o direito associado. Assim, garante-se uma relação personalizada entre o Conjunto de Transição e o direito que concede o fluxo de trabalho.

#### Use uma sequência apropriada de operações ao remover direitos no sistema
<a id="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system" class="xliff"></a>

A ordem das etapas executadas durante a remoção de direitos no sistema pode ter dois resultados operacionais diferentes. Assegure-se de que você compreendeu qual ordem se aplica ao efeito desejado.

Para remover um direito do sistema (e revogá-lo de todos os membros que têm esse direito no momento):

1.  Desabilite a MPR T-In. Assim, evita-se novas concessões.

2.  Exclua o filtro T-Set ou altere-o para que o conjunto fique vazio. Isso faz com que todos os membros existentes realizem uma transição de saída e que a política de transição de saída seja aplicada, incluindo os fluxos de trabalho de desprovisionamento configurado associados ao direito.

3.  Desabilite a MPR T-Out.

Remover um direito, mas deixar os membros atuais isolados (por exemplo, parar de usar o FIM para gerenciar o direito):

1.  Desabilite a MPR T-In. Assim, evita-se novas concessões.

2.  Desabilite a MPR T-Out.

3.  Exclua o filtro T-Set ou altere-o para que o conjunto fique vazio. Como o conjunto não está mais vinculado a uma TMPR, nenhum fluxo de trabalho de desprovisionamento será aplicado.

### Define
<a id="sets" class="xliff"></a>

Ao aplicar as práticas recomendadas aos conjuntos, é necessário considerar o impacto das otimizações na capacidade de gerenciamento e na facilidade de administração no futuro.
Os testes apropriados na escala de produção esperada devem ser executados a fim de identificar o equilíbrio correto entre desempenho e capacidade de gerenciamento antes da aplicação de tais recomendações.

>[!NOTE]
As orientações a seguir se aplicam a conjuntos e grupos dinâmicos.


#### Reduza o uso do aninhamento dinâmico
<a id="minimize-the-use-of-dynamic-nesting" class="xliff"></a>

Isso diz respeito ao filtro de um conjunto que faz referência ao atributo ComputedMember de outro conjunto. Um motivo comum para aninhar conjuntos é evitar a duplicação de uma condição de associação entre diversos conjuntos. Embora essa abordagem possa resultar em uma melhor capacidade de gerenciamento dos conjuntos, há uma compensação de desempenho. É possível otimizar o desempenho duplicando as condições de associação de um conjunto aninhado, em vez de aninhar o próprio conjunto.

Há casos em que, para cumprir um requisito funcional, não é possível evitar o aninhamento de conjuntos. Essas são as principais situações em que se deve aninhar conjuntos. Por exemplo, para definir o conjunto de todos os grupos que não têm proprietários de Funcionários de Tempo Integral, o aninhamento de conjuntos deve ser usado desta maneira: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, em que “X” é o ObjectID do conjunto de Todos os Funcionários de Tempo Integral.

#### Reduza o uso de condições negativas
<a id="minimize-the-use-of-negative-conditions" class="xliff"></a>

As condições negativas são as condições de associação que usam os operadores ou as funções a seguir: `!=`, `not()`, `\<` e `\<=`. Para otimizar o desempenho, sempre que possível, expresse a condição desejada com várias condições positivas em vez de uma condição de negativa.

#### Reduza o uso de condições de associação baseadas em atributos de referência multivalorados
<a id="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes" class="xliff"></a>

O uso de condições baseadas em atributos de referência multivalorados deve ser reduzido, pois muitos desses conjuntos podem afetar o desempenho de operações do atributo usado na condição de associação.

### Redefinição de senha
<a id="password-reset" class="xliff"></a>

#### Computadores do tipo quiosque usados para redefinição de senha devem configurar a segurança local para limpar o arquivo de paginação da memória virtual
<a id="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile" class="xliff"></a>

Ao implantar a redefinição de senha do FIM 2010 em uma estação de trabalho destinada a ser um quiosque, recomenda-se que a política de segurança local Encerrar: Limpar do arquivo de paginação da memória virtual seja ativada para garantir que informações confidenciais da memória do processo não sejam disponibilizadas para usuários não autorizados.

#### Os usuários sempre devem se registrar para redefinir a senha em um computador ao qual estão conectados
<a id="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to" class="xliff"></a>

Quando um usuário tenta se registrar para redefinir a senha por meio de um portal da Web, o FIM 2010 sempre inicia o registro em nome do usuário conectado, independentemente de quem está conectado ao site da Web. Os usuários sempre devem se registrar para redefinir a senha em um computador ao qual estão conectados.

#### Não defina a chave do Registro AvoidPdcOnWan como true
<a id="do-not-set-the-avoidpdconwan-registry-key-to-true" class="xliff"></a>

Ao usar a redefinição de senha do MIM 2016, não defina a chave do Registro AvoidPdcOnWan como true.

Caso esta chave do Registro seja definida como true, o usuário muito provavelmente passará pelos portões de senha, terá sua senha redefinida no controlador de domínio primário (PDC) e tentará fazer logon. Em virtude dessa chave do Registro, o controlador de domínio local não executa a validação secundária com o PCD e, como consequência, nega a solicitação de logon. Se o usuário for negado várias vezes, ele poderá ser bloqueado no domínio e será necessário contatar o suporte.

#### Não ative o registro em log de senhas com texto não criptografado
<a id="do-not-turn-on-logging-of-clear-text-passwords" class="xliff"></a>

É possível registrar em log as senhas com texto não criptografado ao ativar o rastreamento de Nível de Serviço de diagnóstico no Windows

Communication Foundation (WCF). Essa opção não é ativada por padrão e é desaconselhável de ativá-la em ambientes de produção. Essas senhas são visíveis como elementos de texto não criptografado em uma mensagem criptografada do protocolo SOAP quando os usuários se registram para a redefinição de senha. Para obter mais informações, consulte [Configurando o Log de Mensagens](http://go.microsoft.com/fwlink/?LinkID=168572).

#### Não mapeie um fluxo de trabalho de autorização para o processo de redefinição de senha
<a id="do-not-map-an-authorization-workflow-to-the-password-reset-process" class="xliff"></a>

Não anexe um fluxo de trabalho de autorização a uma operação de redefinição de senha.
A redefinição de senha exige uma resposta síncrona e fluxos de trabalho de autorização que contêm atividades, como a atividade de aprovação, são assíncronos.

#### Não mapeie várias atividades de ação na redefinição de senha
<a id="do-not-map-multiple-action-activities-to-password-reset" class="xliff"></a>

Não anexe um fluxo de trabalho que contenha mais que uma ação de atividade a uma operação de redefinição de senha. Um cenário de exemplo seria anexar uma segunda atividade de redefinição de senha do AD DS a uma MPR de redefinição de senha. Não há suporte para esse cenário.

#### Solicite um novo registro ao adicionar, remover ou alterar a ordem das atividades em um fluxo de trabalho existente
<a id="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow" class="xliff"></a>

Ao adicionar, remover ou alterar a ordem de atividades de autenticação em um fluxo de trabalho existente, sempre selecione a opção de solicitar novo registro. Os usuários que tentarem autenticar uma redefinição de senha após a adição ou remoção de uma atividade de um fluxo de trabalho antes do novo registro podem obter efeitos indesejados.

### Configuração do Portal e Configuração de Exibição de Controle de Recurso
<a id="portal-configuration-and-resource-control-display-configuration" class="xliff"></a>

#### Considere adicionar uma declaração de privacidade à página de perfil do usuário
<a id="consider-adding-a-privacy-disclaimer-to-the-user-profile-page" class="xliff"></a>

No MIM, por padrão, algumas informações de perfil do usuário podem ser exibidas para outros usuários. Como cortesia para os usuários, os administradores devem considerar adicionar texto personalizado consistente com as políticas da empresa para a página de Perfil do Usuário. Para obter mais informações sobre como adicionar texto personalizado a uma página do Portal do MIM, consulte Introdução à [Configuração e Personalização do Portal do FIM](http://go.microsoft.com/fwlink/?LinkID=165848).

### Schema
<a id="schema" class="xliff"></a>

#### Não exclua os tipos de recurso de Pessoa ou Grupo
<a id="do-not-delete-person-or-group-resource-types" class="xliff"></a>

Embora os tipos de recurso de Pessoa e Grupo não estejam marcados como tipos de recursos de Núcleo, os próprios recursos ou os atributos atribuídos a eles não devem ser excluídos. A interface do usuário (IU) no Portal do MIM requer que os tipos de recurso de Pessoa e Grupo e seus atributos estejam presentes.

#### Não modifique os atributos de núcleo
<a id="do-not-modify-the-core-attributes" class="xliff"></a>

Há 13 atributos de Núcleo atribuídos a todos os tipos de recursos. Não modifique sua relação com qualquer tipo de recurso. Os 13 atributos de Núcleo são:

-   CreatedTime

-   Criador

-   DeletedTime

-   Descrição

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Localidade

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Não exclua o recurso de esquema com uma dependência nos requisitos de auditoria

Não exclua os recursos de esquema enquanto ainda tiver requisitos de auditoria para esses recursos.

#### Fazer com que expressões regulares não diferenciem maiúsculas de minúsculas
<a id="making-regular-expressions-case-insensitive" class="xliff"></a>

No FIM, pode ser útil para fazer com que algumas expressões regulares não diferenciem maiúsculas de minúsculas. É possível ignorar a diferenciação entre maiúsculas e minúsculas usando ?!:. Por exemplo, para o Tipo de Funcionário, use

`\^(?!:contractor\|full time employee)%.`

#### Cálculo do atributo de membro
<a id="calculation-of-the-member-attribute" class="xliff"></a>

O atributo de membro exposto ao mecanismo de sincronização, na verdade, é mapeado para ComputedMembers. É uma combinação entre membros baseados em critérios e membros selecionados manualmente. Mesmo se todos os três atributos forem adicionados (Filtro, ExplicitMembers e ComputedMembers), o cálculo dinâmico do atributo de membro não ocorrerá em tipos de recurso diferentes de grupo e conjunto.

#### Espaços à direita e à esquerda em cadeias de caracteres são ignorados
<a id="leading-and-trailing-spaces-in-strings-are-ignored" class="xliff"></a>

No FIM, é possível inserir cadeias de caracteres com espaços à esquerda e à direita, mas o sistema do FIM ignora esses espaços. Se uma cadeia de caracteres com espaços à esquerda e à direita for enviada, o mecanismo de sincronização e os serviços Web ignorarão esses espaços.

#### Cadeias de caracteres vazias não são iguais a null
<a id="empty-strings-do-not-equal-null" class="xliff"></a>

Cadeias de caracteres vazias não são iguais a null nesta versão do FIM. Entradas de cadeia de caracteres vazias são consideradas um valor válido. Ausente é considerado um valor nulo.

### Processamento de Fluxo de Trabalho e Solicitação
<a id="workflow-and-request-processing" class="xliff"></a>

#### Não exclua os fluxos de trabalho padrão fornecidos com o MIM 2016
<a id="do-not-delete-default-workflows-that-are-shipped-with-mim-2016" class="xliff"></a>

Os seguintes fluxos de trabalho são fornecidos com o FIM 2010 e não devem ser excluídos:

-   Fluxo de trabalho de expiração

-   Fluxo de trabalho de validação de filtro para administradores

-   Fluxo de trabalho de validação de filtro para usuários sem privilégios de administrador

-   Fluxo de trabalho de notificação de expiração grupo

-   Fluxo de trabalho de validação de grupo

-   Fluxo de trabalho de aprovação do proprietário

-   Fluxo de trabalho de ação de redefinição de senha

-   Fluxo de trabalho AuthN de redefinição de senha

-   Validação do solicitante com autorização do proprietário

-   Validação do solicitante sem autorização do proprietário

-   Fluxo de trabalho do sistema necessário para o registro

#### Não execute dois ou mais ApprovalActivities em paralelo
<a id="do-not-run-two-or-more-approvalactivities-in-parallel" class="xliff"></a>

Não execute dois ou mais ApprovalActivities em paralelo. Isso pode fazer com que a solicitação fique presa na fase de autorização. Para diversas aprovações, inclua uma lista maior de aprovadores na aprovação ou sequencie as duas atividades.

#### Atividades de autorização não devem modificar os dados de recursos do MIM
<a id="authorization-activities-should-not-modify-mim-resources-data" class="xliff"></a>

Evite usar as atividades que modificam os recursos do MIM, como a Atividade do Avaliador de Função, como parte dos fluxos de trabalho em fluxos de trabalho de autorização. Como a solicitação não foi confirmada no ponto de autorização do processamento, quaisquer modificações realizadas nas informações de identidade podem ser aplicadas, apesar da possível rejeição da solicitação.

### Noções Básicas das Partições de Serviço do FIM
<a id="understanding-fim-service-partitions" class="xliff"></a>

O objetivo do FIM é processar solicitações que podem ser iniciadas por vários clientes, como o serviço de sincronização do FIM e os componentes de autoatendimento, de acordo com as políticas configuradas pela empresa. Por padrão, cada instância de serviço do FIM pertence a um grupo lógico composto por uma ou mais instâncias de serviço do FIM, também conhecidas como partição de serviço do FIM. Caso haja apenas uma instância de serviço do FIM implantada para manipular todas as solicitações, podem haver latências de processamento. Algumas operações podem até exceder os valores de tempo limite padrão apropriados para operações de autoatendimento. As partições de serviço do FIM podem ajudá-lo a resolver esse problema. Para obter mais informações, consulte Noções Básicas das Partições de Serviço do FIM.
