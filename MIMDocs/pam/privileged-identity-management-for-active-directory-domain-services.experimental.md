---
title: "O que é o PAM para ADDS? | Microsoft Docs"
description: "O PAM (Privileged Access Management) ajuda as organizações a restringir o acesso privilegiado em um ambiente existente do Active Directory."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: f0947f186b5206d06a67140706ada33a5bc0e016
ms.openlocfilehash: 9a047644d07e3ee3c2d1abfde7753849b5ddc63b
ms.contentlocale: pt-br
ms.lasthandoff: 01/11/2017

---

<a id="privileged-access-management-for-active-directory-domain-services" class="xliff"></a>
# Privileged Access Management para Serviços de Domínio do Active Directory
O PAM (Privileged Access Management) ajuda as organizações a restringir o acesso privilegiado em um ambiente existente do Active Directory.

![Etapas do PAM: preparar, proteger, operar e monitorar – diagrama](media/MIM_PIM_SetupProcess.png)

Concentrando-se em um ciclo de preparação, proteção e monitoramento seu ambiente, o Privileged Access Management atinge dois objetivos:

- Restabelecer o controle sobre um ambiente do Active Directory comprometido, mantendo um ambiente de bastiões separado, conhecido por não ser afetado por ataques mal-intencionados.  
- Isolar o uso de contas privilegiadas para reduzir o risco de roubo dessas credenciais.

> [!NOTE]
> O PAM é uma instância do [PIM](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (Privileged Identity Management) implementada com o MIM (Microsoft Identity Manager).

<a id="what-problems-does-pam-help-solve" class="xliff"></a>
## Quais problemas o PAM ajuda a solucionar?
Uma preocupação real das empresas hoje é o acesso aos recursos em um ambiente do Active Directory. Particularmente preocupantes são as notícias sobre vulnerabilidades, escalonamentos de privilégio não autorizado e outros tipos de acesso não autorizado, incluindo passagem de hash, passagem de tíquete, lança phishing e compromissos do Kerberos.

Hoje, é muito fácil para os invasores obter as credenciais de conta de Admins do Domínio e é muito difícil descobrir esses ataques após o fato ter ocorrido. A meta do PAM é reduzir as oportunidades de que usuários mal-intencionados obtenham acesso, aumentando seu controle e sua percepção do ambiente.

O PAM dificulta a entrada de invasores à rede e seu acesso à conta privilegiada. O PAM adiciona proteção a grupos privilegiados que controlam o acesso a uma variedade de computadores ingressados em domínio e aplicativos nesses computadores. Ele também adiciona mais monitoramento, mais visibilidade e controles mais refinados para que as organizações possam ver quem são seus administradores privilegiados e o que eles estão fazendo. O PAM fornece às organizações um conhecimento mais aprofundado sobre como essas contas administrativas são usadas no ambiente.

<a id="how-is-pam-set-up" class="xliff"></a>
## Como o PAM é configurado?
O PAM se baseia no princípio da administração Just-In-Time, que está relacionada ao [JEA (Just Enough Administration)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA é um kit de ferramentas do Windows PowerShell define um conjunto de comandos para executar atividades privilegiadas e um ponto de extremidade em que os administradores podem obter autorização para executar esses comandos. No JEA, um administrador decide que usuários com determinado privilégio podem executar determinada tarefa. Sempre que um usuário qualificado precisa executar essa tarefa, o administrador habilita essa permissão. As permissões expiram após um período especificado, para que um usuário mal-intencionado não possa roubar o acesso.

A instalação e operação do PAM têm quatro etapas.


1.  **Preparar**: Identifique quais grupos em sua floresta existente tem privilégios significativos. Recrie esses grupos sem membros na floresta de bastiões.

2.  **Proteger**: configure a proteção do ciclo de vida e da autenticação, como o MFA (Multi-Factor Authentication), para quando os usuários solicitarem a administração Just-In-Time. A MFA ajuda a impedir ataques programáticos de software mal-intencionado ou a seguir roubo de credenciais.

3.  **Operar**: depois que os requisitos de autenticação forem atendidos e uma solicitação for aprovada, uma conta de usuário será adicionada temporariamente a um grupo privilegiado na floresta de bastiões. Durante um intervalo de tempo predefinido, o administrador tem todos os privilégios e permissões de acesso atribuídos a esse grupo. Após esse tempo, a conta é removida do grupo.

4.  **Monitorar**: o PAM adiciona auditoria, alertas e relatórios de solicitações de acesso privilegiado. É possível examinar o histórico do acesso privilegiado e ver quem executou uma atividade. Você pode entender se a atividade é válida ou não e identificar facilmente atividades não autorizadas, como uma tentativa de adicionar um usuário diretamente a um grupo privilegiado na floresta original. Essa etapa é importante não somente para identificar o software mal-intencionado, mas também para acompanhamento de invasores “internos”.

<a id="how-does-pam-work" class="xliff"></a>
## Como funciona o PAM?
O PAM se baseia nas novas funcionalidades do AD DS, particularmente, em relação à autorização e autenticação de contas de domínio, e nas novas funcionalidades do Microsoft Identity Manager. O PAM separa as contas privilegiadas de um ambiente existente do Active Directory. Quando uma conta privilegiada precisa ser usada, ela precisa primeiro ser solicitada e, em seguida, aprovada. Após a aprovação, a conta privilegiada recebe a permissão por meio de um grupo principal externo em uma nova floresta bastiões, em vez da floresta atual do usuário ou aplicativo. O uso de uma floresta bastiões dá controle maior à organização, como o controle sobre quando um usuário pode ser um membro de um grupo com privilégios e sobre como o usuário precisa se autenticar.

O Active Directory, o serviço MIM e outras partes da solução também podem ser implantadas em uma configuração de alta disponibilidade.

O exemplo a seguir mostra como o PIM funciona em mais detalhes.

![Processo do PIM e participantes – diagrama](media/MIM_PIM_howitworks.png)

A floresta de bastiões emite associações a um grupo com limite de tempo, que por sua vez, produzem TGTs (tíquetes de concessão de tíquete) com limite de tempo. Os serviços ou aplicativos baseados no Kerberos poderão respeitar e impor esses TGTs se os aplicativos e serviços existirem nas florestas que confiam na floresta de bastiões.

Contas de usuário diárias não precisam ser movidas para uma nova floresta. O mesmo acontece com os computadores, aplicativos e seus grupos. Eles permanecem onde eles estão atualmente, em uma floresta existente. Considere o exemplo de uma organização que trata esses problemas de segurança cibernética hoje, mas não tem planos imediatos para atualizar a infraestrutura de servidor para a próxima versão do Windows Server. Essa organização ainda poderá tirar proveito dessa solução combinada, usando MIM e uma nova floresta de bastiões; ela poderá também controlar melhor o acesso aos recursos existentes.

O PAM oferece as seguintes vantagens:

-   **Isolamento/controle de privilégios**: os usuários não mantêm privilégios em contas que também são usadas para tarefas não privilegiadas, como verificação de email ou navegação na Internet. Os usuários precisam solicitar privilégios. As solicitações são aprovadas ou negadas com base nas políticas do MIM definidas por um administrador do PAM. Até uma solicitação ser aprovada, o acesso privilegiado não estará disponível.

-   **Step-up e proof-up**: esses são os novos desafios de autenticação e autorização para ajudar a gerenciar o ciclo de vida de contas administrativas separadas. O usuário pode solicitar a elevação de uma conta administrativa e essa solicitação passa por fluxos de trabalho MIM.

-   **Log adicional**: além de fluxos de trabalho internos do MIM, há um log adicional para o PAM que identifica a solicitação, como ela foi autorizada e os eventos que ocorrem após a aprovação.

-   **Fluxo de trabalho personalizável**: Os fluxos de trabalho MIM podem ser configurados para cenários diferentes e vários fluxos de trabalho podem ser usados, com base nos parâmetros do usuário solicitante ou nas funções solicitadas.

<a id="how-do-users-request-privileged-access" class="xliff"></a>
## Como os usuários solicitam o acesso privilegiado?
Há várias maneiras pelas quais um usuário pode enviar uma solicitação, incluindo:  
- A API de Serviços Web dos Serviços MIM  
- Um ponto de extremidade REST  
- Windows PowerShell (`New-PAMRequest`)

<a id="what-workflows-and-monitoring-options-are-available" class="xliff"></a>
## Quais opções de fluxos de trabalho e monitoramento estão disponíveis?
Por exemplo, digamos que um usuário era um membro de um grupo administrativo antes do PIM ser instalado. Como parte da instalação do PIM, o usuário será removido do grupo administrativo e uma política será criada em MIM. A política especifica que, se esse usuário solicitar privilégios administrativos e é autenticado pelo MFA, a solicitação será aprovada e uma conta separada para o usuário será adicionada ao grupo privilegiado na floresta de bastiões.

Supondo que a solicitação foi aprovada, o fluxo de trabalho de ação se comunica diretamente com a floresta de bastiões do Active Directory para colocar um usuário em um grupo. Por exemplo, quando Adriana faz uma solicitação para administrar o banco de dados de RH, a conta administrativa para Adriana é adicionada ao grupo privilegiado na floresta de bastiões em questão de segundos. A associação de sua conta administrativa nesse grupo expirará após um limite de tempo. Com o Windows Server Technical Preview, essa associação é associada no Active Directory com um limite de tempo; com o Windows Server 2012 R2 na floresta de bastiões, o limite de tempo é imposto por MIM.

> [!NOTE]
> Quando você adiciona um novo membro a um grupo, a alteração deve ser replicada para outros controladores de domínio na floresta de bastiões. A latência da replicação pode afetar a capacidade dos usuários de acessar recursos. Para obter mais informações sobre a latência de replicação, veja [How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx) (Como funciona a topologia de replicação do Active Directory).
>
> Por outro lado, um link expirado é avaliado em tempo real pelo SAM (Gerente de Contas de Segurança). Embora a adição de um membro do grupo precise ser replicada pelo controlador de domínio que recebe a solicitação de acesso, a remoção de um membro do grupo é avaliada instantaneamente em qualquer controlador de domínio.

Esse fluxo de trabalho destina-se especificamente a essas contas administrativas. Administradores (ou até mesmo scripts) que precisam de acesso ocasional apenas para grupos privilegiados podem solicitar exatamente tal acesso. O MIM registra a solicitação e as alterações no Active Directory, e você pode exibi-las no Visualizador de Eventos ou enviar os dados para soluções de monitoramento corporativo como o ACS (Serviços de Coleta de Auditoria) do System Center 2012 – Operations Manager ou outras ferramentas de terceiros.

