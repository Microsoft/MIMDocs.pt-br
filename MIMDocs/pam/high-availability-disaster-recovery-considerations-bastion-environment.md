---
title: Recuperação de desastre do PAM | Microsoft Docs
description: Saiba como configurar o Privileged Access Management para alta disponibilidade e recuperação de desastre.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 67ce70e6bc0603a991731cf1e5fb95751f5016c6
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043963"
---
# <a name="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment"></a>Considerações sobre alta disponibilidade e recuperação de desastre do ambiente de bastiões

Este artigo descreve as considerações sobre alta disponibilidade e recuperação de desastre ao implantar o AD DS (Serviços de Domínio do Active Directory) e o MIM (Microsoft Identity Manager) 2016 no PAM (Privileged Access Management).

As empresas se concentram em alta disponibilidade e recuperação de desastre para cargas de trabalho no Windows Server, SQL Server e Active Directory. Mas a disponibilidade confiável do ambiente de bastiões do Privileged Access Management também é importante. O ambiente de bastiões é uma parte essencial da infraestrutura de TI da organização, pois os usuários interagem com seus componentes a fim de assumirem funções administrativas. Para obter mais informações sobre alta disponibilidade em geral, baixe o white paper [Microsoft High Availability Overview](https://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc) (Visão geral da Alta Disponibilidade da Microsoft).

## <a name="high-availability-and-disaster-recovery-scenarios"></a>Cenários de alta disponibilidade e recuperação de desastre

Ao planejar a alta disponibilidade e recuperação de desastre, leve em consideração as seguintes perguntas:

- Quais funções podem ser afetadas por uma interrupção?
- Quais funções são críticas para os negócios e/ou críticas para as operações de TI?
- Quais são os riscos que poderiam resultar em uma interrupção desses sistemas?

O escopo dessas considerações afeta o custo total da implantação e das operações e, portanto, as organizações podem priorizar determinadas funções em detrimento de outras, além de aceitar o risco de interrupções temporárias de funções com prioridade mais baixa. A seguinte tabela descreve uma possível classificação de prioridade que poderia ser usada por uma organização:

| **Função da floresta de bastiões** | **Prioridade relativa durante a recuperação** | **Mitigação, se a função não estiver disponível** |
| --------------------------- | --------------------- | -------------- |
| Estabelecimento de relação de confiança         | Baixo | Aguarde até que o ambiente de bastiões seja restaurado |
| Mitigação de usuário e de grupo   | Baixo | Aguarde até que o ambiente de bastiões seja restaurado |
| Administração do MIM          | Baixo | Aguarde até que o ambiente de bastiões seja restaurado |
| Ativação de função privilegiada  | Médio | Contas dedicadas com backup de cartão inteligente para adicionar manualmente os usuários aos grupos administrativos |
| Gerenciamento de recursos         | Alta | Contas dedicadas com backup de cartão inteligente para adicionar manualmente os usuários aos grupos administrativos |
| Monitoramento de usuários e grupos na floresta existente | Baixo | Aguarde até que o ambiente de bastiões seja restaurado |

Agora vamos dar uma olhada em cada uma dessas funções da floresta de bastiões, individualmente.

### <a name="trust-establishment"></a>Estabelecimento de relação de confiança

Deve haver uma relação de confiança de floresta entre os domínios da floresta existente e a floresta do ambiente de bastiões. Isso é recomendável para que os usuários que autenticam no ambiente de bastiões possam administrar os recursos nas florestas existentes. Pode ser necessária uma configuração adicional, por exemplo, para permitir a migração de usuários dos domínios existentes em versões anteriores do Windows Server.

O estabelecimento da relação de confiança exige que os controladores de domínio da floresta existente estejam online, bem como os componentes do MIM e do AD do ambiente de bastiões.  Se houver uma interrupção de qualquer um desses componentes durante o estabelecimento da relação de confiança, o administrador poderá tentar novamente depois que a interrupção tiver sido resolvida.  Caso os controladores de domínio da floresta existente ou o ambiente de bastiões tenham sido recuperados após uma interrupção, o MIM também incluirá os cmdlets `Test-PAMTrust` e `Test-PAMDomainConfiguration` do PowerShell que poderão ser usados para verificar se uma relação de confiança existe.

### <a name="user-and-group-migration"></a>Migração de usuário e de grupo

Após o estabelecimento da relação de confiança, grupos de sombra podem ser criados no ambiente de bastiões, bem como contas de usuário para membros desses grupos e aprovadores. Isso permite que esses usuários ativem funções privilegiadas e obtenham novamente associações efetivas a um grupo.

A migração de usuário e grupo exige que os controladores de domínio da floresta existente estejam online, bem como os componentes do MIM e do AD do ambiente de bastiões.   Se os controladores de domínio da floresta existente não estiverem acessíveis, nenhum usuário e grupo adicional poderá ser adicionado ao ambiente de bastiões, mas os usuários e grupos existentes não serão afetados. Se houver uma interrupção de qualquer um dos componentes durante a migração, o administrador poderá tentar novamente depois que a interrupção tiver sido resolvida.

### <a name="mim-administration"></a>Administração do MIM

Depois que os usuários e grupos forem migrados, um administrador poderá configurar mais detalhadamente no MIM as atribuições de função, associando os usuários como candidatos para ativação em funções.  Ele também poderá configurar as políticas do MIM para aprovação e o Azure MFA.  

A administração do MIM exige que os componentes do MIM e do AD do ambiente de bastiões estejam online.

### <a name="privileged-role-activation"></a>Ativação de função privilegiada

Quando um usuário deseja ativar uma função privilegiada, ele deve se autenticar no domínio do ambiente de bastiões e enviar uma solicitação ao MIM.  O MIM inclui o SOAP e APIs REST, bem como interfaces do usuário no PowerShell e uma página da Web.

A ativação de função privilegiada exige que os componentes do MIM e do AD do ambiente de bastiões estejam online.  Além disso, se o MIM for configurado para usar o [Azure MFA para ativação](use-azure-mfa-for-activation.md) da função selecionada, será necessário o acesso à Internet para entrar em contato com o serviço do Azure MFA.

### <a name="resource-management"></a>Gerenciamento de recursos

Depois que um usuário for ativado com êxito na função, o controlador de domínio poderá gerar um tíquete do Kerberos para ele, que poderá ser usado pelos controladores de domínio nos domínios existentes e que reconhecerá as novas e temporárias associações a um grupo do usuário.

O gerenciamento de recursos exige que um controlador de domínio do domínio de recursos esteja online, bem como um controlador de domínio no ambiente de bastiões.  Depois que um usuário for ativado, a emissão do tíquete do Kerberos não exigirá que o MIM nem o SQL estejam online no ambiente de bastiões.  (Observe que, com o Windows Server 2012 R2 como o nível funcional para o ambiente de bastiões, o MIM deve estar online para terminar a associação a um grupo temporário.)

### <a name="monitoring-of-users-and-groups-in-the-existing-forest"></a>Monitoramento de usuários e grupos na floresta existente

O MIM também inclui um serviço de monitoramento do PAM que verifica regularmente os usuários e grupos nos domínios existentes, bem como atualiza o banco de dados do MIM e o AD de forma condizente.  Esse serviço não precisa estar online para a ativação de função ou durante o gerenciamento de recursos.

O monitoramento exige que os controladores de domínio da floresta existente estejam online, bem como os componentes do MIM e do AD do ambiente de bastiões.  

## <a name="deployment-options"></a>Opções de implantação

Em [Environment overview](environment-overview.md) (Visão geral do ambiente), é ilustrada uma topologia básica adequada para aprender sobre a tecnologia que não se destina à alta disponibilidade. Esta seção descreve como expandir essa topologia a fim de fornecer alta disponibilidade, para organizações com um único site, bem como para aquelas com vários sites existentes.

### <a name="networking"></a>Rede

O tráfego de rede entre os computadores no ambiente de bastiões deve ser isolado das redes existentes, como por uma rede física ou virtual diferente.  Dependendo dos riscos ao ambiente de bastiões, também pode ser necessário ter interconexões físicas independentes entre os computadores.  Algumas tecnologias de cluster de failover têm requisitos adicionais nas interfaces de rede.

Os computadores que hospedam os Serviços de Domínio do Active Directory e aqueles que hospedam os Serviços MIM no ambiente de bastiões precisam de conectividade bidirecional com os recursos na floresta existente para que:

- os usuários sejam autenticados pelos controladores de domínio da floresta PRIV
- os usuários solicitem a ativação
- os tíquetes do Kerberos dos usuários sejam consumíveis pelos recursos na floresta existente
- o MIM monitore os domínios da floresta existente
- o MIM envie email via servidores de email localizados na floresta existente.

### <a name="minimal-high-availability-topologies"></a>Topologias mínimas de alta disponibilidade

Uma organização pode selecionar quais funções em seu ambiente de bastiões exigem alta disponibilidade, com as seguintes restrições:

- A alta disponibilidade para qualquer função fornecida pelo ambiente de bastiões exige, pelo menos, dois controladores de domínio.  
- A alta disponibilidade para solicitações de ativação exige, pelo menos, dois computadores que hospedam o Serviço MIM e também exige alta disponibilidade para o SQL Server.
- A alta disponibilidade do SQL Server com clusters de failover exige, pelo menos, dois servidores que fornecem o SQL Server, não podendo ser os mesmos usados por um controlador de domínio.
- O Serviço MIM não deve ser instalado no controlador de domínio, a fim de minimizar a superfície de ataque de cada servidor.

A menor topologia de alta disponibilidade para todas as funções em um ambiente de bastiões consiste em, pelo menos, quatro servidores e um armazenamento compartilhado. Dois dos servidores devem ser configurados como controladores de domínio, fornecendo os Serviços de Domínio do Active Directory. Os dois outros servidores podem ser configurados como um cluster de failover com o SQL Server, e fornecem o Serviço MIM.

Além disso, uma implantação típica do ambiente de bastiões também incluiria uma estação de trabalho de administração privilegiada para o gerenciamento desses servidores, bem como um componente de monitoramento

O seguinte diagrama ilustra uma possível arquitetura:

![topologia de bastiões – diagrama](media/bastion1.png)

Pode-se configurar servidores adicionais para cada uma dessas funções, a fim de fornecer um melhor desempenho em condições de carga ou para obter redundância geográfica, conforme descrito abaixo.

### <a name="deployments-supporting-multiple-sites"></a>Implantações que dão suporte a vários sites

A escolha da topologia de implantação certa para os recursos implantados em vários sites depende de três fatores:

- Metas e riscos da alta disponibilidade e da recuperação de desastre
- A capacidade de hardware para hospedar o ambiente de bastiões
- O modelo de trabalho administrativo de cada site.

Uma das abordagens mais simples seria hospedar o ambiente de bastiões em um site específico.  Em condições normais, os usuários se conectariam à implantação do MIM no ambiente de bastiões do site e solicitariam a ativação; as ativações, por sua vez, seriam efetivas em todos os recursos em cada site.  Caso o link de rede seja desfeito ou o site que hospeda o ambiente de bastiões não esteja disponível, as credenciais offline poderão ser acessadas em outro site, a fim de executar a administração temporária até que a rede seja reconectada.  Essa abordagem poderá ser adequada para situações em que há previsão de que a administração local de um site específico, como uma filial, seja rara e limitada à nova conexão desse site ao restante da rede de uma organização.

![Topologia de bastião único para multissite – diagrama](media/bastion2.png)

Para alta disponibilidade e recuperação de desastre em sites, também é possível implantar os componentes do ambiente de bastiões em cada site, compartilhando um diretório PRIV e um banco de dados SQL comuns.  Nessa topologia, caso o link de rede seja desfeito, os usuários de cada site poderão continuar a operar de forma independente.

![Topologia de vários bastiões para multissite – diagrama](media/bastion3.png)

Uma restrição dessa abordagem de implantação é que o SQL Server exige um cluster que abrange ambos os sites, o que pode ser complexo de ser implantado. Nessa situação, considere como uma alternativa somente a replicação do Active Directory (floresta PRIV) do ambiente de bastiões.  Caso haja uma interrupção de rede entre sites, os usuários do site B que já ativaram suas funções privilegiadas poderão continuar a operar para administrar os recursos no site B.

![Topologia de bastião replicado para multissite – diagrama](media/bastion4.png)

Se cada site representar um limite administrativo separado, também será possível implantar vários ambientes de bastiões independentes.  Embora cada ambiente de bastiões tenha o mesmo software, os nomes de domínio de cada um seriam diferentes e não haveria nenhuma semelhança entre os diretórios e os bancos de dados de cada ambiente de bastiões. Um usuário que deseja gerenciar recursos em determinado site ativará uma conta de usuário no ambiente de bastiões desse site.

![Topologia de bastiões independentes para multissite – diagrama](media/bastion5.png)

Por fim, implantações mais complexas são possíveis, já que vários ambientes de bastiões podem ser configurados de modo independente para gerenciar recursos em um domínio específico.

![Topologia de bastião complexo para multissite – diagrama](media/bastion6.png)

### <a name="hosted-bastion-environment"></a>Ambiente de bastiões hospedado

Algumas organizações também consideraram estabelecer o ambiente de bastiões separado de qualquer um de seus sites existentes. O software do ambiente de bastiões pode ser hospedado em uma plataforma de virtualização nas redes da organização ou em um provedor de hospedagem externo.  Ao avaliar essa abordagem, tenha em mente que:

- Para proteger contra ataques provenientes dos domínios existentes, a administração do ambiente de bastiões deve ser isolada das contas administrativas do domínio existente.
- O ambiente de bastiões exige conectividade TCP/IP com os controladores de domínio no domínio existente.  Uma lista de portas pode ser encontrada em [Como configurar um firewall para domínios e relações de confiança](https://support.microsoft.com/kb/179442).
- Uma implantação virtualizada dos Serviços de Domínio do Active Directory exige recursos específicos da plataforma de virtualização, conforme descrito em [Implantação e configuração do controlador de domínio virtualizado](https://technet.microsoft.com/library/jj574223.aspx).
- Uma implantação de alta disponibilidade do SQL Server para o Serviço MIM exige uma configuração de armazenamento especializada, descrita na seção [Armazenamento de banco de dados SQL Server](#sql-server-database-storage) abaixo.  Nem todos os provedores de hospedagem podem oferecer atualmente hospedagem do Windows Server com configurações de disco adequadas para os clusters de failover do SQL Server.

## <a name="deployment-preparation-and-recovery-procedures"></a>Procedimentos de recuperação e preparação de implantação

A preparação de uma implantação pronta para alta disponibilidade ou de recuperação de desastre do ambiente de bastiões exige considerações sobre como instalar o Active Directory do Windows Server, o SQL Server e seu banco de dados no armazenamento compartilhado, bem como o Serviço MIM e seus componentes do PAM.

### <a name="windows-server"></a>Windows Server

O Windows Server contém um recurso interno para alta disponibilidade, permitindo que vários computadores trabalhem juntos como um cluster de failover. Os serviços clusterizados são conectados por cabos físicos e por software. Se um ou mais dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover).   Encontre mais detalhes em [Visão geral do Clustering de Failover](https://technet.microsoft.com/library/hh831579.aspx).

Verifique se o sistema operacional e os aplicativos no ambiente de bastiões recebem atualizações referentes a problemas de segurança. Algumas dessas atualizações podem exigir uma reinicialização do servidor; portanto, coordene os horários em que as atualizações são aplicadas em todos os servidores para evitar interrupções prolongadas. Uma abordagem é usar a [Atualização com Suporte a Cluster](https://technet.microsoft.com/library/hh831694.aspx) para os servidores em um cluster de failover do Windows Server.

Os servidores no ambiente de bastiões serão ingressados em um domínio e dependerão dos serviços de domínio. Verifique se eles não estão inadvertidamente configurados com uma dependência em determinado controlador de domínio para serviços como DNS.

### <a name="bastion-environment-active-directory"></a>Ambiente de bastiões do Active Directory

Os Serviços de Domínio do Active Directory do Windows Server incluem suporte nativo para alta disponibilidade e recuperação de desastre.

#### <a name="preparation"></a>Preparação

Uma implantação de produção típica do gerenciamento de acesso com privilégios inclui, pelo menos, dois controladores de domínio no ambiente de bastiões. Instruções para configurar o primeiro controlador de domínio no ambiente de bastiões são incluídas na etapa 2 dos artigos sobre implantação, [Prepare the PRIV domain controller](step-2-prepare-priv-domain-controller.md) (Preparar o controlador de domínio PRIV).

O procedimento para adicionar um controlador de domínio adicional pode ser encontrado em [Instalar uma réplica de controlador de domínio do Windows Server 2012 em um domínio existente (nível 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Se o controlador de domínio precisar ser hospedado em uma plataforma de virtualização como o Hyper-V, examine as advertências em [Implantação e configuração do controlador de domínio virtualizado](https://technet.microsoft.com/library/jj574223.aspx).

#### <a name="recovery"></a>Recuperação

Após uma interrupção, verifique se, pelo menos, um controlador de domínio está disponível no ambiente de bastiões antes de reiniciar outros servidores.

Em um domínio, o Active Directory distribui as funções FSMO (Flexible Single Master Operation) entre os controladores de domínio, conforme descrito em [How Operations Masters Work](https://technet.microsoft.com/library/cc780487.aspx) (Como funcionam os mestres de operações).  Se ocorrer uma falha em um controlador de domínio, poderá ser necessário transferir uma ou mais das [Funções do Controlador de Domínio](https://technet.microsoft.com/library/cc786438.aspx) atribuídas e esse controlador de domínio.

Depois de determinar que um controlador de domínio não será retornado para a produção, lembre-se de verificar se todas as funções foram atribuídas a esse controlador de domínio e reatribuí-las, conforme necessário. As instruções podem ser encontradas em [View the Current Operations Master Role Holders](https://technet.microsoft.com/library/cc816893.aspx) (Exibir os proprietários de função do mestre de operações atuais) e seus artigos relacionados.

Também é recomendável verificar as configurações de DNS de computadores ingressados em um ambiente de bastiões, bem como os controladores de domínio em domínios CORP que têm uma relação de confiança com esse controlador de domínio, a fim de garantir que nenhum deles tem embutido em código uma dependência do endereço IP do computador desse controlador de domínio.

### <a name="sql-server-database-storage"></a>Armazenamento do banco de dados SQL Server

Uma implantação de alta disponibilidade exige clusters de failover do SQL Server, e as instâncias de cluster de failover do SQL Server responde com base no armazenamento compartilhado entre todos os nós em relação ao armazenamento de log e de banco de dados. O armazenamento compartilhado pode estar na forma de discos de cluster do Clustering de Failover do Windows Server, discos em uma rede SAN (Rede de Área de Armazenamento) ou em compartilhamentos de arquivos em um servidor SMB.  Observe que eles devem ser dedicados ao ambiente de bastiões; o compartilhamento de armazenamento com outras cargas de trabalho fora do ambiente de bastiões não é recomendada, pois pode prejudicar a integridade do ambiente de bastiões.

### <a name="sql-server"></a>SQL Server

O Serviço MIM exige uma implantação do SQL Server no ambiente de bastiões.   Para Alta Disponibilidade, o SQL pode ser implantado com o uso de uma FCI (instância de cluster de failover). Ao contrário de instâncias independentes, em FCIs, a alta disponibilidade do SQL Server é protegida pela presença de nós redundantes na FCI. No caso de falha ou de uma atualização planejada, a propriedade do grupo de recursos é movida para outro nó do Cluster de Failover do Windows Server.

Se você precisar de suporte somente para recuperação de desastre, mas não para alta disponibilidade, o envio de logs, a replicação de transação, a replicação de instantâneo ou o espelhamento de banco de dados poderá ser usado em vez do clustering de failover.   

#### <a name="preparation"></a>Preparação

Quando você instala o SQL Server no ambiente de bastiões, ele não deve depender de nenhum SQL Server já presente nas florestas CORP.  Além disso, é recomendável que o SQL Server seja implantado em um servidor dedicado, diferente do servidor usado para o controlador de domínio.
Mais informações são documentadas no guia do SQL Server referente às [Instâncias de cluster de failover do AlwaysOn](https://msdn.microsoft.com/library/ms189134.aspx).

#### <a name="recovery"></a>Recuperação

Se o SQL Server foi configurado para a recuperação de desastre usando o envio de logs, uma ação deverá ser tomada para atualizar o SQL Server durante a recuperação.  Além disso, será necessário reiniciar cada instância do Serviço MIM.

Caso o SQL Server tenha falhado ou a conectividade entre o SQL Server e o Serviço MIM seja perdida, após a restauração do SQL Server, é recomendável reiniciar cada Serviço MIM.  Isso garantirá que o Serviço MIM restabeleça a conexão com o SQL Server.

### <a name="mim-service"></a>Serviço MIM
O Serviço MIM é necessário para processar as solicitações de ativação.  Para que um computador que hospeda o Serviço MIM possa ser desativado para manutenção enquanto ainda estão sendo recebidas solicitações de ativação, vários computadores do Serviço MIM poderão ser implantados.  Observe que o Serviço MIM não está envolvido em operações do Kerberos após a adição de um usuário a um grupo.  

#### <a name="preparation"></a>Preparação
É recomendável implantar o Serviço MIM em vários servidores ingressados no domínio PRIV.
Para alta disponibilidade, veja os documentos do Windows Server referentes aos [Requisitos de hardware de clustering de failover e opções de armazenamento](https://technet.microsoft.com/library/jj612869.aspx) e [Creating a Windows Server 2012 Failover Cluster](https://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx) (Criando um cluster de failover do Windows Server 2012).

Para a implantação de produção em vários servidores, é possível usar o NLB (Balanceamento de Carga de Rede) para distribuir a carga de processamento.  Você também deve ter um único alias (por exemplo, registros A ou CNAME) para que um nome comum seja exposto ao usuário.

>[!IMPORTANT]
> Se você usar uma tecnologia com balanceamento de carga que não seja o recurso NLB no Windows Server 2012 R2, verifique se sua solução redirecionará uma sessão ao mesmo servidor, e não a um servidor aleatório.

Em uma implantação do MIM com vários servidores, cada Serviço MIM tem um nome do host externo, um nome de serviço e um nome da partição de serviço.  O valor padrão do nome de serviço é o nome do computador e o valor padrão do nome do host externo e o nome da partição de serviço são configurados durante a instalação do Serviço MIM na tela que solicita o endereço do Servidor do Serviço MIM. Esses três nomes são armazenados no arquivo %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config como atributos `externalHostName`, `serviceName` e `servicePartitionName` do nó de configuração `resourceManagementService`.  

Quando um Serviço MIM recebe uma solicitação, o nome da partição de serviço é armazenado como um atributo na solicitação.   Posteriormente, apenas as outras instalações do Serviço MIM que têm o mesmo nome de partição de serviço têm permissão para interagir com essa solicitação.  Como resultado, se o cenário de PAM incluir aprovações manuais ou outro processamento de solicitação de longa vida, garanta que cada Serviço MIM tem o mesmo atributo `servicePartitionName` no arquivo de configuração.

#### <a name="recovery"></a>Recuperação

Após uma interrupção, verifique se, pelo menos, um controlador de domínio do Active Directory e o SQL Server estão disponíveis no ambiente de bastiões antes de reiniciar o Serviço MIM.  

Uma instância de fluxo de trabalho só pode ser concluída por um servidor do Serviço MIM que tem o mesmo nome de partição de serviço e nome do serviço do servidor do Serviço MIM que a iniciou.  Se determinado computador falhar ao hospedar um Serviço MIM que estava processando solicitações e esse computador não for retornado ao serviço, será necessário instalar o Serviço MIM em um novo computador. No novo Serviço MIM após a instalação, edite o arquivo *resourcemanagementservice.exe.config* e defina os atributos `serviceName` e `servicePartitionName` da nova implantação do MIM com o mesmo nome do host e nome da partição de serviço do computador que falhou.

### <a name="mim-pam-components"></a>Componentes do PAM no MIM

O instalador do Portal e do Serviço MIM também incorpora componentes adicionais do PAM, incluindo módulos do PowerShell e dois serviços.

#### <a name="preparation"></a>Preparação

Os componentes do Privileged Access Management devem ser instalados em cada computador no ambiente de bastiões no qual o Serviço MIM está sendo instalado.  Eles não podem ser adicionados posteriormente.

#### <a name="recovery"></a>Recuperação

Após a recuperação de uma interrupção, verifique se o Serviço MIM está em execução em, pelo menos, um servidor.  Em seguida, garanta que o serviço de monitoramento do PAM no MIM também está em execução no servidor, usando `net start "PAM Monitoring service"`.

Se o nível funcional da floresta do ambiente de bastiões for o Windows Server 2012 R2, verifique se o serviço de componente do PAM no MIM também está em execução no servidor, usando o comando `net start "PAM Component service"`.
