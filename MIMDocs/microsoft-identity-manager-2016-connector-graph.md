---
title: O agente de gerenciamento Microsoft Identity Manager para o Microsoft Graph | Microsoft Docs
author: fimguy
description: O Microsoft Graph (visualização) é o gerenciamento de ciclo de vida de conta do AD de usuário externo. Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja dar a eles acesso à Autenticação Integrada do Windows local ou aplicativos baseados em Kerberos
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479359"
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>O agente de gerenciamento do Microsoft Identity Manager para o Microsoft Graph (Visualização Pública)
=======================================================================================

<a name="summary"></a>Resumo 
=======

O [agente de gerenciamento do Microsoft Identity Manager para o Microsoft Graph (visualização)](http://go.microsoft.com/fwlink/?LinkId=717495) habilita cenários de integração adicional para os clientes do Azure AD Premium.

O [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integra diretórios locais ao Azure AD e garante que os usuários tenham uma identidade comum e autenticação consistente em aplicativos do AD DS, Office 365, Azure e SaaS integrados ao Azure AD, sincronizando os usuários e grupos de diretórios locais com o Azure AD.   Esse agente de gerenciamento pode ser implantado para operações de gerenciamento de acesso e identidade especializadas além da sincronização de usuários e grupos no Azure AD.  Esse agente de gerenciamento aparece em objetos adicionais de metaverso de sincronização de MIM obtidos do [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 e beta. 

<a name="scenarios-covered"></a>Cenários abordados
=================

<a name="b2b-account-lifecycle-management"></a>Gerenciamento de ciclo de vida de conta B2B
--------------------------------

O cenário inicial na visualização para o agente de gerenciamento do Microsoft Identity Manager do Microsoft Graph (visualização) é o gerenciamento de ciclo de vida de contas do AD de usuário externo. Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja dar a eles acesso à Autenticação Integrada do Windows local ou aplicativos baseados em Kerberos, por meio do proxy de [aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou outros mecanismos de gateway. O proxy do aplicativo Azure AD exige que cada usuário tenha sua própria conta do AD DS, para fins de identificação e delegação

Cenários adicionais podem ser adicionados no futuro e [documentados aqui](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Determinar a topologia de implantação
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Preparar-se para usar o MA(Agente de Gerenciamento) para o Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autorizar o MA a gerenciar seu diretório do Azure AD
----------------------------------------------------

1.  O agente de gerenciamento do Graph requer que o aplicativo Web/aplicativo de API seja criado no Azure AD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Figura 1. Novo registro do aplicativo

2.  Abra o aplicativo criado e use a ID do Aplicativo como Id do Cliente na página de conectividade do MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Figura 2. ID do Aplicativo

2.  Gerar novo segredo do cliente por meio da abertura de todas as configurações-\> chaves. Defina uma descrição de chave e selecione duração needful. Salve as alterações. Um valor secreto não estará disponível depois que você sair de página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Figura 3. Novo segredo do cliente

3.  Adicione "API do Microsoft Graph" para o aplicativo abrindo "Permissões necessárias".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Figura 4. Adicionar nova API

A seguinte permissão deve ser adicionada para a "API do Microsoft Graph":

| Operação com objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importar Grupo          | Group.Read.All ou Group.ReadWrite.All                                                | Aplicativo     |
| Importar Usuário           | User.Read.All ou User.ReadWrite.All ou Directory.Read.All ou Directory.ReadWrite.All | Aplicativo     |

Mais detalhes sobre as permissões necessárias podem ser encontrados [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Crie um conector com a ID do Aplicativo e o Segredo do Cliente gerado. Cada agente de gerenciamento deve ter seu próprio aplicativo no Azure AD para evitar a importação em paralelo para o mesmo aplicativo. O conector do Graph dá suporte à seguinte lista de tipos de objeto:

-   Usuário

    -   Importação Completa/Delta

    -   Exportar (Adicionar, Atualizar, Excluir)

-   Group

    -   Importação Completa/Delta

    -   Exportar (Adicionar, Atualizar, Excluir)


A lista de tipos de atributos que têm suporte:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (cadeia de caracteres no espaço do conector)

-   microsoft.graph.directoryObject (referência no espaço do conector para qualquer um dos objetos com suporte)

-   microsoft.graph.contact

Os atributos de múltiplos valores (Conjunto) também têm suporte para qualquer tipo de formulário da lista anterior.

O conector do Graph usa o atributo 'id' para âncora e DN para todos os objetos.

Não há suporte para renomeação no momento, pois a GraphAPI não permite a alteração de objeto para o atributo 'id' para o objeto existente.

<a name="access-token-lifetime"></a>Tempo de vida do token de acesso
=====================

Um aplicativo do Graph requer um token de acesso para acessar a GraphAPI. Um conector solicitará um novo token de acesso para cada iteração de importação (a iteração de importação depende do tamanho da página). Por exemplo:

-   O AzureAD contém 10000 objetos

-   O tamanho de página configurado no conector é 5000

Nesse caso, haverá duas iterações durante a importação, e cada uma delas retornará 5 000 objetos para sincronização. Portanto, um novo token de acesso será solicitado duas vezes.

Observe que, durante a exportação, um novo token de acesso será solicitado para cada objeto que deve ser adicionado/atualizado/excluído.

<a name="installing-the-connector"></a>Instalar o conector
========================

Antes de usar o conector, verifique se você tem o seguinte no servidor de sincronização: Microsoft .NET 4.5.2 Framework ou posterior; Microsoft Identity Manager 2016 SP1; deve ser usado o hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

Conectores para o Microsoft Identity Manager 2016 SP1, o Conector do Connector está disponível como um download do [Centro de Download da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Configuração do conector
=======================

Página Conectividade:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Figura 5. Página Conectividade

A página de conectividade (Figura 1) contém a versão de API do Graph que é usada e o nome do locatário. A Id do Cliente e o Segredo do Cliente representam a ID do Aplicativo e o valor de Chave do aplicativo WebAPI que deve ser criado no Azure AD.

Página Parâmetros globais:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Figura 6. Página Parâmetros globais

A página de parâmetros globais contém as seguintes configurações:

Formato de data/hora – formato que é usado para qualquer atributo com o tipo Edm.DateTimeOffset. Todas as datas são convertidas em uma cadeia de caracteres usando esse formato durante a importação. O formato de conjunto é aplicado a qualquer atributo, o que salva a data.

Tempo limite de HTTP (segundos) – tempo limite em segundos que será usado durante cada chamada HTTP para o aplicativo WebAPI.

Forçar a alteração de senha do usuário criado no próximo logon – essa opção é usada para o novo usuário que será criado durante a exportação. Se a opção estiver habilitada, a propriedade [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) será definida como true; caso contrário, será false.

<a name="troubleshooting"></a>Solução de problemas
===============

**Habilitar logs**

Se houver algum problema no Graph, os logs poderão ser usados para localizar o problema. O conector do Graph usa a mesma origem que todos os conectores genéricos. Assim, os rastreamentos podem ser habilitados da [mesma forma que para conectores Genéricos; confira o wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou, simplesmente adicionando o seguinte a miiserver.exe.config (na seção system.diagnostics/sources):

\<source name="ConnectorsLog" switchValue="Verbose"\>

\<ouvintes\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

Observação: se 'Executar este agente de gerenciamento em um processo separado' estiver habilitado, dllhost.exe.config deverá ser usado em vez de miiserver.exe.config.

**Erro de token de acesso expirado**

O conector pode retornar uma mensagem de Erro 401 HTTP não autorizado: "O token de acesso expirou":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Figura 7. “O token de acesso expirou.” Erro do

A causa desse problema pode ser a configuração da vida útil do token de acesso do lado do Azure. Por padrão, o token de acesso expira após uma hora. Para aumentar o tempo de expiração, confira [este artigo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exemplo de como usar a [versão de visualização pública do módulo do PowerShell do Azure AD](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>Próximas etapas
----------
- [Explorador do Graph (ótimo para solução de problemas de chamada HTTP)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Controle de versão, suporte e políticas de alteração de falha para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Baixar o agente de gerenciamento do Microsoft Identity Manager para o Microsoft Graph (visualização)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Guias com suporte de cenários específicos
----------------------------------
[Implantação de ponta a ponta de MIM B2B]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
