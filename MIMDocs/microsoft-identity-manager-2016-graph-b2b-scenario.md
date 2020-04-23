---
title: Como configurar o conector do Microsoft Identity Manager para o Microsoft Graph para B2B | Microsoft Docs
author: billmath
description: O conector do Microsoft Graph é o gerenciamento de ciclo de vida de conta do AD de usuário externo. Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja dar a eles acesso à Autenticação Integrada do Windows local ou aplicativos baseados em Kerberos
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 0d5f970168934f3fcc4c721aad0a439e2babcfe7
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79381498"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Colaboração B2B (entre empresas) do Azure AD com Microsoft Identity Manager (MIM) 2016 SP1 com Proxy do Aplicativo Azure
============================================================================================================================

O cenário inicial é o gerenciamento do ciclo de vida da conta do AD do usuário externo.   Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja dar a eles acesso à Autenticação Integrada do Windows local ou aplicativos baseados em Kerberos, por meio do [Proxy de Aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou outros mecanismos de gateway. O Proxy de Aplicativo do Azure AD exige que cada usuário tenha sua própria conta do AD DS, para fins de identificação e delegação.

## <a name="scenario-specific-guidance"></a>Orientação específicas do cenário

Algumas suposições feitas na configuração de B2B com o MIM e o Proxy de Aplicativo do Azure AD:

-   Você já tiver implantado um AD local, o Microsoft Identity Manager estiver instalado e houver a configuração básica do Serviço MIM, do Portal do MIM, do AD MA (Agente de Gerenciamento do Active Directory) e FIM MA (Agente de Gerenciamento do FIM).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Você já seguiu as instruções no artigo sobre como baixar e instalar o [conector do Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Você tem o Azure AD Connect configurado para sincronização de usuários e grupos com o Azure AD.

-   Você já configurou os conectores do proxy de aplicativos e os grupos de conectores. Caso contrário, você pode visitar [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Você já publicou um ou mais aplicativos, que contam com a Autenticação Integrada do Windows ou contas do AD individuais por meio do Proxy do Aplicativo Azure AD

-   Você convidou ou convida um ou mais convidados, o que resultou em um ou mais usuários serem criados no <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal> do Azure AD



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Cenário de exemplo de implantação de ponta a ponta B2B

Este guia se baseia no cenário a seguir:

A Contoso Pharmaceuticals trabalha com a Trey Research Inc. como parte de seu departamento de Pesquisa e Desenvolvimento. Os funcionários da Trey Research precisam acessar o aplicativo de relatórios de pesquisa fornecido pela Contoso Pharmaceuticals.

-   A Contoso Pharmaceuticals está em seu próprio locatário para configurar um domínio personalizado.

-   Alguém convidou um usuário externo para o locatário da Contoso Pharmaceuticals.
    Este usuário aceitou o convite e pode acessar recursos compartilhados.

-   A Contoso Pharmaceuticals publicou um aplicativo por meio do Proxy de Aplicativo. Nesse cenário, o aplicativo de exemplo é o Portal do MIM. Isso permitiria que um usuário convidado participasse de processos do MIM, por exemplo, em cenários de suporte técnico ou para solicitar acesso a grupos no MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configurar o AD e o Azure AD Connect para excluir os usuários adicionados ao Azure AD

Por padrão, o Azure AD Connect presumirá que os usuários não administradores no Active Directory precisam ser sincronizados com o Azure AD.  Se o Azure AD Connect localizar um usuário existente no Azure AD que corresponda ao usuário do AD local, o Azure AD Connect combinará as duas contas e presumirá que essa é uma sincronização anterior do usuário e tornará o AD local autoritativo.  No entanto, esse comportamento padrão não é adequado para o fluxo B2B, em que a conta de usuário é originada no Azure AD. 

Portanto, os usuários incluídos no AD DS pelo MIM do Azure AD precisam ser armazenados de maneira que o Azure AD não tente sincronizá-los com o Azure AD.
Uma maneira de fazer isso é criar uma nova unidade organizacional no AD DS e configurar o Azure AD Connect para excluir a unidade organizacional.  

Mais informações podem ser encontradas em [Sincronização do Azure AD Connect: configurar a filtragem](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Criar o aplicativo do Azure AD 


Observação: antes de criar o agente de gerenciamento para o conector do Graph na sincronização do MIM, verifique se você examinou o guia para implantar o [Conector do Graph](microsoft-identity-manager-2016-connector-graph.md) e criou um aplicativo com uma ID e um segredo de cliente.
Verifique se o aplicativo foi autorizado para pelo menos uma destas permissões: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Criar o agente de gerenciamento


Na interface de usuário do Synchronization Service Manager, selecione **Conectores** e **Criar**.
Selecione **Graph (Microsoft)** e forneça um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividade

Na página Conectividade, você deve especificar a Versão da API do Graph. A PAI de produção é **V 1.0**, a não de produção é **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parâmetros globais

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurar hierarquia de provisionamento

Esta página é usada para mapear o componente DN, por exemplo OU, para o tipo de objeto que deve ser provisionado, por exemplo, organizationalUnit. Isso não é necessário para este cenário, portanto, deixe o padrão e clique em Avançar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar partições e hierarquias

Na página de partições e hierarquias, selecione todos os namespaces com objetos que você planeja importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecionar tipos de objeto

Na página de tipos de objeto, selecione os tipos de objeto que você planeja importar. Você deve selecionar pelo menos 'User'.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar atributos

Na tela Selecionar Atributos, selecione os atributos do Azure AD que serão necessários para gerenciar usuários B2B do AD. A "ID" do atributo é obrigatória.  Os atributos `userPrincipalName` e `userType` serão usados posteriormente nessa configuração.  Outros atributos são opcionais, incluindo

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar âncoras

Na tela Configurar Âncora, configurar o atributo de âncora é uma etapa necessária. por padrão, use o atributo de ID para mapeamento de usuário.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de conector

Na página de configuração Filtro de Conector, o MIM permite filtrar objetos com base no filtro de atributos. Neste cenário para B2B, a meta é trazer apenas Usuários com o valor do atributo `userType` igual a `Guest`, e não os usuários com userType igual a `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar regras de associação e projeção

Este guia pressupõe que você estará criando uma regra de sincronização.  Uma vez que a configuração de regras de associação e projeção é processada pela regra de sincronização, não é necessário identificar uma associação e projeção no próprio conector. Deixe a configuração padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar fluxo de atributo

Este guia pressupõe que você estará criando uma regra de sincronização.  Projeção não é necessária para definir o fluxo de atributo na sincronização do MIM, pois ela é processada pela regra de sincronização criada mais tarde. Deixe a configuração padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar desprovisionamento

A definição para configurar desprovisionamento permite que você configure a sincronização do MIM para excluir o objeto se o objeto do metaverso for excluído. Neste cenário, os tornamos desconectores, uma vez que a meta é deixá-los no Azure AD. Neste cenário, não estamos exportando nada para o Azure AD e o conector está configurado apenas para importação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensões

Configurar extensões neste agente de gerenciamento é uma opção, mas não é obrigatório, pois estamos usando uma regra de sincronização. Se decidirmos usar uma regra avançada no fluxo de atributo anteriormente, haverá uma opção para definir a extensão de regras.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Como estender o esquema de metaverso


Antes de criar a regra de sincronização, precisamos criar um atributo chamado userPrincipalName vinculado ao objeto person usando o MV Designer.

No cliente Sincronização, selecione Metaverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selecione o tipo de objeto de pessoa

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Em Ações, clique em Adicionar atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Por fim, preencha os seguintes detalhes

Nome do atributo: **userPrincipalName**

Tipo de Atributo: **Cadeia de Caracteres (Indexável)**

Indexado = **Verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criar Regras de Sincronização de Serviço MIM

Nas etapas abaixo, iniciamos o mapeamento da conta de convidado B2B e do fluxo de atributo. Aqui são feitas algumas suposições: que você já tem o MA do Active Directory e o MA do FIM configurados para trazer usuários do Portal e do Serviço do MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

As próximas etapas exigirão a adição de uma configuração mínima para o MA do FIM e o MA do AD.

Mais detalhes podem ser encontrados aqui para a configuração <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> - Como Provisionar Usuários no AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de Sincronização: importar Usuário Convidado para MV para o Metaverso do Serviço de Sincronização do Azure Active Directory<br>

Navegue até o Portal do MIM, selecione Regras de Sincronização e clique em Nova.  Crie uma regra de sincronização de entrada para o fluxo de B2B por meio do conector do Graph.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Na etapa de critérios de relacionamento, selecione "Criar recurso no FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configure as seguintes regras de fluxo de atributos de entrada.  Preencha os atributos `accountName`, `userPrincipalName` e `uid`, pois eles serão usados posteriormente neste cenário:

| **Somente o fluxo inicial** | **Usado como teste de existência** | **Fluxo (Valor de Origem ⇒ Atributo FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regra de Sincronização: criar conta de usuário convidado para o Active Directory 

Esta regra de sincronização cria o usuário no Active Directory.  Verifique se o fluxo para `dn` deve colocar o usuário na unidade organizacional excluída do Azure AD Connect.  Além disso, atualize o fluxo para `unicodePwd` para cumprir a política de senha do AD. O usuário não precisará saber a senha.  Observe que o valor de `262656` para `userAccountControl` codifica os sinalizadores `SMARTCARD_REQUIRED` e `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Regras de fluxo:

| **Somente o fluxo inicial** | **Usado como teste de existência** | **Fluxo (valor FIM ⇒ Atributo de Destino)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra de Sincronização Opcional: importar SID de Objetos do Usuário Convidado B2B para permitir logon no MIM 

Essa regra de sincronização de entrada recupera o atributo de SID do usuário do Active Directory de volta para o MIM para que o usuário possa acessar o Portal do MIM.  O Portal do MIM requer que o usuário tenha os atributos `samAccountName`, `domain` e `objectSid` preenchidos no banco de dados de serviço do MIM.

Configure o sistema externo de origem como o `ADMA`, uma vez que o atributo `objectSid` será definido automaticamente pelo AD quando o MIM criar o usuário.
 
Observe que, se você configurar os usuários a serem criados no Serviço do MIM, eles não deverão estar no escopo de nenhum conjunto destinado a regras de política de gerenciamento de SSPR.  Talvez você precise alterar suas definições de conjunto para excluir os usuários criados pelo fluxo de B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Somente o fluxo inicial** | **Usado como teste de existência** | **Fluxo (Valor de Origem ⇒ Atributo FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Executar as regras de sincronização

Em seguida, convidamos o usuário e executamos as regras de sincronização do agente de gerenciamento na seguinte ordem:

-   Importação e Sincronização Completas no Agente de Gerenciamento `MIMMA`.  Isso garante a que sincronização do MIM tenha as regras de sincronização mais recente configuradas.

-   Importação e Sincronização Completas no Agente de Gerenciamento `ADMA`.  Isso garante que o MIM e o Active Directory sejam consistentes.  Neste ponto, ainda não haverá nenhuma exportação pendente para convidados.

-   Importação e sincronização completas no Agente de Gerenciamento B2B do Graph.  Isso traz os usuários convidados para o metaverso.  Neste ponto, uma ou mais contas terá exportação pendente para `ADMA`.  Se não houver nenhuma exportação pendente, verifique se os usuários convidados foram importados para o espaço do conector e se as regras foram configuradas para que eles possam receber contas do AD.

-   Exportação, Importação Delta e Sincronização no Agente de Gerenciamento `ADMA`.  Se as exportações falharem, verifique a configuração de regra e determine se havia algum requisito de esquema ausente. 

-   Exportação, Importação Delta e Sincronização no Agente de Gerenciamento `MIMMA`.  Quando isso for concluído, não deve mais haver nenhuma exportação pendente.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcional: Proxy de Aplicativo para convidados B2B fazendo logon no Portal do MIM

Agora que criamos as regras de sincronização no MIM. Na configuração do Proxy de Aplicativo, defina o uso da entidade de segurança de nuvem para permitir o KCD no proxy de aplicativo.
Além disso, em seguida, o usuário foi adicionado manualmente para o gerenciamento de usuários e grupos. As opções para não mostrar o usuário até que a criação tenha ocorrido no MIM para adicionar o convidado a um grupo do Office, uma vez provisionado, requer um pouco mais de configuração não abordada neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Depois que tudo estiver configurado, peça para o usuário B2B fazer logon e ver o aplicativo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Próximas etapas
----------

[Como Provisionar Usuários no AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Referência de Função para FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Como fornecer acesso remoto seguro a aplicativos locais](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Baixe o conector do Microsoft Identity Manager para o Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
