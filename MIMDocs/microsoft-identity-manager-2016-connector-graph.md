---
title: O conector do Microsoft Identity Manager para o Microsoft Graph | Microsoft Docs
author: fimguy
description: O conector do Microsoft Identity Manager para o Microsoft Graph permite o gerenciamento de ciclo de vida de conta do AD de usuário externo. Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja dar a eles acesso à Autenticação Integrada do Windows local ou aplicativos baseados em Kerberos
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 462b649ca02519e5af5c3b1243506a74efa7052a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044252"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Conector do Microsoft Identity Manager para o Microsoft Graph


## <a name="summary"></a>Resumo 


O [conector do Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) habilita cenários de integração adicional para os clientes do Azure AD Premium.  Ele aparece em objetos adicionais de metaverso de sincronização do MIM obtidos da  [API do Microsoft Graph](https://developer.microsoft.com/en-us/graph/) v1 e beta.

## <a name="scenarios-covered"></a>Cenários abordados


### <a name="b2b-account-lifecycle-management"></a>Gerenciamento de ciclo de vida de conta B2B


O cenário inicial para o conector do Microsoft Identity Manager para o Microsoft Graph é como um conector para ajudar a automatizar o gerenciamento de ciclo de vida de conta do AD DS para usuários externos. Nesse cenário, uma organização está sincronizando os funcionários com o Azure AD do AD DS usando o Azure AD Connect e também convidou convidados para seu diretório do Azure AD. Convidar alguém resulta em um objeto de usuário externo estar no diretório do Azure AD daquela organização, que não está do AD DS da organização. Então a organização deseja dar a esses convidados acesso a Autenticação Integrada do Windows local ou a aplicativos baseados em Kerberos por meio de [Proxy de Aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou outros mecanismos de gateway. O Proxy de Aplicativo do Azure AD exige que cada usuário tenha sua própria conta do AD DS, para fins de identificação e delegação.  

Para saber como configurar a sincronização de MIM para criar e manter automaticamente contas do AD DS para convidados, depois de ler as instruções neste artigo, continue lendo o artigo [Colaboração B2B (entre empresas) do Azure AD com o MIM 2016 SP1 com o Proxy de Aplicativo do Azure](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Este artigo ilustra as regras de sincronização necessárias para o conector.

### <a name="other-identity-management-scenarios"></a>Outros cenários de gerenciamento de identidade


O conector pode ser usado para outros cenários de gerenciamento de identidade específicos que envolvem criar, ler, atualizar e excluir objetos de usuário, grupo e contato no Azure AD, além da sincronização de usuário e grupo com o Azure AD. Ao avaliar os cenários possíveis, lembre: esse conector não pode ser operado em um cenário que resultaria em uma sobreposição de fluxo de dados, conflito de sincronização real ou potencial com uma implantação do Azure AD Connect.  O [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594)  é a abordagem recomendada para integrar diretórios locais ao Azure AD, sincronizando os usuários e grupos de diretórios locais ao Azure AD.  O Azure AD Connect tem muitos outros recursos de sincronização e permite cenários como senha e write-back de dispositivo, que não são possíveis para objetos criados pelo MIM. Se os dados estiverem sendo levados para o AD DS, por exemplo, garanta que sejam excluídos da tentativa do Azure AD Connect de combinar esses objetos de volta com o diretório do Azure AD.  Esse conector também não pode ser usado para fazer alterações a objetos do Azure AD criados pelo Azure AD Connect.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Como preparar-se para usar o Connector para o Microsoft Graph

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Como autorizar o conector a recuperar ou gerenciar objetos no diretório do Azure AD


1.  O conector requer que um aplicativo Web/API seja criado no Azure AD para que ele possa ser autorizado com as permissões apropriadas para operar no Azure AD por meio do Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Figura 1. Novo registro do aplicativo

2.  No portal do Azure, abra o aplicativo criado e salve a ID do aplicativo como uma ID de cliente para usar mais tarde na página de conectividade do MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Figura 2. ID do aplicativo

3.  Gerar novo segredo do cliente por meio da abertura de todas as configurações-\> chaves. Defina uma descrição de chave e selecione duração needful. Salve as alterações. Um valor secreto não estará disponível depois que você sair de página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Figura 3. Novo segredo do cliente

4.  Adicione "API do Microsoft Graph" para o aplicativo abrindo "Permissões necessárias".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Figura 4. Adicionar nova API

A permissão a seguir deve ser adicionada ao aplicativo para permitir que ele use a "API do Microsoft Graph", dependendo do cenário:

| Operação com objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importar Grupo          | `Group.Read.All` ou `Group.ReadWrite.All`                                                | Aplicativo     |
| Importar Usuário           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All` | Aplicativo     |

Mais detalhes sobre as permissões necessárias podem ser encontrados [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Conceda ao aplicativo as permissões necessárias.


## <a name="installing-the-connector"></a>Instalar o conector


6.  Antes de instalar o conector, verifique se que você tem o seguinte no servidor de sincronização: 

 - Microsoft .NET 4.5.2 Framework ou posterior
 - Microsoft Identity Manager 2016 SP1, e deve usar o hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

7. O conector do Microsoft Graph, além de outros conectores para o Microsoft Identity Manager 2016 SP1, está disponível como um download do [Centro de Download da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Reinicie o Serviço de Sincronização do MIM.
 
## <a name="connector-configuration"></a>Configuração do conector



9.  Na interface do usuário do Synchronization Service Manager, selecione  **Conectores**  e  **Criar**.
Selecione  **Graph (Microsoft)**  , crie um conector e dê a ele um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. No serviço de sincronização de MIM da interface do usuário, especifique a ID do aplicativo e o Segredo do Cliente gerado. Cada agente de gerenciamento configurado na Sincronização do MIM deve ter seu próprio aplicativo no Azure AD para evitar a execução da importação em paralelo para o mesmo aplicativo.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Figura 5. Página de conectividade

A página de conectividade (Figura 5) contém a versão de API do Graph que é usada e o nome do locatário. A ID do Cliente e o Segredo do Cliente representam a ID do Aplicativo e o valor de Chave do aplicativo WebAPI que deve ser criado no Azure AD.

11. Faça as alterações necessárias na página Parâmetros Globais:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Figura 6. Página Parâmetros globais

A página de parâmetros globais contém as seguintes configurações:

- Formato de data/hora – formato que é usado para qualquer atributo com o tipo Edm.DateTimeOffset. Todas as datas são convertidas em uma cadeia de caracteres usando esse formato durante a importação. O formato de conjunto é aplicado a qualquer atributo, o que salva a data.

 - Tempo limite de HTTP (segundos) – tempo limite em segundos que será usado durante cada chamada HTTP para o aplicativo WebAPI.

 - Forçar a alteração de senha do usuário criado no próximo logon – essa opção é usada para o novo usuário que será criado durante a exportação. Se a opção estiver habilitada, a propriedade [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) será definida como true; caso contrário, será false.

## <a name="configuring-the-connector-schema-and-operations"></a>Configurando o esquema do conector e as operações


12.   Configure o esquema.  O conector dá suporte à seguinte lista de tipos de objeto:

-   Usuário

    -   Importação Completa/Delta

    -   Exportar (Adicionar, Atualizar, Excluir)

-   Grupo

    -   Importação Completa/Delta

    -   Exportar (Adicionar, Atualizar, Excluir)


A lista de tipos de atributos que têm suporte:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (cadeia de caracteres no espaço do conector)

-   `microsoft.graph.directoryObject` (referência no espaço do conector a qualquer um dos objetos com suporte)

-   `microsoft.graph.contact`

Os atributos de múltiplos valores (Coleção) também têm suporte para qualquer tipo de formulário da lista acima.

O conector usa o atributo '`id`' para âncora e DN para todos os objetos.  Portanto, não é necessário renomear, pois a API do Graph não permite que um objeto altere o atributo 'id'.


## <a name="access-token-lifetime"></a>Tempo de vida do token de acesso


Um aplicativo do Graph requer um token de acesso para acessar a API do Graph. Um conector solicitará um novo token de acesso para cada iteração de importação (a iteração de importação depende do tamanho da página). Por exemplo:

-   O Azure AD contém 10.000 objetos

-   O tamanho de página configurado no conector é 5000

Nesse caso, haverá duas iterações durante a importação, e cada uma delas retornará 5.000 objetos para sincronização. Portanto, um novo token de acesso será solicitado duas vezes.

Durante a exportação, um novo token de acesso será solicitado para cada objeto que deve ser adicionado/atualizado/excluído.

## <a name="troubleshooting"></a>Solução de problemas


**Habilitar logs**

Se houver algum problema no Graph, os logs poderão ser usados para localizar o problema. Assim, os rastreamentos podem ser habilitados da [mesma forma que para conectores Genéricos](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou simplesmente adicionando o seguinte a `miiserver.exe.config` (dentro da seção `system.diagnostics/sources`):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>Se 'Executar este agente de gerenciamento em um processo separado' estiver habilitado, `dllhost.exe.config` deverá ser usado em vez de `miiserver.exe.config`.

**Erro de token de acesso expirado**

O conector pode retornar uma mensagem de Erro 401 HTTP não autorizado: "O token de acesso expirou":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Figura 7. “O token de acesso expirou.” Erro do

A causa desse problema pode ser a configuração da vida útil do token de acesso do lado do Azure. Por padrão, o token de acesso expira após uma hora. Para aumentar o tempo de expiração, confira [este artigo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exemplo de como usar a [versão de visualização pública do módulo do PowerShell do Azure AD](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"** }}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

## <a name="next-steps"></a>Próximas etapas

- [Explorador do Graph, ótimo para solução de problemas de chamada HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Controle de versão, suporte e políticas de alteração de falha para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Baixe o conector do Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
[Implantação de ponta a ponta B2B]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
