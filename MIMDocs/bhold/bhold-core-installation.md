---
title: "Instalação do BHOLD Core | Microsoft Docs"
description: "Documento principal de instalação do BHOLD Suite"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 33fbe63528d5d7c543ae286f934654538782b4d5
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2017
---
# <a name="bhold-core-installation"></a>Instalação do BHOLD Core

O módulo BHOLD Core fornece os principais recursos do BHOLD Suite dentro de seu ambiente. O módulo BHOLD Core deve ser instalado e configurado em um servidor em sua rede local antes que você possa instalar outros módulos BHOLD Suite.

## <a name="bhold-core-installation-requirements"></a>Requisitos de instalação do BHOLD Core

O módulo BHOLD Core constitui a base do Microsoft BHOLD Suite. Você deve instalar o módulo BHOLD Core antes de instalar outros módulos do Microsoft BHOLD Suite.

### <a name="bhold-core-hardware-requirements"></a>Requisitos de hardware do BHOLD Core

O módulo BHOLD Core constitui a base do Microsoft BHOLD Suite. Você deve instalar o módulo BHOLD Core antes de instalar outros módulos do Microsoft BHOLD Suite.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Mínimo** | **Recomendado** |
|Processador | Processador de 64 bits | Processador de 64 bits com vários núcleos |
| Memória |3 GB | 6 GB ou mais |
|Armazenamento| 30 GB disponíveis |Depende do tamanho da implantação |
|Adaptador de rede| Conexão de 100 Mbps ao servidor SQL e ao FIM (Forefront Identity Manager) | Conexão de 1 Gbps ao SQL e ao servidor FIM|

Essas recomendações se baseiam em implementações comuns e não levam em consideração outros aplicativos em execução no servidor. Talvez você precise usar componentes de maior desempenho dependendo do seu ambiente específico.

### <a name="bhold-core-software-requirements"></a>Requisitos de software do BHOLD Core

O módulo BHOLD Core deve ser instalado em um computador que atenda aos seguintes requisitos:

- O servidor deve estar executando o Windows Server 2012 (64 bits) ou Windows Server 2016 
- O servidor deve ser um membro de um domínio do AD DS (Active Directory Domain Services). Em ambientes de teste, o servidor pode ser um controlador de domínio do AD DS.
- O BHOLD Core deve ser instalado por um usuário conectado com uma conta no mesmo domínio que o servidor e que pertence ao grupo de Administradores de Domínio no domínio e ao grupo de Administradores no servidor.
- Os IIS (Serviços de informações da Internet) da Microsoft com o ASP.NET deve ser instalado no servidor. O IIS deve ser configurado com a Autenticação do Windows habilitada. Se o BHOLD Core está sendo instalado no Windows Server 2012/2016, a Compatibilidade de Gerenciamento do IIS 6 deve estar instalada. Se o BHOLD Core está sendo instalado no Windows Server 2012, as Ferramentas de script do IIS 6 devem estar instaladas
- O .NET Framework 3.5 deve estar instalado.
  - Já que o BHOLD Core requer o .NET 3.5, ele não pode ser instalado no Server Core.
- O Silverlight 4 é necessário para outros módulos BHOLD e, portanto, é recomendável instalá-lo antes de instalar o BHOLD Core.
- O Microsoft SQL Server 2014 ou Microsoft SQL Server 2016 deve estar instalado no servidor do BHOLD Core ou então em outro servidor na rede local. 

Computadores cliente do Windows devem estar executando o Microsoft Internet Explorer versão 6 ou posterior e o Microsoft Silverlight versão 4 ou posterior.

## <a name="before-you-begin"></a>Antes de começar

O módulo BHOLD Core requer uma conta de usuário que é usada para autenticar e autorizar o serviço BHOLD Core para o servidor e outras entidades de rede. Esta seção explica como criar e configurar essa conta de usuário e fornece uma planilha de pré-instalação que ajudará você a coletar as informações necessárias para concluir a instalação do BHOLD Core.

>{!IMPORTANTE} quando você instala o BHOLD Core, você pode usar um banco de dados do SQL Server existente como banco de dados do BHOLD Core ou então você pode permitir que o Assistente de instalação do BHOLD Core crie o banco de dados do BHOLD Core para você. Se você optar por usar um banco de dados existente, certifique-se de que nenhum outro usuário possa acessar o banco de dados enquanto você estiver instalando o BHOLD Core. Antes de instalar o BHOLD Core, verifique se os controles de acesso do banco de dados do SQL Server não permitem o acesso por qualquer pessoa que não seja o usuário que está instalando o BHOLD Core.

## <a name="required-user-and-group"></a>Usuário e grupo requeridos

O módulo BHOLD Core deve ser capaz de fazer logon no domínio com uma conta de usuário que é dedicada para esse propósito e que é um membro de dois grupos de segurança específicos, incluindo um que é criado especificamente para o módulo BHOLD Core. A associação ao grupo Administradores do Domínio é necessária para realizar esse procedimento.
**Para criar e configurar o grupo de segurança e o usuário do BHOLD Core**

1.  Em um controlador de domínio, clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Usuários e Computadores do Active Directory**.

2.  Na árvore de console, expanda o domínio em que a conta será criada, clique com o botão direito do mouse em **Usuários**, aponte para **Novo**e, em seguida, clique em **Grupo**.

3.  Na caixa de diálogo **Novo Objeto – Grupo**, em **Nome do grupo**, digite o nome do grupo (padrão do BHOLD: BHOLDApplicationGroup) e, em seguida, clique em **OK**.

4.  Clique com o botão direito do mouse em **Usuários**, aponte para **Novo**e clique em **Usuário**.

5.  Em **Nome completo**, digite um nome que ajudará você a identificar a conta, por exemplo, Conta de Serviço do BHOLD Core.

6.  Em **Nome de logon do usuário**, digite o nome de usuário da conta de serviço do BHOLD Core (padrão do BHOLD: b1user) e, em seguida, clique em **Avançar**.

7.  Em **Senha** e **Confirmar senha**, digite a senha da conta de serviço.

8.  Limpe **O usuário deve alterar a senha no próximo logon**, selecione **O usuário não pode alterar a senha** e **A senha nunca expira**, clique em **Avançar** e depois clique em **Concluir**.

9.  No painel de resultados do console, clique com o botão direito do mouse na conta de usuário e clique em **Adicionar a um grupo**.

10. Na caixa de diálogo **Selecionar Grupos**, digite o nome de exibição do grupo que você criou anteriormente, digite um ponto e vírgula (;) e, em seguida, digite IIS_IUSRS.

11. Clique em **Verificar Nomes** e depois em **OK**.  

O procedimento a seguir deve ser realizado no computador em que o módulo BHOLD Core deve ser instalado. Você deve fazer logon como membro do grupo Administradores do Domínio para executar este procedimento.

## <a name="bhold-core-installation-worksheet"></a>Planilha de instalação do módulo BHOLD Core

Antes de você começar a instalar o módulo BHOLD Core, você precisa estar preparado para fornecer as informações que o assistente de Configuração do BHOLD Core requer para concluir a instalação. A planilha a seguir ajudará você a registrar essas informações, de modo que você estará pronto para fornecê-las quando necessário. Cada seção corresponde a uma página no Assistente de instalação do BHOLD Core.

### <a name="account-settings"></a>Configurações de conta

| **Item**                                    | **Descrição**                                                                                                                                                                                                                                                                                             | **Valor**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar um provedor de segurança em um Domínio/Computador** | Quando selecionado, especifica que a segurança do Active Directory Domain Services controlará o acesso ao BHOLD Core.                                                                                                                                                                                                  | Marque a caixa de seleção. **Importante:** a instalação falhará se essa caixa de seleção não estiver selecionada.                                                                 |
| **Domínio**                                  | Especifica o domínio que contém o grupo de aplicativos, a conta de serviço e o servidor do BHOLD. **Importante:** especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio é fabrikam.com, especifique o nome de domínio como CONTOSO. | Grave o nome de domínio aqui:                                                                                                                                        |
| **Grupo de aplicativos**                       | Especifica o nome do grupo de segurança que você criou anteriormente em [Usuário e grupo necessários](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Grave o nome do grupo aqui:                                                                                                                                         |
| **Usuário de serviço**                            | Especifica o nome de logon da conta de usuário de serviço que você criou anteriormente em [Usuário e grupo necessários](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Grave o nome da conta de usuário aqui:                                                                                                                                  |
| **Senha**                                | Especifica a senha da conta de usuário do serviço do BHOLD Core.                                                                                                                                                                                                                                              | Grave a senha aqui: **Importante:** certifique-se de manter essa senha em um local seguro, oculto.                                                                |
| **IP/Porta do Site**                         | Especifica o endereço IP e número da porta do site a ser criado no servidor da intranet. Altere o valor padrão (\*) somente se você não usará o mesmo endereço IP que o site padrão. Altere o número da porta para uma porta disponível apenas se a porta padrão (5151) já estiver em uso.             | Se um endereço IP não padrão for usado pelo site da Web padrão, grave-o aqui: se o número da porta padrão já estiver em uso, escreva o número da porta do site do BHOLD aqui: |

### <a name="database-settings"></a>Configurações do banco de dados

| **Item**                                       | **Descrição**                                                                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar segurança integrada**                    | Especifica que a Autenticação do Windows é usada para acessar o banco de dados.                                                                                                                                                                                                     | Marque a caixa de seleção se a autenticação do Windows for usada para se conectar ao SQL Server. Desmarque a caixa de seleção se a autenticação do SQL Server for usada. O banco de dados deverá ter sido criado antes de executar a Instalação do BHOLD Core se a Autenticação do SQL Server for usada. **Observação:** se a Autenticação do Windows for usada, você deverá fazer logon com uma conta que tenha a função de servidor sysadmin no servidor de banco de dados. |
| **Usuário de Banco de Dados** e **Senha de Banco de Dados** | Especifica o nome de usuário e a senha de um usuário com a função de servidor sysadmin no servidor de banco de dados. Esses valores são fornecidos somente quando a Autenticação do SQL Server é usada.                                                                                               | Grave o nome de usuário do SQL Server aqui: Grave a senha do usuário do SQL Server aqui: **Observação:** certifique-se de manter essa senha em um local seguro, oculto.                                                                                                                                                                                                                                                  |
| **Servidor de Banco de Dados** e **Nome do Banco de Dados**   | Especifica o nome NetBIOS do servidor de banco de dados e o nome do banco de dados (padrão: b1) que a Instalação do BHOLD Core criará. Se você não estiver usando a instância padrão do servidor de banco de dados, especifique a instância do servidor de banco de dados no formato *\<servidor\>*\\*\<instância\>*. | Grave o nome de servidor (ou servidor e instância) aqui: Grave o nome do banco de dados aqui:                                                                                                                                                                                                                                                                                                                   |
| **Verifique as restrições para o usuário de banco de dados**    | Obsoleto.                                                                                                                                                                                                                                                                 | Não altere o valor padrão                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Instalação do BHOLD Core

Para instalar o módulo BHOLD Core, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo BHOLD Core: 

- BholdCore *\<Versão\>*\_Release.msi

Substitua *\<Versão\>* pelo número de versão do BHOLD Core que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

### <a name="postinstallation-settings"></a>Configurações pós-instalação

Concluída a instalação do BHOLD Core, você deverá configurar o Firewall do Windows e alterar configurações avançadas no pool de aplicativos do BHOLD Core nos Serviços de Informações da Internet para concluir a configuração do BHOLD Core. Se necessário, você também deverá alterar os atributos de sistema do BHOLD para atender às suas necessidades.

#### <a name="configuring-windows-firewall"></a>Configurando o Firewall do Windows

Se os usuários acessarão o BHOLD por meio de navegadores da web em computadores remotos, você deverá configurar o Firewall do Windows no servidor do BHOLD Core para permitir conexões de entrada na porta do site que você especificou quando você instalou BHOLD Core.

Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Para permitir conexões de entrada para o site do BHOLD

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas**, clique com o botão direito do mouse em **Firewall do Windows com Configurações Avançadas** e, em seguida, clique em **Executar como administrador**.

2.  No painel esquerdo, clique em **Regras de Entrada** e, no painel direito, clique em **Nova Regra**.

3.  No assistente para Nova Regra de Entrada, clique em **Porta** e, em seguida, clique em **Avançar**.

4.  Verifique se **TCP** está selecionado e, em **Portas locais específicas**, digite o número da porta padrão do BHOLD Core (5151) ou o número da porta que você especificou quando instalou o BHOLD Core e, em seguida, clique em **Avançar**.

5.  Verifique se **Permitir a conexão** está selecionado e clique em **Avançar**.

6.  Na página **Perfil**, desmarque as caixas de seleção para localizações das quais você não deseja acesso ao site do BHOLD e, em seguida, clique em **Avançar**.

7.  Na página **Nome**, digite um nome para a regra (por exemplo, Permitir conexões de entrada para o BHOLD Core) e, em seguida, clique em **Concluir**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Habilitando aplicativos de 32 bits para o pool de aplicativos do BHOLD Core

Para permitir que o IIS funcione corretamente com o módulo BHOLD Core, você deve configurar o pool de aplicativos do BHOLD Core para dar suporte a aplicativos de 32 bits. Você deve estar conectado com a conta usada para instalar o módulo BHOLD Core para executar esse procedimento.

**Para habilitar o suporte a aplicativos de 32 bits para o pool de aplicativos do BHOLD Core**

1.  Para iniciar o Gerenciador dos Serviços de Informações da Internet, clique em **Iniciar**, aponte para **Ferramentas do Administrador**e clique em **Gerenciador do IIS (Serviços de Informações da Internet)**.

2.  Na árvore de console, expanda o nome do servidor e, em seguida, clique em **Pools de Aplicativos**.

3.  No painel **Pools de Aplicativos**, clique com o botão direito do mouse em **CoreAppPool** e clique em **Configurações Avançadas**.

4.  Na caixa de diálogo **Configurações Avançadas** na lista **Habilitar Aplicativos de 32 Bits**, selecione **Verdadeiro** e, em seguida, clique em **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Estabelecendo o nome da entidade de serviço para o site do BHOLD

Se o nome de rede que é usado para contatar o site BHOLD não é igual ao nome de host do servidor, você deve estabelecer um SPN (nome da entidade de serviço) para HTTP. Por exemplo, se você usar um registro de recurso CNAME no DNS para especificar um alias para o servidor ou se você usar o balanceamento de carga de rede, você deverá registrar esses endereços de rede adicionais no Active Directory. Se você não conseguir fazer isso, o Internet Explorer não poderá usar o protocolo Kerberos ao contatar o site do BHOLD.

>[!IMPORTANT]
Se o módulo BHOLD Core estiver instalado no mesmo computador que o Portal do FIM, você deverá criar registros de recursos DNS (CNAME ou A) com nomes de host diferentes para os servidores que executam o BHOLD Core e o servidor que executa o Portal do FIM. Apenas um SPN pode ser estabelecido para um determinado par de serviço-tipo/servidor-alias e, portanto, o BHOLD Core e o Portal do FIM exigem SPNs separados porque eles geralmente são executados em contas diferentes. O comando setspn relatará um erro se um SPN já tiver sido estabelecido em outra conta.

A associação em **Administradores do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Para definir o SPN do site do BHOLD

1.  No controlador de domínio do Active Directory Domain Services, clique em **Iniciar**, **Todos os Programas** e **Acessórios**. Em seguida, clique com o botão direito do mouse em **Prompt de Comando** e clique em **Executar como administrador**.

2.  No prompt de comando, digite o seguinte comando e pressione ENTER: setspn –S HTTP/ *\<networkalias\> \<domain\>* \\ *\<accountname\>*, em que:

    -   *\<networkalias\>* é o endereço que os clientes usam para entrar em contato com o site do BHOLD

    -   *\<domain\>*\\*\<accountname\>* é o nome de usuário e domínio da conta de serviço do BHOLD Core que você criou quando instalou o BHOLD Core.

3.  Repita a etapa anterior para todos os outros nomes que os clientes usam para entrar em contato com o site BHOLD, por exemplo, aliases CNAME, nomes que incluem um nome de domínio totalmente qualificado ou nomes que incluem um nome de domínio NetBIOS (curto).

#### <a name="setting-bhold-system-attributes"></a>Definindo os atributos de sistema do BHOLD

Para validar que a instalação do módulo BHOLD Core foi bem-sucedida, abra o portal do BHOLD Core e exiba os atributos de sistema. Além disso, para garantir que o módulo BHOLD Core funcione corretamente em seu ambiente, você pode modificar os seguintes atributos de sistema do BHOLD, conforme apropriado:

| **Atributo**                | **Descrição**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Defina como S se o site do BHOLD está em execução em um serviço Web clusterizado para garantir que os itens exibidos recentemente funcionem corretamente. Defina como N se o site do BHOLD está em execução em um servidor IIS autônomo.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Defina como Y para garantir que as unidades organizacionais (orgunits) possam ser movidas apenas para orgunits com o mesmo tipo organizacional da orgunit pai. Por exemplo, isso impede que a orgunit de projeto seja movida para uma orgunit de departamento. Defina como N para permitir que uma orgunit seja colocada em uma orgunit de um tipo diferente. |
| **Dias entre execuções de ABA**     | Defina como um inteiro de dois dígitos para especificar o intervalo (em dias) entre duas execuções de ABA (autorização baseada em atributo). Por exemplo, para especificar que as execuções de ABA serão separadas por dois dias, digite 02.                                                                                                                     |
| **Hora de início da execução de ABA**    | Defina como um inteiro de dois dígitos para especificar a hora do dia quando uma execução de autorização baseada em atributo ocorrerá. Por exemplo, para especificar que a execução de ABA ocorrerá às 23h (23:00), digite 23.                                                                                                             |
| **Cardinalidade do Sistema**       | Defina como N se você não quiser uma verificação de cardinalidade do sistema no BHOLD. O valor padrão é S.                                                                                                                                                                                                                             |
| **Registro em log**                  | Defina como N se você não quiser que as alterações sejam registradas. O valor padrão é S.                                                                                                                                                                                                                                            |
| **Processamento de SystemQueue**   | Defina como N se você não desejar o processamento de fila do sistema. Não altere esse valor, a menos que seja instruído a fazer isso pelo suporte do produto.                                                                                                                                                                                           |

Você deve fazer logon como membro do grupo Administradores do Domínio para executar este procedimento.

**Para definir um atributo de sistema do BHOLD**

1.  Clique em **Iniciar**, em **Todos os Programas** e depois em **Internet Explorer**.

2.  Na caixa de endereço, digite , em que *\<servidor\>* é o nome do servidor de site do BHOLD e *\<porta\>* é o número da porta associado ao site.

3.  Clique em **Início**, clique em **Valores** e, em seguida, clique em **Modificar**.

4.  Localize o nome do atributo que você deseja alterar, digite o novo valor na caixa ao lado do nome do atributo e, em seguida, clique em **OK**.

## <a name="next-steps"></a>Próximas etapas

Depois que você instalou o BHOLD Core e validou que a instalação foi bem-sucedida, você pode instalar módulos adicionais. Nesse ponto, o banco de dados do BHOLD estará basicamente vazio, contendo somente uma conta de usuário, a conta raiz e uma unidade organizacional (orgunit), a orgunit raiz. Para adicionar mais usuários ao banco de dados do BHOLD, você pode instalar o módulo Conector de Gerenciamento de Acesso ou o módulo gerador de modelos do BHOLD, dependendo de suas necessidades. Você pode usar o módulo Conector de Gerenciamento de Acesso para importar dados de usuário do serviço de sincronização do FIM ou então você pode usar o Gerador de Modelos do BHOLD para importar dados de usuário de um conjunto de arquivos estruturados. Para obter mais informações sobre como usar o módulo Conector de Gerenciamento de Acesso, consulte [Guia de laboratório de teste: Conector de Gerenciamento de Acesso do BHOLD](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

Para obter mais informações sobre como usar o módulo Gerador de Modelos do BHOLD, consulte:

- [Guia de conceitos do Microsoft BHOLD Suite](https://technet.microsoft.com/en-us/library/jj134102(v=ws.10).aspx)
- [Referência Técnica do Microsoft BHOLD Suite](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx).
