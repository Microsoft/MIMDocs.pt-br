---
title: Instalação de Integração de FIM/MIM do BHOLD | Microsoft Docs
description: O módulo Integração do BHOLD adiciona gerenciamento de função de autoatendimento para MIM e do FIM
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 317c9ae4c940a509b6ac328cd5bb7cd7baa4dde9
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516006"
---
# <a name="bhold-fimmim-integration-installation"></a>Instalação de Integração de FIM/MIM com o BHOLD

O módulo Integração de FIM com o BHOLD adiciona gerenciamento de função de autoatendimento para o Microsoft Identity Manager, tornando possível para os usuários solicitar funções adicionais e impor quem é que pode assumir essas funções. O módulo Integração de FIM com o BHOLD estende o Portal do FIM para tornar mais fácil gerenciar funções de usuários como parte da administração geral de FIM. Este tópico descreve como você deve configurar sua infraestrutura de rede para permitir que você instale e use o módulo Integração de FIM com o BHOLD. Ele também aborda como instalar o módulo Integração de FIM com o BHOLD e a configuração que é necessária após você instalar o módulo Integração de FIM com o BHOLD.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisitos de instalação da Integração de FIM com o BHOLD

O módulo Integração de FIM com o BHOLD estende o Portal do FIM e o Serviço FIM para permitir que os usuários gerenciem as respectivas funções dentro do Portal do FIM. Por esse motivo, é essencial que o módulo BHOLD Core e os recursos necessários do FIM sejam instalados e configurados antes de se instalar o módulo Integração de FIM com o BHOLD.
O itens a seguir são os componentes de software que devem estar presentes no computador antes da instalação do módulo Integração de FIM com o BHOLD:

- Portal e Serviço do Microsoft Identity Manager 2016
- Microsoft Silverlight 3 ou posterior
- Serviços de Informações da Internet e ASP.NET
- Ferramentas do Microsoft Silverlight

Além disso, os módulos BHOLD Core e Conector de Gerenciamento de Acesso já deverão ter sido implantados em um servidor no ambiente e o FIM deverá ser configurado com um ou mais agentes de gerenciamento do BHOLD. Para obter informações sobre como instalar e configurar o módulo BHOLD Core, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Para obter informações sobre como instalar e usar o módulo Conector de Gerenciamento de Acesso, consulte [Instalação do Conector de Gerenciamento de Acesso](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) e [Guia de Laboratório de Teste: Conector de Gerenciamento de Acesso do BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> O nome do banco de dados do serviço FIM deve ser FIMService. A configuração da Integração de FIM com o BHOLD falhará se o FIM não tiver sido instalado com o nome de banco de dados padrão do serviço FIM.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo Integração de FIM com o BHOLD, você deverá criar um diretório do BHOLD no diretório raiz da unidade de disco C: (C:\BHOLD).

Além disso, você precisa estar preparado para fornecer as informações que o assistente de Configuração da Integração de FIM com o BHOLD requer para concluir a instalação. A planilha a seguir ajudará você a registrar essas informações, de modo que você estará pronto para fornecê-las quando necessário.

### <a name="bholdfim-account-settings"></a>Configurações de conta de FIM do BHOLD

| **Item**                            | **Descrição**                                                                                                                                                                                                               | **Valor**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar um provedor de segurança em um Domínio** | Quando selecionado, especifica que a segurança do Active Directory Domain Services controlará o acesso ao BHOLD Core.                                                                                                                    | Marque a caixa de seleção. **Importante:** a instalação falhará se essa caixa de seleção não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                          | Especifica o domínio que contém a **conta de serviço** que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio é fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Nome de usuário**                        | Especifica o nome de logon da conta de usuário do serviço BHOLD Core.                                                                                                                                                              | Grave o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Senha**                        | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                           | Grave a senha aqui: **Importante:** mantenha essa senha em um local seguro e oculto.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Configurações do Serviço FIM

| **Item**            | **Descrição**                                                                                                                                                                                                                               | **Valor**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Usuário**            | Especifica o nome de logon de uma conta com privilégios de administrador para o FIM. A Microsoft recomenda fortemente que você não use a conta associada ao usuário raiz no BHOLD Core (por padrão, a conta usada para instalar o BHOLD Core). | Grave o nome da conta de usuário aqui:                                                                   |
| **Senha**        | Especifica a senha da conta de usuário do administrador do FIM.                                                                                                                                                                                 | Grave a senha aqui: **Importante:** mantenha essa senha em um local seguro e oculto. |
| **Banco de dados do FIM**    | Especifica o nome do banco de dados do Serviço FIM.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/Porta do Site** | Especifica o nome ou endereço IP do servidor de Portal do FIM e a porta do site.                                                                                                                                                               | Grave o nome do servidor ou o endereço e a porta aqui:                                                     |

### <a name="bhold-core-connection"></a>Conexão do BHOLD Core

| **Item**               | **Descrição**                                                                                                                                                                                                                                                                                                                                                                               | **Valor**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domínio**             | Especifica o nome do domínio da conta especificada no **Usuário**, abaixo. Especifique o domínio no formato de NetBIOS (curto).                                                                                                                                                                                                                                                                   | Grave o nome de domínio da conta de usuário aqui:                                                            |
| **Usuário**               | Especifica o nome de logon da conta de **um usuário BHOLD que é um supervisor** de todos os usuários e funções e tem permissão para vincular e desvincular funções de usuário. A Microsoft recomenda fortemente que você não use a conta associada ao usuário raiz no BHOLD Core (por padrão, a conta usada para instalar o BHOLD Core). Essa conta pode ser a mesma conta que você usa para se conectar ao FIM | Grave o nome da conta de usuário aqui:                                                                   |
| **Senha**           | Especifica a senha da conta de usuário especificada em **Usuário**.                                                                                                                                                                                                                                                                                                                             | Grave a senha aqui: **Importante:** mantenha essa senha em um local seguro e oculto. |
| **Endereço IP/de Máquina** | Especifica o endereço IP do servidor do site do BHOLD Core. Não use o nome do servidor.                                                                                                                                                                                                                                                                                                        | Grave o endereço IP aqui:                                                                          |
| **Número da porta**        | Especifica o número da porta do site do BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Grave o número da porta aqui:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Configuração da Integração de FIM com o BHOLD

Para instalar o módulo Integração de FIM com o BHOLD, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo Integração de FIM com o BHOLD:

- BholdFIMIntegration<em>\<Versão\></em>\_Release.msi

Substitua *\<Versão\>* pelo número de versão da Integração de FIM com o BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

![executando o msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Tarefas de pós-instalação

Depois de instalar a Integração de FIM com o BHOLD, você deverá configurar o Microsoft SharePoint para conceder permissões de proprietário do site da conta de serviço do BHOLD. Além disso, se o Portal do FIM estiver configurado para usar a segurança de protocolo SSL, você deverá modificar os arquivos que contiverem referências para os endereços de páginas do BHOLD adicionados ao Portal do FIM.

### <a name="configuring-sharepoint"></a>Configurando o SharePoint

Para funcionar corretamente, a Integração de FIM com o BHOLD exige que a conta de serviço do BHOLD tenha permissões de membro de site no Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Para conceder permissões de membro de site para a conta de serviço do BHOLD

1.  Faça logon no servidor que executa a Integração de FIM com o BHOLD com privilégios de administrador.

2.  Clique em **Iniciar** e clique em **Internet Explorer**.

3.  Na barra de endereços, digite <https://localhost> se o SharePoint estiver configurado para usar a segurança SSL; caso contrário, digite <http://localhost>.

4.  No lado esquerdo da página do **Site da Equipe**, clique em **Pessoas e Grupos**.

5.  Em **Grupos** clique em **Membros do Site da Equipe** e na barra de ferramentas do painel central, clique em **Novo** e, em seguida, clique em **Adicionar Usuários**.

6.  Na página **Adicionar Usuários: Site da Equipe**, em **Usuários/Grupos**, digite BHOLDApplicationGroup e clique no botão Verificar Nomes sob a caixa **Usuários/Grupos**. O nome do grupo deve ser resolvido para incluir o nome de domínio.

7.  Clique em **Conceder permissões a usuários diretamente**, selecione **Controle Total – Tem Controle Total** e, em seguida, clique em **OK**.

8.  Verifique se BHOLDApplicationGroup está listado em **Permissões: Site da Equipe** e, em seguida, feche o Internet Explorer.

![executando o msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configurando o BHOLD para dar suporte a SSL

Se o Portal do FIM estiver configurado para usar a segurança SSL, você deverá modificar arquivos no servidor do FIM para que os links para páginas do BHOLD sejam abertos. Os arquivos estão localizados na pasta a seguir: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

A tabela a seguir lista os arquivos e as versões originais e alteradas das cadeias de caracteres a serem editadas.

| **Arquivo**                  | **Cadeia de caracteres original**                                                                                                                   | **Cadeia de Caracteres Alterada**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>* \>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Sendo que:

-   *\<BHOLD_Server\>* especifica o nome do servidor do BHOLD conforme encontrado na versão original do arquivo

-   *\<MIM_Server\>* especifica o nome do servidor do FIM conforme encontrado na versão original do arquivo

-   *\<BHOLD_Server_FQDN\>*  Especifica o FQDN (nome de domínio totalmente qualificado) do servidor do BHOLD

-   *\<MIM_Port\>* especifica o número da porta do servidor do FIM conforme encontrado na versão original do arquivo

-   *\<MIM_Server_FQDN\>* especifica o FQDN do servidor do FIM

-   *\<MIM_SSL_Port\>* especifica uma porta diferente para uso com SSL no servidor do FIM

### <a name="enable-approval-workflows-in-bhold-core"></a>Habilitar fluxos de trabalho de aprovação no BHOLD Core

Quando o FIM e BHOLD são integrados para autoatendimento, os fluxos de trabalho para aprovações são executados no Serviço FIM. Isso é semelhante ao modelo de fluxo de trabalho para solicitações originadas no Portal do FIM, assim como quando um usuário envia uma solicitação para ingressar em uma lista de distribuição. No entanto, há diferenças importantes entre fluxos de trabalho de função do BHOLD e outros fluxos de trabalho hospedados no Serviço FIM. No caso do BHOLD, os parâmetros de fluxo de trabalho que especificam quais usuários devem aprovar uma solicitação de função se originam no BHOLD, em vez de serem armazenados nas definições de fluxo de trabalho no banco de dados do Serviço FIM. Esses parâmetros são fornecidos para o Serviço FIM pelo BHOLD quando a primeira solicitação é feita e então um fluxo de trabalho de ação comunica os resultados de volta ao BHOLD Core.

O BHOLD seleciona um aprovador para uma solicitação de autoatendimento de uma de três maneiras:

-   **Gerente de linha como aprovador: seleção baseada em função para uma unidade organizacional (OrgUnit)** Se uma função tiver um atributo chamado roletype definido como Approver ou Escalator e, se essa função estiver vinculada a um ou mais usuários no contexto de uma OrgUnit, solicitações de usuários dentro dessa OrgUnit deverão ser aprovadas por um dos usuários vinculados à função com o roletype Approver ou Escalator.

-   **Gerente de linha como aprovador: seleção com base em atributo para uma OrgUnit** Cada OrgUnit pode ter um ou mais atributos que especificam os aliases de usuários que podem aprovar atribuições de função para outros usuários na OrgUnit. Esses atributos são nomeados approver1, approver2 e assim por diante. Quando um usuário na OrgUnit solicita uma atribuição de função, o BHOLD encaminha a solicitação (via FIM) para os usuários especificados pelos atributos de aprovador da OrgUnit. Se uma OrgUnit não tem nenhum desses atributos definido, o BHOLD verifica as OrgUnits pai até a OrgUnit raiz.

-   **Gerente de função como aprovador: seleção com base em atributo para uma função** Uma função pode ter um ou mais atributos (também nomeados approver1 e assim por diante) que especificam os aliases de usuários que podem aprovar a atribuição da função. Quando um usuário solicita que uma função que tem esses atributos definidos seja atribuída a ele, o BHOLD encaminha a solicitação para os usuários especificados pelos atributos.

Se um aprovador para uma solicitação de função de autoatendimento não é especificado por um desses métodos, o BHOLD atribui a função automaticamente, por padrão, sem a necessidade de aprovação. Por esse motivo, imediatamente após a instalação da Integração do FIM com o BHOLD, você deverá configurar a OrgUnit raiz com o alias de um aprovador, tal como a conta raiz. Isso impedirá que uma função seja concedida inadvertidamente a um usuário antes que uma política de aprovação mais abrangente possa ser implementada.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Para configurar um aprovador para a OrgUnit raiz

1.  Faça logon no servidor do BHOLD Core como administrador.

2.  Clique em **Iniciar** e clique em **Internet Explorer**.

3.  Na barra de endereços do Internet Explorer, digite <http://localhost:5151/bhold/core> e pressione Enter.

4.  Na home page do BHOLD Core, em **Def. do atributo**, clique em **Tipos de atributo**.

5.  Na página **Tipo de atributo**, clique em **Adicionar**.

6.  Na **página Adicionar tipo de atributo**, em **Identidade**, digite approver1; na lista **Tipo de dados**, clique em **Alfanumérica**; em **Comprimento máximo**, digite 255; em **Lista de valores**, clique em **Não**; em **Inglês**, digite Aprovador 1, clique em **OK** e, em seguida, clique em **Concluído**.

7.  No painel esquerdo, em **Def. de atributo** clique em **Conjuntos de tipos de atributo**.

8.  Na página **Conjuntos de tipos de atributo**, clique em **Adicionar**.

9.  Na página **Adicionar conjunto de tipos de atributo**, em **Descrição**, digite Atributos OrgUnit e, em seguida, clique em **OK**.

10. Na página **Atributos OrgUnit**, expanda **Tipos de atributo** e, em seguida, clique em **Modificar**.

11. Na lista **Tipo de atributo**, clique em **approver1**, clique em **Adicionar** e, em seguida, clique em **Concluído**.

12. No painel esquerdo, clique em **Tipos de objeto**.

13. Na página **Tipos de objeto**, clique em **OrgUnit**.

14. Na página **Tipo de objeto/OrgUnit**, expanda **Conjuntos de tipos de atributo** e, em seguida, clique em **Modificar**.

15. Na página **Vincular o conjunto de tipos de atributo/OrgUnit**, em **Ordem**, digite 10; na lista **Conjunto de tipos de atributo**, clique em **Atributos OrgUnit**, clique em **Adicionar** e, em seguida, clique em **Concluído**.

16. No painel esquerdo, em **Modelo**, clique em **Unidades organizacionais**.

17. Na página **Unidades organizacionais**, clique em **raiz**.

18. Na página **Unidade organizacional/raiz**, clique em **Modificar**.

19. Na página **Modificar atributos da unidade organizacional/raiz**, em **Aprovador**, digite o nome de usuário e de domínio do usuário que aprovará solicitações de atribuição de função, no formato *\<domínio\>* \\ *\<usuário\>* , em que  *\<domínio\>* é o nome de domínio NetBIOS (curto) e *\<usuário\>* é o nome de logon do usuário.
20. Clique em **OK**.

> [!IMPORTANT]
> O nome de usuário e de domínio deve corresponder ao alias padrão de um usuário no banco de dados do BHOLD Core.

Como uma alternativa para especificar um aprovador para unidades organizacionais, você pode especificar um aprovador para funções propostas no banco de dados do BHOLD Core. Para fazer isso, crie o atributo approver1, adicione-o a um conjunto de tipos de atributo associado ao tipo de objeto de Função e, em seguida, modifique cada função proposta para especificar o aprovador.

Para fornecer maior segurança de fluxo de trabalho, além de aprovadores, você deve designar modos de aprovação e usuários adicionais, criando e populando os seguintes atributos para OrgUnits e funções:

- escalator<em>\<n\></em>

- owner<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- notification<em>\<n\></em>

em que *\<n\>* indica um sufixo numérico opcional para fornecer vários atributos do mesmo tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Verificar os fluxos de trabalho de aprovação configurados no Serviço FIM

A instalação da integração do FIM com o BHOLD cria conjuntos, definições de fluxo de trabalho e MPRs (regras de política de gerenciamento) para o Serviço FIM. Se você tiver personalizado sua implantação do FIM para alterar os conjuntos de administradores ou os conjuntos de usuários capazes de fazer solicitações, você deverá garantir que as MPRs façam referência aos conjuntos de usuários corretos.

> [!NOTE]
> Antes que os usuários do Portal do FIM possam usar os recursos de autoatendimento fornecidos pelo BHOLD, as contas de usuários deverão ser sincronizadas no banco de dados do BHOLD por meio do Serviço de Sincronização do FIM. Em particular, deve haver um registro de usuário no banco de dados do BHOLD Core e no banco de dados do Serviço FIM para cada usuário que pode fazer uma solicitação de autoatendimento ou que é especificado como um aprovador ou escalonador para solicitações de autoatendimento.

## <a name="next-steps"></a>Próximas etapas

- Para obter informações sobre como instalar o Portal do FIM e outros recursos do FIM, consulte [Planejamento e Arquitetura](https://technet.microsoft.com/library/ee808044.aspx) na Biblioteca Técnica do Microsoft Forefront.
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)
