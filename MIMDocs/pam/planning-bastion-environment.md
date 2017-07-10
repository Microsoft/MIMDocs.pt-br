---
title: "Planejando um ambiente de bastiões | Microsoft Docs"
description: 
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/16/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b459906f0c8d2c631e9b63813e208c9098ea5a4e
ms.contentlocale: pt-br
ms.lasthandoff: 07/10/2017


---

<a id="planning-a-bastion-environment" class="xliff"></a>
# Planejando um ambiente de bastiões

A adição de um ambiente de bastiões com uma floresta administrativa dedicada a um Active Directory permite que as organizações gerenciem facilmente contas administrativas, estações de trabalho e grupos em um ambiente que tem controles de segurança mais fortes que seu ambiente de produção existente.

Essa arquitetura permite uma série de controles que não são possíveis nem facilmente configurados em uma arquitetura de floresta única. Isso inclui o provisionamento de contas como usuários padrão não privilegiados na floresta administrativa que são altamente privilegiados no ambiente de produção, possibilitando uma maior imposição técnica de governança. Essa arquitetura também permite o uso do recurso de autenticação seletiva de uma relação de confiança como um meio de restringir logons (e a exposição de credenciais) apenas para hosts autorizados. Em situações em que se deseje um nível maior de segurança para a floresta de produção sem incorrer em custos e complexidade de uma recompilação completa, uma floresta administrativa pode fornecer um ambiente que aumente o nível de garantia do ambiente de produção.

Além da floresta administrativa dedicada, pode-se usar técnicas adicionais. Isso inclui restringir o local em que as credenciais administrativas são expostas, limitar privilégios de função de usuários nessa floresta e garantir que as tarefas administrativas não sejam realizadas em hosts usados para atividades de usuário padrão (por exemplo, email e navegação na Web).

<a id="best-practice-considerations" class="xliff"></a>
## Considerações sobre melhores práticas

Uma floresta administrativa dedicada é uma floresta do Active Directory de domínio único padrão usada para o gerenciamento do Active Directory. Uma vantagem do uso de florestas e domínios administrativos é que eles podem ter mais medidas de segurança quando comparado às florestas de produção, devido a seus casos de uso limitados. Além disso, como essa floresta é separada e não confia nas florestas existentes da organização, o comprometimento de segurança em outra floresta não se estenderá para essa floresta dedicada.

Um design de floresta administrativa apresenta as seguintes considerações:

<a id="limited-scope" class="xliff"></a>
### Escopo limitado

O valor de uma floresta de administrador é o alto nível de garantia de segurança e a superfície de ataque reduzida. A floresta pode hospedar aplicativos e funções de gerenciamento adicionais, mas cada aumento no escopo aumentará a superfície de ataque da floresta e de seus recursos. O objetivo é limitar as funções da floresta para manter a superfície de ataque mínima.

De acordo com o [Modelo de camada](tier-model-for-partitioning-administrative-privileges.md) de particionamento de privilégios administrativos, as contas em uma floresta administrativa dedicada devem estar em uma única camada, normalmente, camada 0 ou 1. Se uma floresta estiver na camada 1, considere restringi-la em um escopo específico de aplicativo (por exemplo, aplicativos de finanças) ou a comunidade de usuários (por exemplo, fornecedores de TI terceirizados).

<a id="restricted-trust" class="xliff"></a>
### Relação de confiança restrita

A floresta *CORP* de produção deve confiar na floresta *PRIV* administrativa, mas não o oposto. Isso pode ser uma relação de confiança de domínio ou uma relação de confiança de floresta. O domínio da floresta de administrador não precisa confiar nos domínios e florestas gerenciadas para gerenciar o Active Directory, embora outros aplicativos possam exigir uma relação de confiança bidirecional, validação de segurança e testes.

A autenticação seletiva deve ser usada para garantir que as contas na floresta de administrador usam apenas os hosts de produção apropriados. Para manter os controladores de domínio e direitos de delegação no Active Directory, isso geralmente exige a concessão do direito “Autorizado a fazer logon” de controladores de domínio a contas de administrador da Camada 0 designadas na floresta de administrador. Veja [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc816580.aspx)para obter mais informações.

<a id="maintain-logical-separation" class="xliff"></a>
## Mantendo uma separação lógica

Para garantir que o ambiente de bastiões não é afetado por incidentes de segurança atuais ou futuros no Active Directory organizacional, as seguintes diretrizes devem ser usadas durante a preparação de sistemas para o ambiente de bastiões:

- Os Windows Servers não devem ser ingressados no domínio ou utilizar a distribuição de software ou de configurações do ambiente existente.

- O ambiente de bastiões deve conter seus próprios Serviços de Domínio do Active Directory, fornecendo o Kerberos, LDAP, DNS e serviços de hora em hora ao ambiente de bastiões.

- O MIM não deve usar um farm de bancos de dados SQL no ambiente existente. O SQL Server deve ser implantado em servidores dedicados no ambiente de bastiões.

- O ambiente de bastiões exige o Microsoft Identity Manager 2016, especificamente, os componentes do PAM e o Serviço MIM devem ser implantados.

- O software de backup e mídia do ambiente de bastiões deve ser mantido separado do software e mídia dos sistemas nas florestas existentes, para que um administrador na floresta existente não possa subverter um backup do ambiente de bastiões.

- Os usuários que gerenciam os servidores do ambiente de bastiões deverão fazer logon em estações de trabalho que não são acessíveis aos administradores no ambiente existente, para que não haja perda das credenciais do ambiente de bastiões.

<a id="ensure-availability-of-administration-services" class="xliff"></a>
## Garantir a disponibilidade dos serviços de administração

Como a administração de aplicativos será transferida para o ambiente de bastiões, leve em consideração como fornecer disponibilidade suficiente para atender aos requisitos dos aplicativos. As técnicas incluem:

- Implante Serviços de Domínio do Active Directory em vários computadores no ambiente de bastiões. Pelo menos dois são necessários para garantir a autenticação contínua, mesmo que um servidor seja reiniciado temporariamente para a manutenção agendada. Podem ser necessários computadores adicionais para uma carga maior ou para gerenciar recursos e administradores localizados em várias regiões geográficas.

- Prepare contas de vigilância na floresta existente e na floresta de administrador dedicada, para fins de emergência.

- Implante o SQL Server e o Serviço do MIM em vários computadores no ambiente de bastiões.

- Mantenha uma cópia de backup do AD e do SQL de cada alteração em usuários ou definições de função na floresta de administrador dedicada.

<a id="configure-appropriate-active-directory-permissions" class="xliff"></a>
## Configurar permissões apropriadas do Active Directory

A floresta administrativa deve ser configurada para o privilégio mínimo com base nos requisitos de administração do Active Directory.

- Contas da floresta de administrador que são usadas para administrar o ambiente de produção não devem receber privilégios administrativos para a floresta de administração nem para domínios ou estações de trabalho nela.

- Privilégios administrativos sobre a floresta de administrador propriamente dita devem ser controlados rigorosamente por um processo offline, a fim de reduzir a oportunidade de um invasor ou funcionário mal-intencionado em posse de informações privilegiadas apagar os logs de auditoria. Isso também ajuda a garantir que a equipe em posse das contas de administrador de produção não reduza as restrições sobre suas contas e aumente o risco para a organização.

- A floresta administrativa deve seguir as configurações do SCM (Microsoft Security Compliance Manager) para o domínio, inclusive configurações fortes para protocolos de autenticação.

Ao criar o ambiente de bastiões, antes de instalar o Microsoft Identity Manager, identifique e crie as contas que serão usadas para administração nesse ambiente. Isso incluirá:

- **Contas de vigilância** só devem poder fazer logon nos controladores de domínio do ambiente de bastiões.

- Os **administradores “Cartão Vermelho”** provisionam outras contas e realizam a manutenção não agendada. Nenhum acesso a sistemas ou florestas existentes fora do ambiente de bastiões é fornecido a essas contas. As credenciais, por exemplo, um cartão inteligente, devem ser fisicamente protegidas, e o uso dessas contas deve ser registrado.

- As **contas de serviço** necessárias no Microsoft Identity Manager, no SQL Server e em outros softwares.

<a id="harden-the-hosts" class="xliff"></a>
## Proteger os hosts

Todos os hosts, incluindo controladores de domínio, servidores e estações de trabalho ingressados na floresta administrativa, devem ter os sistemas operacionais mais recentes e service packs instalados e atualizados.

- Os aplicativos necessários para realizar a administração devem ser pré-instalados em estações de trabalho, para que as contas que as usam não precisem estar no grupo de administradores locais para as instalarem. Normalmente, a manutenção do Controlador de Domínio pode ser realizada com o RDP e as Ferramentas de Administração de Servidor Remoto.

- Os hosts da floresta de administrador devem ser atualizados automaticamente com atualizações de segurança. Embora isso possa criar um risco de interromper as operações de manutenção do controlador de domínio, isso fornece uma redução significativa dos riscos de segurança de vulnerabilidades sem patch.

<a id="identify-administrative-hosts" class="xliff"></a>
### Identificar hosts administrativos

O risco de um sistema ou estação de trabalho deve ser medido pela atividade de risco mais alta que é executada nele ou nela, como navegação pela Internet, envio e recebimento de email ou uso de outros aplicativos que processam conteúdo desconhecido ou não confiável.

Os hosts administrativos incluem os seguintes computadores:

- Uma área de trabalho na qual as credenciais do administrador são fisicamente digitadas ou inseridas.

- “Servidores de atalho” administrativos nos quais são executadas sessões e ferramentas administrativas.

- Todos os hosts nos quais são realizadas ações administrativas, incluindo aquelas que usam uma área de trabalho de usuário padrão que executa um cliente RDP para administrar servidores e aplicativos remotamente.

- Servidores que hospedam aplicativos que precisam ser administrados e que não são acessados usando o RDP com o Modo de Administrador Restrito ou a comunicação remota do Windows PowerShell.

<a id="deploy-dedicated-administrative-workstations" class="xliff"></a>
### Implantar estações de trabalho administrativas dedicadas

Embora sejam inconvenientes, estações de trabalho protegidas separadas, dedicadas a usuários com credenciais administrativas de alto impacto podem ser necessárias. É importante fornecer um host com um nível de segurança igual ou maior que o nível dos privilégios confiados às credenciais. Considere incorporar as seguintes medidas para obter proteção adicional:

- **Verificar todas as mídias no build como limpas** para reduzir a instalação de malware em uma imagem mestra ou sua injeção em um arquivo de instalação durante o download ou armazenamento.

- **Linhas de base de segurança** devem ser usadas como configurações iniciais. O Microsoft SCM (Security Compliance Manager) pode ajudar a configurar as linhas de base em hosts administrativos.

- **Inicialização segura** para reduzir tentativas de ataques ou de malware de carregar um código não assinado no processo de inicialização.

- **Restrição de software** para garantir que apenas o software administrativo autorizado seja executado nos hosts administrativos. Os clientes podem usar o AppLocker para essa tarefa com uma lista aprovada de aplicativos autorizados, para ajudar a impedir que softwares mal-intencionados e aplicativos sem suporte sejam executados.

- **Criptografia de volume completo** para reduzir a perda física de computadores, como laptops administrativos usados remotamente.

- **Restrições de USB** para proteger contra a infecção física.

- **Isolamento de rede** para proteger contra ataques de rede e ações acidentais do administrador. Os firewalls de host devem bloquear todas as conexões de entrada, exceto aquelas explicitamente necessárias, e bloquear todo o acesso à Internet de saída desnecessário.

- **Antimalware** para proteger contra ameaças conhecidas e malware.

- **Reduções de explorações** para atenuar as ameaças e explorações desconhecidas, incluindo o EMET (Enhanced Mitigation Experience Toolkit).

- **Análise da superfície de ataque** para evitar a introdução de novos vetores de ataque no Windows durante a instalação de novo software. Ferramentas como o ASA (Analisador de Superfície de Ataque) ajudam a avaliar as definições de configuração em um host e a identificar os vetores de ataque introduzidos por software ou alterações na configuração.

- **Privilégios administrativos** não devem ser concedidos a usuários em seus computadores locais.

- **Modo RestrictedAdmin** para as sessões RDP de saída, exceto quando exigido pela função. Veja [Novidades sobre os Serviços de Área de Trabalho Remota no Windows Server](https://technet.microsoft.com/library/dn283323.aspx) para obter mais informações.

Embora algumas dessas medidas possam parecer extremas, as divulgações públicas nos últimos anos exemplificaram os recursos significativos que os adversários capacitados têm para comprometer as metas.

<a id="prepare-existing-domains-to-be-managed-by-the-bastion-environment" class="xliff"></a>
## Preparar os domínios existentes a serem gerenciados pelo ambiente de bastiões

O MIM usa cmdlets do PowerShell para estabelecer a relação de confiança entre os domínios existentes do AD e a floresta administrativa dedicada no ambiente de bastiões. Depois de implantar o ambiente de bastiões e antes que os usuários ou grupos sejam convertidos em JIT, os cmdlets `New-PAMTrust` e `New-PAMDomainConfiguration` atualizarão as relações de confiança de domínio e criarão os artefatos necessários para o AD e MIM.

Quando a topologia existente do Active Directory é alterada, os cmdlets `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` e `Remove-PAMDomainConfiguration` podem ser usados para atualizar as relações de confiança.

<a id="establish-trust-for-each-forest" class="xliff"></a>
## Estabelecer relação de confiança para cada floresta

O cmdlet `New-PAMTrust` deve ser executado uma única vez para cada floresta existente. Ele é invocado no computador do Serviço do MIM no domínio administrativo. Os parâmetros para esse comando são o nome de domínio do domínio superior da floresta existente e as credenciais de um administrador desse domínio.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Depois de estabelecer a relação de confiança, configure cada domínio para habilitar o gerenciamento do ambiente de bastiões, como descrito na próxima seção.

<a id="enable-management-of-each-domain" class="xliff"></a>
## Habilitar o gerenciamento de cada domínio

Há sete requisitos para habilitar o gerenciamento de um domínio existente.

<a id="1-a-security-group-on-the-local-domain" class="xliff"></a>
### 1. Um grupo de segurança no domínio local

Deve haver um grupo no domínio existente cujo nome é o nome NetBIOS do domínio seguido de três cifrões, por exemplo, *CONTOSO$$$*. O escopo do grupo deve ser *domínio local* e o tipo de grupo deve ser *Segurança*. Isso será necessário para a criação dos grupos na floresta administrativa dedicada com o mesmo Identificador de segurança dos grupos neste domínio. Crie esse grupo com o seguinte comando do PowerShell, executado por um administrador do domínio existente e em uma estação de trabalho ingressada no domínio existente:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

<a id="2-success-and-failure-auditing" class="xliff"></a>
### 2. Auditoria de êxito e falha

As configurações de política de grupo no controlador de domínio para a auditoria devem incluir a auditoria de êxito e de falha do gerenciamento de contas de auditoria e do acesso do serviço de diretório da auditoria. Isso pode ser feito com o console de Gerenciamento de Política de Grupo, executado por um administrador do domínio existente e em uma estação de trabalho ingressada no domínio existente:

3. Vá para **Iniciar** > **Ferramentas Administrativas** > **Gerenciamento de Política de Grupo**.

4. Navegue até **Floresta: contoso.local** > **Domínios** > **contoso.local** > **Controladores de Domínio** > **Política de Controladores de Domínio Padrão**. Será exibida uma mensagem informativa.

    ![Política de controladores de domínio padrão – captura de tela](media/pam-group-policy-management.jpg)

5. Clique com o botão direito do mouse em **Política de Controladores de Domínio Padrão** e selecione **Editar**. Uma nova janela será exibida.

6. Na janela do Editor de Gerenciamento de Política de Grupo, na árvore Política de Controladores de Domínio Padrão, navegue até **Configuração do Computador** > **Políticas** > **Configurações do Windows** > **Configurações de Segurança** > **Políticas Locais** > **Política de Auditoria**.

    ![Editor de gerenciamento de política de grupo – captura de tela](media/pam-group-policy-management-editor.jpg)

5. No painel de detalhes, clique com o botão direito do mouse em **Gerenciamento de conta de auditoria** e selecione **Propriedades** no menu. Selecione **Definir estas configurações de política**, marque a caixa de seleção **Êxito**, marque a caixa de seleção **Falha**, clique em **Aplicar** e em **OK**.

6. No painel de detalhes, clique com o botão direito do mouse em **Acesso ao serviço de diretório de auditoria** e selecione **Propriedades**. Selecione **Definir estas configurações de política**, marque a caixa de seleção **Êxito**, marque a caixa de seleção **Falha**, clique em **Aplicar** e em **OK**.

    ![Configurações de política de êxito e falha – captura de tela](media/pam-group-policy-management-editor2.jpg)

7. Feche a janela do Editor de Gerenciamento de Política de Grupo e a janela Gerenciamento de Política de Grupo. Então aplique as configurações de auditoria abrindo uma janela do PowerShell e digitando:

    ```
    gpupdate /force /target:computere
    ```

A mensagem “A atualização da Política de Computador foi concluída com sucesso." deve aparecer após alguns minutos.

<a id="3-allow-connections-to-the-local-security-authority" class="xliff"></a>
### 3. Permitir conexões com a Autoridade de Segurança Local

Os controladores de domínio devem permitir RPC em conexões TCP/IP para LSA (Autoridade de Segurança Local) no ambiente de bastiões. Em versões mais antigas do Windows Server, o suporte a TCP/IP no LSA deve ser habilitado no Registro:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

<a id="4-create-the-pam-domain-configuration" class="xliff"></a>
### 4. Criar a configuração de domínio do PAM

O cmdlet `New-PAMDomainConfiguration` deve ser executado no computador do Serviço do MIM no domínio administrativo. Os parâmetros para esse comando são o nome de domínio do domínio existente e as credenciais de um administrador desse domínio.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

<a id="5-give-read-permissions-to-accounts" class="xliff"></a>
### 5. Conceder permissões de leitura de contas

As contas na floresta de bastiões usadas para estabelecer funções (administradores que usam os cmdlets `New-PAMUser` e `New-PAMGroup` ), bem como a conta usada pelo serviço do monitor do MIM, precisam ter permissões de leitura naquele domínio.

As etapas a seguir permitem o acesso de leitura para o usuário *PRIV\Administrator* no domínio *Contoso* dentro do controlador de domínio *CORPDC*:

1. Certifique-se de que você está conectado ao CORPDC como um administrador de domínio do Contoso (como Contoso\Administrator).

2. Inicie Usuários e Computadores do Active Directory.

3. Clique com o botão direito do mouse no domínio **contoso.local** e selecione **Delegar Controle**.

4. Na guia Usuários e grupos selecionados, clique em **Adicionar**.

5. No pop-up Selecionar Usuários, Computadores ou Grupos, clique em **Locais** e altere o local para *priv.contoso.local*. No nome do objeto, digite *Administradores de Domínio* e clique em **Verificar Nomes**. Quando um pop-up for exibido, para o nome de usuário, digite *priv\administrator* e a senha.

6. Após Administradores de Domínio, digite *; MIMMonitor*. Depois que os nomes Administradores de Domínio e MIMMonitor forem sublinhados, clique em **OK** e em **Avançar**.

7. Na lista de tarefas comuns, selecione **Ler todas as informações do usuário**, clique em **Avançar** e em **Concluir**.

18. Feche Usuários e Computadores do Active Directory.

<a id="6-a-break-glass-account" class="xliff"></a>
### 6. Uma conta de vigilância

Se a meta do projeto de gerenciamento de acesso privilegiado é reduzir o número de contas com privilégios de Administrador de Domínio atribuídos permanentemente ao domínio, deve haver uma conta de *vigilância* no domínio, caso haja um problema posteriormente com a relação de confiança. Deve-se ter contas de acesso de emergência à floresta de produção em cada domínio e elas só devem ter a capacidade de fazer logon em controladores de domínio. Para organizações com vários sites, contas adicionais podem ser necessárias para redundância.

<a id="7-update-permissions-in-the-bastion-environment" class="xliff"></a>
### 7. Permissões de atualização no ambiente de bastiões

Examine as permissões para o objeto *AdminSDHolder* no contêiner Sistema desse domínio. O objeto *AdminSDHolder* tem uma ACL (lista de controle de acesso) exclusiva, que é usada para controlar as permissões de entidades de segurança que são membros de grupos do Active Directory com privilégios internos. Observe que, se forem feitas alterações às permissões padrão que afetariam usuários com privilégios administrativos no domínio, desde que essas permissões não se apliquem a usuários cujas contas estejam no ambiente de bastiões.

<a id="select-users-and-groups-for-inclusion" class="xliff"></a>
## Selecionar usuários e grupos para inclusão

A próxima etapa é definir as funções do PAM, associando os usuários e grupos aos quais eles devem ter acesso. Isso geralmente será um subconjunto dos usuários e grupos da camada identificada como sendo gerenciada no ambiente de bastiões. Encontre mais informações em [Defining roles for Privileged Access Management](defining-roles-for-pam.md) (Definindo funções para o Privileged Access Management).

