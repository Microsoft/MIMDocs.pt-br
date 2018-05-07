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
ms.openlocfilehash: 77d322f447546897ad18f0981e5faad12efafef1
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Colaboração business-to-business (B2B) do Azure AD com Microsoft Identity Manager (MIM) 2016 SP1 com Proxy do Aplicativo Azure (Visualização Pública)
============================================================================================================================

O cenário inicial na visualização é o gerenciamento do ciclo de vida da conta do AD do usuário externo.   Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja dar a eles acesso à Autenticação Integrada do Windows local ou aplicativos baseados em Kerberos, por meio do proxy de [aplicativo do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) ou outros mecanismos de gateway. O proxy do aplicativo Azure AD exige que cada usuário tenha sua própria conta do AD DS, para fins de identificação e delegação

## <a name="scenario-specific-supported-guidance"></a>Diretrizes específicas do cenário com suporte

Nesse cenário, uma organização fez alguns convites para o seu diretório do Azure AD e deseja conceder a esses convidados acesso ao Windows no local. Autenticação Integrada ou aplicativos baseados no Kerberos, por meio do proxy do [aplicativo Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O proxy do aplicativo Azure AD exige que cada usuário tenha sua própria conta do AD DS, para fins de identificação e delegação

Algumas suposições feitas na configuração de B2B com o MIM e o Proxy do Aplicativo Azure

-   Você já instalou o [Graph Management Agent](microsoft-identity-manager-2016-connector-graph.md).

-   Você tem um AD local e o Azure AD Connect configurado para sincronizar usuários e grupos com o Azure AD.

    -   Grupos do Office controlando o acesso ao aplicativo usando o [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Você já configurou os conectores do proxy de aplicativos e os grupos de conectores. Caso contrário, você pode visitar [aqui](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Um ou mais aplicativos publicados, que contam com a Autenticação Integrada do Windows ou contas do AD individuais por meio do Proxy do Aplicativo Azure AD

-   Você convidou ou convida um ou mais convidados criados no Azure AD <https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager instalado e configuração básica do Serviço, Portal e Agente de Gerenciamento do Active Directory.
    <https://docs.microsoft.com/en-us/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>Implantação de ponta a ponta B2B

Cenário

A Contoso Pharmaceuticals trabalha com a Trey Research Inc. como parte de seu departamento de Pesquisa e Desenvolvimento. Os funcionários da Trey Research precisam acessar o aplicativo de relatórios de pesquisa fornecido pela Contoso Pharmaceuticals.

-   A Contoso Pharmaceuticals está em um locatário independente para configurar um domínio personalizado.

-   Usuário externo convidado para o locatário da Contoso Pharmaceuticals.
    Este usuário aceitou o convite e pode acessar recursos compartilhados.

-   Publicado de um aplicativo por meio do Proxy de Aplicativo, e neste cenário, como um exemplo para usar o serviço e portal do MIM para que o usuário convidado participe do Processo do MIM, Exemplos de cenários do Helpdesk.

## <a name="create-the-graph-management-agent"></a>Criar o Graph Management Agent

Nota: Antes de criar o conector, verifique se você revisou o [Graph Management Agent](microsoft-identity-manager-2016-connector-graph.md).

Na interface de usuário do Synchronization Service Manager, selecione **Conectores** e **Criar**.
Selecione **Graph (Microsoft)** e forneça um nome descritivo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividade

Na página Conectividade, você deve especificar que a Versão da API de gráficos pronta para produção é **V 1.0** , Não produção é **Beta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Capacidades

Na página Parâmetros Globais, você configura o DN para o log de alterações delta e recursos LDAP adicionais. A página é pré-preenchida com as informações fornecidas pelo servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Parâmetros globais

Na página Parâmetros Globais, você configura o DN para o log de alterações delta e recursos LDAP adicionais. A página é pré-preenchida com as informações fornecidas pelo servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurar hierarquia de provisionamento

Esta página é usada para mapear o componente DN, por exemplo OU, para o tipo de objeto que deve ser provisionado, por exemplo, organizationalUnit. deixe este padrão clique em avançar

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar partições e hierarquias

Na página de partições e hierarquias, selecione todos os namespaces com objetos que você planeja importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecionar tipos de objeto

Na página de partições e hierarquias, selecione todos os namespaces com objetos que você planeja importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar atributos

Na tela Selecionar Atributos, selecione os atributos necessários para gerenciar usuários B2B. O atributo "id" é necessário

-   id

-   displayName

-   mail

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar âncoras

Com o Configurar âncora, configurar a âncora é uma etapa necessária. Por padrão, use o atributo de id para mapeamento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de conector

A página de configuração Filtro de Conector, você pode filtrar objetos com base no filtro de atributos. Neste cenário para B2B, o objetivo é apenas trazer os usuários com o userType igual a Convidado e não membro.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar regras de associação e projeção

A configuração de regras de associação e projeção são manipuladas pela regra de sincronização, não é necessário identificar uma associação e projeção no próprio conector. Deixe a configuração padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar fluxo de atributo

Assim como a associação e a projeção, não é necessário definir o fluxo de atributo, pois ele é manipulado pela regra de sincronização que será criada posteriormente. Deixe a configuração padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar desprovisionamento

Configurar o desprovisionamento permite excluir o objeto se o objeto metaverse for excluído. Neste teste, nós fazemos com que eles se desconectem, pois o objetivo é deixá-los no Azure e também não estamos exportando nada para o Azure, pois é apenas para importação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensões

Configurar extensões neste agente de gerenciamento é uma opção, mas não é obrigatório, pois estamos usando uma regra de sincronização. Se decidirmos por uma regra avançada no fluxo de atributo anteriormente, essa seria uma opção a ser definida.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criar Regras de Sincronização de Serviço MIM

Nas etapas abaixo, iniciamos o mapeamento da conta de convidado B2B e do fluxo de atributo. Supõe-se aqui que você já tem o Agente de Gerenciamento do Active Directory configurado e o agente de gerenciamento do FIM MA configurado para conversar com o Serviço e portal MIM.

Antes de criar a regra de sincronização, precisamos criar um atributo chamado userPrincipalName vinculado ao objeto person usando o MV Designer.

No cliente Sincronização, selecione Metaverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selecione o tipo de objeto de pessoa

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Em Ações, clique em Adicionar atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Por fim, preencha os seguintes detalhes

Nome do atributo: **userPrincipalName**

Tipo de Atributo: **String (Indexável)**

Indexado = **Verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

As próximas etapas exigirão a configuração mínima da configuração do FIM Service Management Agent e do Active Directory Domain Services Management Agent.

Mais detalhes podem ser encontrados aqui para a configuração <https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx> - Como Provisionar Usuários no AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de Sincronização: Importar Usuário Convidado para MV para o Metaverse do Serviço de Sincronização do Azure Active Directory<br>

Navegue para o Serviço e portal do MIM, Selecionar regras de sincronização e clique em Nova.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Selecione "Criar recurso no FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Regras de fluxo:

| **Somente o fluxo inicial** | **Usado como teste de existência** | **Fluxo (valor FIM ⇒ Atributo de Destino)**                          |
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

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regra de sincronização: Criar conta de usuário convidado para o Active Directory 

Esta regra de sincronização cria o usuário no diretório ativo

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
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra de Sincronização: Importar SID de Objetos do Usuário Convidado B2B para permitir login no MIM 

Esta regra de sincronização cria o usuário no diretório ativo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Por fim, convidamos o usuário e, em seguida, executamos o gerenciamento na seguinte ordem:

-   Importação e sincronização completas no agente de gerenciamento do MIMMA

-   Importação e Sincronização Completas no Agente de Gerenciamento ADMA_SCONTOSO_B2B

-   Importação e Sincronização Completas no B2B Graph Management Agent

-   Exportação, Importação Delta e Sincronização no Agente de Gerenciamento ADMA_SCONTOSO_B2B

-   Exportação, Importação Delta, e Sincronização no Agente de Gerenciamento MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Finalmente: Proxy de Aplicativo com um convidado B2B e Login no MIM

Agora que criamos as regras de sincronização no MIM. Na configuração do Proxy de Aplicativo, defina o uso do princípio de nuvem para permitir o KCD no proxy de aplicativo.
Além disso, em seguida, o usuário foi adicionado manualmente para o gerenciamento de usuários e grupos. As opções para não mostrar o usuário até que a criação tenha ocorrido no MIM para adicionar o convidado a um grupo do Office, uma vez provisionado, requer um pouco mais de configuração não abordada neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Somente o fluxo inicial** | **Usado como teste de existência** | **Fluxo (valor FIM ⇒ Atributo de Destino)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

Configurar uma vez

Finalmente, faça login do usuário B2B e confira o aplicativo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Próximas etapas
----------

[Como Provisionar Usuários no AD DS](https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx)

[Referência de Função para FIM 2010](https://technet.microsoft.com/en-us/library/ff800820(v=ws.10).aspx)

[Como fornecer acesso remoto seguro a aplicativos locais](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

[Baixar o agente de gerenciamento do Microsoft Identity Manager para o Microsoft Graph (visualização)](http://go.microsoft.com/fwlink/?LinkId=717495)
