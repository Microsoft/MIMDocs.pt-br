---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "Avalie o processo de criação de usuários no AD DS usando o Microsoft Identity Manager 2016"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 60b28497f6abba14bd186cf2e2f2ce69b08693bc
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Como Provisionar Usuários no AD DS

Aplica-se a: Microsoft Identity Manager 2016 SP1 (MIM)

Um requisito básico para um sistema de gerenciamento de identidade é a capacidade de provisionar recursos para um sistema externo.

Este guia explica os principais blocos de construção envolvidos no processo de provisionamento de usuários do Microsoft® Identity Manager (MIM) 2016 nos Active Directory® Domain Services (AD DS), descreve como verificar se o seu cenário funciona como esperado, oferece sugestões para o gerenciamento de usuários do Active Directory usando o MIM 2016 e lista mais fontes para obter informações.

## <a name="before-you-begin"></a>Antes de começar


Nesta seção, você encontrará informações sobre o escopo deste documento. Em geral, os guias “Como...” são destinados a leitores que já têm experiência básica com o processo de sincronização de objetos com o MIM, conforme abordado nos [Guias de Introdução](http://go.microsoft.com/FWLink/p/?LinkId=190486) relacionados.

### <a name="audience"></a>Público-alvo


Este guia destina-se a profissionais de tecnologia da informação (TI) que já tem um conhecimento básico de como funciona o processo de sincronização do MIM e têm interesse em obter experiência prática e mais informações conceituais sobre cenários específicos.

### <a name="prerequisite-knowledge"></a>Conhecimento de pré-requisito


Este documento presume que você tem acesso a uma instância de execução do MIM e experiência na configuração de cenários de sincronização simples, conforme descrito nos seguintes documentos:

-   [Introdução à Sincronização de Entrada](http://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Introdução à Sincronização de Saída](http://go.microsoft.com/FWLink/p/?LinkId=189653)

O conteúdo deste documento foi concebido para funcionar como uma extensão desses documentos introdutórios.

### <a name="scope"></a>Escopo


O cenário descrito neste documento foi simplificado para atender aos requisitos de um ambiente de laboratório básico. O foco é auxiliar na compreensão dos conceitos e tecnologias abordados.

Este documento o ajudará a desenvolver uma solução que envolve o gerenciamento de grupos no AD DS usando o MIM.

### <a name="time-requirements"></a>Requisitos de tempo


Os procedimentos deste documento levam de 90 a 120 minutos para serem concluídos.

Essas estimativas pressupõem que o ambiente de testes já está configurado e não inclui o tempo necessário para configurá-lo.

### <a name="getting-support"></a>Como obter suporte


Se houver dúvidas sobre o conteúdo deste documento ou se você tiver comentários gerais que gostaria de discutir, fique à vontade para postar uma mensagem no [Fórum do Forefront Identity Manager 2010](http://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Descrição do cenário


A Fabrikam, uma empresa fictícia, está planejando usar o MIM para gerenciar contas de usuário de AD DS da empresa. Como parte desse processo, a Fabrikam precisa provisionar usuários no AD DS. Para começar o teste inicial, a Fabrikam instalou um ambiente de laboratório básico, composto pelo MIM e pelo AD DS.
Nesse ambiente de laboratório, a Fabrikam está testando um cenário em que um usuário foi criado manualmente no Portal do MIM. O objetivo desse cenário é provisionar o usuário como um usuário habilitado com uma senha predefinida no AD DS.

## <a name="scenario-design"></a>Design do Cenário


Para usar este guia, três componentes de arquitetura são necessários:

-   Controlador de domínio do Active Directory

-   Um computador que execute o Serviço de Sincronização FIM
-   Um computador que execute o Portal do FIM

A ilustração a seguir descreve o ambiente necessário.

![Ambiente necessário](media/how-provision-users-adds/image001.png)


É possível executar todos os componentes em um computador.

>[!NOTE]
Para obter mais informações sobre a configuração do MIM, consulte o [Guia de Instalação do FIM](http://go.microsoft.com/FWLink/p/?LinkId=165845).

## <a name="scenario-components-list"></a>Lista de Componentes do Cenário


A tabela a seguir lista os componentes que fazem parte do cenário deste guia.

| ![Unidade Organizacional](media/how-provision-users-adds/image005.jpg)   | Unidade organizacional                | Objetos MIM – Unidade organizacional (OU) usada como destino para usuários provisionados.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Contas de usuário](media/how-provision-users-adds/image006.jpg)   | Contas de usuário                      | &#183; **ADMA** – Conta de usuário do Active Directory com direitos suficientes para se conectar ao AD DS.<br/> &#183; **FIMMA** – Conta de usuário do Active Directory com direitos suficientes para se conectar ao MIM.
                                                                 |
| ![Agentes de gerenciamento e perfis de execução](media/how-provision-users-adds/image007.jpg)  | Agentes de gerenciamento e perfis de execução | &#183; **ADMA da Fabrikam** – Agente de gerenciamento que troca dados com o AD DS. <br/> &#183; FIMMA da Fabrikam – Agente de gerenciamento que troca dados com o MIM.                                                                                 |
| ![Regras de sincronização](media/how-provision-users-adds/image008.jpg)  | Regras de sincronização              | Regra de Sincronização de Saída de Grupo da Fabrikam – Regra de sincronização de saída que provisiona usuários no AD DS.                                     |
| ![Define](media/how-provision-users-adds/image009.jpg)   | Define                               | Todos os Prestadores de Serviço – Conjunto com associação dinâmica para todos os objetos com um valor de atributo EmployeeType do Prestador de Serviço.                                |
| ![Fluxos de Trabalho](media/how-provision-users-adds/image010.jpg)  | Fluxos de Trabalho                          | Fluxo de Trabalho de Provisionamento do AD – Fluxo de trabalho para colocar o usuário do MIM dentro do escopo da Regra de Sincronização de Saída do AD.                                |
| ![Regras de política de gerenciamento](media/how-provision-users-adds/image011.jpg)   | Regras de política de gerenciamento            | Regra de Política de Gerenciamento de Provisionamento do AD – Regra de política de gerenciamento (MPR) disparada quando um recurso se torna membro do conjunto Todos os Prestadores de Serviço. |
| ![Usuários do MIM](media/how-provision-users-adds/image012.jpg) | Usuários do MIM                          | Britta Simon – Usuária do MIM provisionada no AD DS.                                                                                             |



## <a name="scenario-steps"></a>Etapas do cenário


O cenário descrito neste guia é composto pelos blocos de construção mostrados na figura a seguir.

![Etapas do cenário](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Configuração dos Sistemas Externos


Nesta seção, encontram-se instruções sobre os recursos que você precisa criar que estão fora do ambiente do MIM.

### <a name="step-1-create-the-ou"></a>Etapa 1: Criar a OU


A OU serve como um contêiner para o usuário de exemplo provisionado. Para obter mais informações sobre a criação de OUs, consulte [Criar uma Nova Unidade Organizacional](http://go.microsoft.com/FWLink/p/?LinkId=189655).

Crie uma OU chamada MIMObjects no AD DS.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Etapa 2: Criar as contas de usuário do Active Directory

Para o cenário deste guia, duas contas de usuário do Active Directory são necessárias:

- **ADMA** – Utilizada pelo agente de gerenciamento do Active Directory.

- **FIMMA** – Utilizada pelo agente de gerenciamento do Serviço FIM.

Em ambos os casos, isso é suficiente para criar contas de usuário normais. Mais informações sobre os requisitos específicos de ambas as contas serão fornecidas mais adiante neste documento. Para obter mais informações sobre a criação de usuários, consulte [Criar uma Nova Conta de Usuário](http://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Configuração do Serviço de Sincronização FIM


É necessário iniciar o Synchronization Service Manager FIM para seguir as etapas de configuração desta seção.

### <a name="creating-the-management-agents"></a>Criar os agentes de gerenciamento

Para o cenário deste guia, será necessário criar dois agentes de gerenciamento:

-   **ADMA da Fabrikam** – Agente de gerenciamento para o AD DS.

-   **FIMMA da Fabrikam** – Agente de gerenciamento para o agente de gerenciamento do Serviço FIM.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Etapa 3: Criar o agente de gerenciamento ADMA da Fabrikam

Ao configurar um agente de gerenciamento do AD DS, é necessário especificar uma conta usada pelo agente de gerenciamento na troca de dados com o AD DS. Use uma conta de usuário normal. No entanto, para importar dados do AD DS, a conta deve ter o direito de sondar alterações do controle DirSync. Se você quiser que o agente de gerenciamento exporte dados para o AD DS, será necessário conceder direitos suficientes à conta nas OUs de destino. Para obter mais informações sobre esse tópico, consulte [Configuração da Conta ADMA](http://go.microsoft.com/FWLink/p/?LinkId=189657).

Para criar um usuário no AD DS, é necessário realizar um fluxo de saída do DN do objeto. Além disso, é uma boa prática realizar o fluxo do nome, sobrenome e nome de exibição para garantir que os objetos sejam detectáveis.

No AD DS, ainda é comum que os usuários utilizem o atributo sAMAccountName para fazer logon no serviço de diretório. Se um valor não for especificado para esse atributo, o serviço de diretório gerará um valor aleatório para ele. No entanto, esses valores aleatórios não são amigáveis, por isso, uma versão amigável desse atributo normalmente é parte de uma exportação para o AD DS. Para habilitar um usuário a fazer logon no AD DS, também é necessário incluir uma senha criada por meio do atributo unicodePwd na lógica de exportação.

>[!Note]                                
Verifique se o valor especificado como unicodePwd está em conformidade com as políticas de senha do AD DS de destino.

Ao definir uma senha para contas do AD DS, também é necessário criar uma conta como conta habilitada. Isso pode ser feito por meio da configuração do atributo userAccountControl. Para obter mais informações sobre o atributo userAccountControl, consulte [Usando o FIM para Habilitar ou Desabilitar Contas no Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658).

A tabela a seguir lista as configurações específicas do cenário mais importantes que precisam ser configuradas.

| Página de designer do agente de gerenciamento                          | Configuração                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Criar um agente de gerenciamento                                 | 1. **Agente de gerenciamento para:** AD DS  <br/> 2.  **Nome:** ADMA da Fabrikam |
| Conectar à floresta do Active Directory                      | 1. **Selecionar as partições de diretório:** “DC=Fabrikam,DC=com”   <br/>   2. Clique em **Contêineres** para abrir a caixa de diálogo **Selecionar Contêineres** e verifique se **MIMObjects** é a única OU selecionada.        |
| Selecionar tipos de objeto                                     | Além dos tipos de objeto já selecionados, selecione **usuário.** |
| Selecionar atributos                                       | 1. Clique em **Mostrar Tudo.** <br/>   2. Selecione os seguintes atributos: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Para obter mais informações, consulte os seguintes tópicos em Ajuda:
- Criar um Agente de Gerenciamento
- Conectar à Floresta do Active Directory
- Utilizar o Agente de Gerenciamento do Active Directory
- Configurar partições de diretório

>[!Note]
Verifique se há uma regra de fluxo de atributo de importação foi configurada para o atributo ExpectedRulesList.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Etapa 4: Criar o agente de gerenciamento FIMMA da Fabrikam

Ao configurar um agente de gerenciamento do Serviço FIM, é necessário especificar uma conta usada pelo agente de gerenciamento na troca de dados com o Serviço FIM.

Use uma conta de usuário normal. A conta deve ser a mesma especificada durante a instalação do MIM. Consulte Utilizando o Windows PowerShell para Realizar um [Teste Rápido da Configuração de Conta do FIM MA](http://go.microsoft.com/FWLink/p/?LinkId=189659) para um script que pode ser usado para determinar o nome da conta FIMMA especificada durante a configuração e para testar se a conta ainda é válida.

A tabela a seguir lista as configurações específicas do cenário mais importantes que precisam ser configuradas. Crie o agente de gerenciamento com base nas informações fornecidas na tabela a seguir.  

| Página de designer do agente de gerenciamento | Configuração |
|------------|------------------------------------|
| Criar um agente de gerenciamento | 1. **Agente de gerenciamento para:** Agente de Gerenciamento do Serviço FIM <br/> 2. **Nome** FIMMA da Fabrikam |
| Conectar ao banco de dados     | Use as seguintes configurações: <br/> &#183; **Servidor:** localhost <br/> &#183; **Banco de dados:** FIMService <br/> &#183; **Endereço básico do Serviço FIM:** http://localhost:5725 <br/> <br/> Forneça as informações sobre a conta criada para esse agente de gerenciamento |
| Selecionar tipos de objeto                                     | Além dos tipos de objeto já selecionados, selecione **Pessoa.**   |
| Configurar mapeamentos de tipo de objeto                          | Além dos mapeamentos de tipo de objeto já existentes, adicione um mapeamento do **Tipo de Objeto da Fonte de Dados** Pessoa à pessoa de Tipo de Objeto **Metaverso**. |
| Configurar fluxo de atributo                                | Além dos mapeamentos de fluxo de atributo já existentes, adicione os seguintes mapeamentos de fluxo de atributo: <br/><br/> ![Fluxo de atributos](media/how-provision-users-adds/image018.jpg) |




Para obter mais informações, consulte os tópicos a seguir na Ajuda:
-   Criar um Agente de Gerenciamento

-   Conectar a um Banco de Dados do Active Directory

-   Utilizar o Agente de Gerenciamento do Active Directory

-   Configurar partições de diretório

>[!NOTE]
 Verifique se há uma regra de fluxo de atributo de importação foi configurada para o atributo ExpectedRulesList.

### <a name="step-5-create-the-run-profiles"></a>Etapa 5: Criar perfis de execução

A tabela a seguir lista os perfis de execução necessários para criar o cenário deste guia.

| Agente de gerenciamento  | Perfil de execução     |
|-------------------|--------------------------------------|
| ADMA da Fabrikam     | 1. Importação completa  <br/> 2. Sincronização completa <br/> 3. Importação de delta <br/> 4. Sincronização delta <br/> 5. Exportar                                                                    |
| FIMMA da Fabrikam   | 1. Importação completa <br> 2. Sincronização completa <br/> 3. Importação de delta <br/> 4. Sincronização delta <br/> 5. Exportar|                                                                                                                                                                                   

Crie perfis de execução para cada agente de gerenciamento de acordo com a tabela anterior.


>[!Note]
Para obter mais informações, consulte Criar um Perfil de Execução de Agente de Gerenciamento na Ajuda do MIM.                                                                                                                  


>[!Important]
 Verifique se o provisionamento está habilitado no ambiente. Isso pode ser feito executando o script, Utilizando o Windows PowerShell para Habilitar o Provisionamento (http://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>Configuração do Serviço FIM


Para o cenário deste guia, é necessário configurar uma política de provisionamento, conforme mostrado na figura a seguir.

![Política de provisionamento](media/how-provision-users-adds/image019.png)

O objetivo desta política de provisionamento é colocar grupos dentro do escopo da Regra de Sincronização de Saída de Usuário do AD. Ao colocar o recurso no escopo da regra de sincronização, permite-se que o mecanismo de sincronização provisione o recurso para o AD DS de acordo com a configuração.

Para configurar o Serviço FIM, acesse http://localhost/identitymanagement no Internet Explorer®. Na página do Portal do MIM, para criar a política de provisionamento, acesse as páginas relacionadas na seção Administração. Para verificar a configuração, execute o script [Utilizando o Windows PowerShell para Documentar a Configuração de Política de Provisionamento](http://go.microsoft.com/FWLink/p/?LinkId=189661).

### <a name="step-6-create-the-synchronization-rule"></a>Etapa 6: Criar a regra de sincronização

As tabelas a seguir mostram a configuração da regra de sincronização de provisionamento necessária da Fabrikam. Crie a regra de sincronização de acordo com os dados nas tabelas a seguir.

| Configuração da regra de sincronização                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Nome                                                                                                       | Regra de Sincronização de Saída de Usuário do Active Directory                         |                                                          
| Descrição                                                                                               |                                                                             |                                                           
| Precedência                                                                                                | 2                                                                           |                                                           
| Direção do Fluxo de Dados   | Saída             |       
| Dependência       |         |                                         


| Escopo |                                                                             |                                                           
|--------|-------|
| Tipo de Recurso do Metaverso | pessoa |                                                         
| Sistema Externo                   |ADMA da Fabrikam                                                               |                                                       
| Tipo de Recurso do Sistema Externo                                                                              | usuário      



| Relationship ||
|------------|---------|
| Criar Recurso no Sistema Externo                                                                         | verdadeiro                                                                        |                                                           
| Habilitar o Desprovisionamento                                                                                      | Falso                                                                       |                                                           

| Critérios de relacionamento                                                                                      | |
|------------|----------|
| Atributo ILM     | Atributo de fonte de dados                                                       |
| Atributo de fonte de dados         | sAMAccountName    |

| Fluxos iniciais do atributo de saída        | |                                                             |
|-------------------|---------------------- |---------------|
| Permitir nulos                 | Destination                                                                 | Origem                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **Constante:** 512                                         |
| false                                                                     | unicodePwd                    | Constante: P\@\$\$W0rd                                    |

| Fluxos persistentes do atributo de saída  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Permitir nulos                                                                                                | Destination                                                                 | Origem                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



 >[!NOTE]
 Importante Verifique se Somente Fluxo Inicial foi selecionado para o fluxo de atributo que tem o DN como destino.                                                                          

### <a name="step-7-create-the-workflow"></a>Etapa 7: Criar o fluxo de trabalho

O objetivo do Fluxo de Trabalho de Provisionamento do AD é adicionar a regra de sincronização de provisionamento da Fabrikam a um recurso. As tabelas a seguir mostram a configuração.  Crie um fluxo de trabalho de acordo com os dados nas tabelas abaixo.

| Configuração do fluxo de trabalho               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nome                                 | Fluxo de Trabalho de Provisionamento de Usuários do Active Directory                     |
| Descrição                          |                                                                 |
| Tipo de Fluxo de Trabalho                        | Ação                                                          |
| Executar na Atualização da Política                 | Falso                                                           |

| Regra de sincronização                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nome                                 | Regra de Sincronização de Saída de Usuário do Active Directory             |
| Ação                               | Adicionar                                                             |




### <a name="step-8-create-the-mpr"></a>Etapa 8: Criar a MPR

A MPR necessária é do tipo Definir Transição e dispara quando um recurso se torna membro de um conjunto Todos os Prestadores de Serviço. As tabelas a seguir mostram a configuração.  Crie uma MPR de acordo com os dados nas tabelas abaixo.

| Configuração da MPR                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Nome                                 | Regra de Política de Gerenciamento de Provisionamento de Usuários do AD                 |
| Descrição                          |                                                             |
| Tipo                                 | Definir Transição                                              |
| Concede Permissões                   | Falso                                                       |
| Desativado                             | Falso                                                       |

| Definição da transição                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo de transição                      | Transição de Entrada                                               |
| Conjunto de Transição                       | Todos os Prestadores de Serviço                                             |

| Fluxos de trabalho de política                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo                                 | Ação                                                      |
| Nome para Exibição                         | Fluxo de Trabalho de Provisionamento de Usuários do Active Directory                 |




## <a name="initializing-your-environment"></a>Inicializando o Ambiente


Os objetivos da fase de inicialização são os seguintes:

-   Aplique a regra de sincronização ao metaverso.

-   Coloque a estrutura do Active Directory no espaço conector do Active Directory.

### <a name="step-9-run-the-run-profiles"></a>Etapa 9: Executar os perfis de execução

A tabela a seguir lista os perfis de execução que fazem parte da fase de inicialização.  Execute os perfis de execução de acordo com a tabela a seguir.

| Executar                                                                                                           | Agente de gerenciamento                                      | Perfil de execução          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | FIMMA da Fabrikam                                        | Importação completa          |
| 2                                                                                                             |                                                       | Sincronização completa |
| 3                                                                                                             |                                                       | Exportar               |
| 4                                                                                                             |                                                       | Importação de delta         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | ADMA da Fabrikam                                         | Importação completa          |
| 6                                                                                                             |                                                       | Sincronização completa |



>[!NOTE]
Verifique se a regra de sincronização de saída foi projetada com êxito no metaverso.

## <a name="testing-the-configuration"></a>Testar a Configuração


O objetivo desta seção é testar a configuração real. Para testar a configuração:

1.  Crie um usuário de exemplo no Portal do FIM.

2.  Verifique os requisitos de provisionamento do usuário de exemplo.

3.  Provisione o usuário de exemplo no AD DS.

4.  Verifique se o usuário existe no AD DS.

### <a name="step-10-create-a-sample-user-in-mim"></a>Etapa 10: Criar um usuário de exemplo no MIM


A tabela a seguir lista as propriedades do usuário de exemplo. Crie um usuário de exemplo de acordo com os dados na tabela a seguir.

| Atributo                              | Valor                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Nome                             | Britta                                                         |
| Sobrenome                              | Simon                                                          |
| Nome para Exibição                           | Britta Simon                                                   |
| Nome da Conta                           | BSimon                                                         |
| Domain                                 | Fabrikam                                                       |
| Tipo de Funcionário                          | Prestador de Serviços                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Verifique os requisitos de provisionamento do usuário de exemplo


Para provisionar o usuário de exemplo no AD DS, dois pré-requisitos devem ser cumpridos:

1.  O usuário deve ser um membro do conjunto Todos os Prestadores de Serviços.

2.  O usuário do conjunto deve estar dentro do escopo da regra de sincronização de saída.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Etapa 11: Verificar se que o usuário é membro de Todos os Prestadores de Serviços

Para verificar se o usuário é membro do conjunto Todos os Prestadores de Serviço, abra o conjunto e, em seguida, clique em Exibir Membros.

![Verifique se o usuário é membro de Todos os Prestadores de Serviço](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Etapa 12: Verificar se o usuário está no escopo da regra de sincronização de saída

Para verificar se o usuário está no escopo da regra de sincronização, abra a página de propriedades do usuário e examine o atributo Lista de Regras Esperadas na guia Provisionamento. O atributo Lista de Regras Esperadas deve listar o Usuário do AD

Regra de Sincronização de Saída. A captura de tela a seguir mostra um exemplo do atributo Lista de Regras Esperadas.

![Status da regra de sincronização](media/how-provision-users-adds/image023.jpg)

Neste ponto do processo, o Status da Regra de Sincronização está Pendente. Isso significa que a regra de sincronização ainda não foi aplicada ao usuário.



### <a name="step-13-synchronize-the-sample-group"></a>Etapa 13: Sincronizar o grupo de exemplo


Antes de iniciar o primeiro ciclo de sincronização de um objeto de teste, acompanhe o estado esperado do objeto depois da execução de cada perfil de execução em um plano de teste. O plano de teste deve incluir, ao lado do estado geral do objeto (criado, atualizado ou excluído), os valores de atributo esperados.
Use o plano de teste para verificar suas expectativas de plano de teste. Se uma etapa não retornar os resultados esperados, não vá para a próxima etapa até que a discrepância entre o resultado esperado e o resultado real seja resolvida.

Para verificar as expectativas, é possível usar as estatísticas de sincronização como um primeiro indicador. Por exemplo, se você espera que os novos objetos sejam preparados em um espaço conector, mas as estatísticas de importação não retornarem nenhum “Adds”, obviamente há algo em seu ambiente que não funciona conforme o esperado.

![Estatísticas de sincronização](media/how-provision-users-adds/image024.jpg)

Embora as estatísticas de sincronização possam oferecer uma primeira indicação de que o cenário funciona como o esperado, use o Espaço Conector de Pesquisa e o recurso de Pesquisa de Metaverso do Synchronization Service Manager para verificar os valores de atributo esperados.

Sincronizar o usuário no AD DS:

1.  Importe o usuário para o espaço conector do FIM MA.

2.  Projete o usuário no metaverso.

3.  Provisione o usuário no espaço conector do Active Directory.

4.  Exporte informações de status para o FIM.

5.  Exporte o usuário para o AD DS.

6.  Confirme a criação do usuário.

Para realizar essas tarefas, execute os seguintes perfis de execução.

| Agente de gerenciamento | Perfil de execução  |
|------------------|--------------|
| FIMMA da Fabrikam   | 1. Importação de delta <br/> 2. Sincronização Delta <br/> 3. Exportar <br/> 4. Importação de delta |
| FIMMA da Fabrikam   | 1. Exportar <br/> 2. Importação de delta       |


Após a importação do banco de dados do Serviço FIM, Britta Simon e o objeto ExpectedRuleEntry que vincula Britta à

Regra de Sincronização de Saída de Usuários do AD serão testados no espaço conector FIMMA da Fabrikam. Ao examinar as

propriedades de Britta no espaço conector, ao lado de valores de atributo configurados no Portal do FIM, você também encontrará uma referência válida ao objeto Entrada da Regra Esperada. A captura de tela a seguir mostra um exemplo.

![Propriedades de objeto do espaço conector](media/how-provision-users-adds/image025.jpg)

O objetivo da sincronização delta executada no FIMMA da Fabrikam é realizar várias operações:

-   Projeção – O novo objeto de usuário e o objeto Entrada da Regra Esperada relacionado serão projetados no metaverso.

-   Provisionamento – O objeto Britta Simon projetado recentemente será projetado no espaço conector do ADMA da Fabrikam.

-   Fluxos de Atributo de Exportação – Fluxos de atributo de exportação ocorrerão em ambos os agentes de gerenciamento. No ADMA da Fabrikam, o objeto Britta Simon projetado recentemente será preenchido com novos valores de atributo. No FIMMA da Fabrikam, o objeto Britta Simon existente e o objeto ExpectedRuleEntry relacionado serão atualizados com valores de atributo que são resultados da projeção.

![Estatísticas de sincronização](media/how-provision-users-adds/image026.jpg)

Como já indicado pelas estatísticas de sincronização, uma atividade de provisionamento ocorreu no espaço conector do ADMA da Fabrikam. Ao examinar as propriedades do objeto de metaverso de Britta Simon, você descobrirá que essa atividade é um resultado do atributo ExpectedRulesList preenchido com uma referência válida.

![propriedades do objeto de metaverso](media/how-provision-users-adds/image027.jpg)

Durante a exportação no FIMMA da Fabrikam a seguir, o status da regra de sincronização de Britta Simon será atualizado de Pendente para Aplicada, o que indica que a regra de sincronização de saída está ativa no objeto do metaverso.

![Regra de sincronização aplicada](media/how-provision-users-adds/image028.jpg)

Como um novo objeto foi provisionado no espaço conector do ADMA, deve haver um Adicionar exportação pendente nesse agente de gerenciamento. Usando um script para essa finalidade, é possível ver um Adicionar exportação pendente relatado para o ADMA da Fabrikam. Para usar o script, consulte [Utilizando o Windows PowerShell para Exibir o Estado de Exportação de um Agente de Gerenciamento](http://go.microsoft.com/FWLink/p/?LinkId=189664).

![Exportações pendentes do agente de gerenciamento](media/how-provision-users-adds/image029.jpg)

No FIM, cada execução de exportação requer uma importação delta para concluir a operação de exportação. A importação delta executada após uma execução de exportação é chamada de confirmação de importação. A confirmação de importações é necessária para habilitar o Serviço de Sincronização do FIM a realizar os requisitos de atualização apropriados em execuções de sincronização sucessivas.


Execute os perfis de execução de acordo com as instruções nesta seção.

>[!IMPORTANT]
Cada execução de perfil de execução deve ter êxito, sem erros.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Etapa 14: Verificar o usuário provisionado no AD DS

Para verificar se o usuário de exemplo foi provisionado no AD DS, abra a OU do FIMObjects. Britta Simon deve estar localizada na OU do FIMObjects.

![verificar se o usuário está na OU do FIMObjects](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>Resumo
=======

O objetivo deste documento é apresentar os principais blocos de construção para a sincronização de um usuário no MIM com o AD DS. No teste inicial, inicie com o mínimo de atributos necessários para concluir uma tarefa e adicione mais atributos ao cenário quando as etapas gerais funcionarem conforme o esperado. Quanto menor a complexidade, mais simples será o processo de solução de problemas.

Ao testar a configuração, é muito provável que você exclua e crie novamente novos objetos de teste. Para objetos com um

atributo ExpectedRulesList preenchido, isso pode ter como resultado objetos ERE órfãos.
Para obter uma descrição de como remover esses objetos do ambiente de teste, consulte [Um Método para Remover Objetos ExpectedRuleEntry Órfãos do Ambiente](http://go.microsoft.com/FWLink/p/?LinkId=189667).

Em um cenário normal de sincronização que inclui o AD DS como destino de sincronização, o MIM não será autoritativo para todos os atributos de um objeto. Por exemplo, ao gerenciar objetos de usuário no AD DS usando o FIM, no mínimo, o domínio e os atributos objectSID precisam ser contribuídos pelo agente de gerenciamento do AD DS.
Os atributos de nome, domínio e objectSID da conta serão necessários se sua intenção for habilitar um usuário para fazer logon no Portal do FIM. Para preencher esses atributos do AD DS, uma regra de sincronização de entrada adicional será necessária para o espaço conector do AD DS. Ao gerenciar objetos com várias fontes de valores de atributo, é necessário garantir que a prioridade de fluxo de atributo está configurada corretamente. Se a prioridade de fluxo de atributo não estiver configurada corretamente, o mecanismo de sincronização bloqueará o preenchimento dos valores de atributo. É possível saber mais sobre a prioridade de fluxo de atributo no artigo [Sobre a Prioridade de Fluxo de Atributo](http://go.microsoft.com/FWLink/p/?LinkId=189675).

<a name="see-also"></a>Consulte também
=========

<a name="other-resources"></a>Outros Recursos
---------------

[Utilizando o FIM para Habilitar ou Desabilitar Contas no Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189670)

[Sobre Atributos de Referência](http://go.microsoft.com/FWLink/p/?LinkId=189671)

[Como Gerenciar Minha Conta FIM MA](http://go.microsoft.com/FWLink/p/?LinkId=189672)

[Detectando Contas Não Autoritativas – Parte 1: Previsão](http://go.microsoft.com/FWLink/p/?LinkId=189673)

[A Versão Pobre do Mecanismo de Detecção de Conector](http://go.microsoft.com/FWLink/p/?LinkId=189674)

[Configurando a Conta ADMA](http://go.microsoft.com/FWLink/p/?LinkId=189657)

[Um Método para Remover Objetos ExpectedRuleEntry Órfãos do Ambiente](http://go.microsoft.com/FWLink/p/?LinkId=189667)

[Sobre a Prioridade de Fluxo de Atributo](http://go.microsoft.com/FWLink/p/?LinkId=189675)

[Sobre Exportações](http://go.microsoft.com/FWLink/p/?LinkId=189676)
