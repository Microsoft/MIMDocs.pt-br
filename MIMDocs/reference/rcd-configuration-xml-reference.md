---
title: Referência de XML de configuração de exibição de controle de recurso | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 0eedee975a785bd20ee37c85262a0c5f678b09b5
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010703"
---
# <a name="resource-control-display-configuration-xml-reference"></a>Referência de XML de configuração de exibição de controle de recurso

Os recursos de Configuração de Exibição de Controle de Recurso (RCDC) são recursos definidos pelo usuário que você pode usar para controlar como os outros recursos no repositório de dados do Microsoft Identity Manager 2016 SP1 (MIM) são exibidos na interface do usuário (IU) para o usuário final. Cada recurso RCDC contém um arquivo de configuração XML que pode ser alterado para adicionar, modificar ou remover o texto e os controles da interface do usuário. Embora o MIM 2016 SP1 forneça vários recursos RCDC padrão, você também pode criar recursos RCDC personalizados. Para saber mais sobre como usar a interface de usuário do RCDC no Portal do FIM, veja [Introdução à Configuração e Personalização do Portal do FIM](/previous-versions/mim/ee534913(v=ws.10)) na documentação do FIM.


## <a name="known-issues"></a>Problemas conhecidos

Não há suporte para o valor padrão em muitos controles RCDC.

Nesta versão, não há suporte para a definição de valores padrão em controles em um controle de recurso, exceto para o controle de botão de opção. Você pode contornar esse problema para uma caixa de lista suspensa especificando um valor padrão que não esteja associado com nenhum valor para forçar o usuário a alterar a seleção. Para contornar esse problema com outros controles, você precisará usar um fluxo de trabalho de autorização para fornecer um valor padrão durante o envio da solicitação.

## <a name="basic-structure"></a>Estrutura básica

Os dados XML para um recurso RCDC consistem em um único elemento XML **ObjectControlConfiguration** .

>[!NOTE]
>Para o esquema XSD completo, consulte o <a href="#appendix-a">Apêndice A: esquema XSD padrão</a>.

Este é o esquema XSD para o elemento **ObjectControlConfiguration** :

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```

O elemento **ObjectControlConfiguration** contém os seguintes elementos:

- **ObjectDataSource**: esse elemento especifica o TypeName de uma classe de fonte de dados que usa o Controle de Recursos (RC). Para obter uma descrição e a definição de esquema, veja a próxima seção de fontes de dados neste documento. Um elemento **ObjectControlConfiguration** pode conter até 32 nós do elemento **ObjectDataSource**.

- **XmlDataSource**: é uma fonte de dados simples que é mais comumente usada para especificar o design de uma página de resumo. Para obter uma descrição e a definição de esquema, veja a próxima seção de fontes de dados neste documento. Um elemento **ObjectControlConfiguration** pode conter até 32 nós do elemento **XmlDataSource**.

- **Panel**: o administrador pode personalizar o layout da página de RCDC modificando elementos dentro dos elementos do painel. Para saber mais, veja a seção sobre o painel neste documento. Um elemento **ObjectControlConfiguration** deve ter apenas um elemento de painel.

- **Events**: os administradores não podem fornecer code-behind personalizado, esse recurso é limitado. Esse é o Event que um painel ou controle pode emitir, com base em uma alteração de estado. Para saber mais, veja a seção de eventos neste documento. Um elemento **ObjectControlConfiguration** pode conter, opcionalmente, um elemento **Event**. Em geral, o uso de **Events** personalizados não tem suporte a menos que isso seja especificamente desenvolvido em melhorias posteriores.

## <a name="data-sources"></a>Fontes de dados

O Microsoft Identity Manager usa fontes de dados como uma maneira de associar dados a componentes de interface do usuário. Isso ajuda a facilitar a separação dos dados da camada de apresentação. Há dois tipos de fontes de dados nos dados de configuração de recursos do RCDC: **ObjectDataSource** e **XmlDataSource**.

-   **ObjectDataSources** especifica uma classe .NET da Microsoft que fornece os dados para o RC. Há um conjunto fixo de tipos disponíveis de ObjectDataSources desde que o administrador possa optar por consumir ao criar RCDCs.

-   **XMLDataSources** fornece uma maneira simples de estruturar dados com base em XML e pode ser usado por administradores para fornecer dados personalizados. Os dados XML devem ser especificados diretamente no RCDC, a menos que você use a estrutura XML interna e predefinida. A estrutura XML interna é usada para gerar páginas de resumo no RC.

No RCDC, você pode vincular essas fontes de dados para atributos dos controles da interface do usuário que são especificados no RCDC para gerar a interface do usuário.

### <a name="objectdatasource-elements"></a>Elementos ObjectDataSource

O Microsoft Identity Manager fornece os tipos comuns de origem de dados na tabela a seguir que estão disponíveis para todos os tipos de recurso (exceto onde observado).

| TypeName | Descrição | Associação bidirecional | Sintaxe de associação |
|---|---|---|---|
| PrimaryResourceObjectDataSource | Isso representa o recurso do FIM 2010 que está sendo criado, exibido ou editado. O caminho na cadeia de caracteres de vinculação é o nome do atributo. O tipo de recurso é especificado pelo atributo TargetObjectType do RCDC em vez de no atributo RCDC.ConfigurationData. | Yes | `[AttributeName]` valor do atributo de objeto fornecido por seu nome. |
| PrimaryResourceDeltaDataSource  | Esta fonte de dados cria o XML delta que compara o estado original e o estado atual do recurso do FIM 2010. O XML delta gerado é consumido pelo controle do resumo do RC para renderizar a interface do usuário para a solicitação que o usuário está enviando. | Não | `DeltaXml` Isso é usado com o controle de resumo para exibir o Delta. |
| PrimaryResourceRightsDataSource | Esta fonte de dados fornece os direitos em linha para cada atributo do recurso do FIM 2010. Isso permite que o RC determine antes do envio quais permissões que o usuário tem nesse atributo de envio e, em seguida, renderiza a interface do usuário para esse atributo adequadamente. | Não | `[AttributeName]` |
| SchemaDataSource                | Esta fonte de dados pode ser usada para acessar informações relacionadas ao esquema, como nome de exibição, descrição, se o atributo é necessário, bem como informações do tipo de recurso. | Não | `[AttributeName].Required` Valor booliano que indica se o atributo deve ter um valor para ser válido. <br/> `[AttributeName].DisplayNameString` Valor que indica o nome de exibição da associação. <br/> `[AttributeName].DescriptionString` Valor que indica a descrição da associação. <br/>`[AttributeName].StringRegexString` valor que indica a Regex de cadeia de caracteres de uma associação. <br/> `[AttributeName].DisplayName` <br/> `[AttributeName].Description` <br/> `[AttributeName].IntegerValueMinimum` <br/>`[AttributeName].IntegerValueMaximum` <br/>`[AttributeName].LocalizedAllowedValues` |
| DomainDataSource                | Essa fonte de dados fornece uma enumeração de domínios, com base nos recursos de configuração de domínio. Essa fonte de dados pode ser usada somente em RCDCs que são para recursos de grupo e recursos de usuário. | Yes | Domínio |

Veja a seguir um snippet de RCDC de exemplo que associa três fontes de dados ao controle UocTextBox para editar o atributo de descrição de um grupo:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource-element"></a>Elemento XMLDataSource

Usando um elemento **XMLDataSource** , você pode especificar dados personalizados que o RCDC pode consumir para um determinado recurso. Nesse caso, os dados XML devem ser especificados no RCDC. Como alternativa, essa fonte de dados pode ser usada para fazer referência a uma estrutura de dados XML interna para renderizar a interface do usuário para páginas de resumo. Você controla o tipo de **XMLDataSource** a ser usado ao defini-lo no RCDC.


| TypeName | Descrição | Associação bidirecional | Sintaxe de associação |
|---|---|---|---|
| **XMLDataSource**            | A fonte de dados representa os dados XML. Os dados podem estar em formatos XSL ou xsl inseridos:<ul><li>Formato XSL no Microsoft.IdentityManagement.WebUI.Controls.dll:<br/>```<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement. WebUI.Controls.Resources.DefaultSummary.xsl"> </my:XmlDataSource>```</li><li>Formato XSL incorporado:<br/>```<my:XmlDataSource my:Name="RequestStatusTransformXsl"><xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform xmlns:msxsl="urn:schemas-microsoft-com:xslt"></xsl:stylesheet></my:XmlDataSource>```</li></ul>  | Não | `Xpath[;namespaces]`em que `Xpath` é um XPath XML válido para selecionar a nota necessária, com mais frequência "/" (raiz). `namespaces` é uma lista opcional de cadeias de caracteres prefix = URI. A cadeia de caracteres é delimitada por ponto e vírgula, conforme necessário para que o XPath funcione no XML de namespace. |
| **ReferenceDeltaDataSource** | A fonte de dados representa deltas de atributos de referência com vários valores. Ele é usado somente em RCDC para Group e Set. <br/> Embora a fonte de dados não esteja limitada a Groups ou Sets, ela requer alterações de código no host do RCDC para enviar esses deltas. Atualmente, Group e Set são os únicos hosts que reconhecem esta fonte de dados.  | Yes | `[AttributeName].Add` onde `[AttributeName]` representa um atributo de referência e os dados retornados são as adições Delta.<ul><li>Exemplo: `[ReferenceAttribute].Add`</li><li>Exemplo: `<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}"/>`</li></ul>`[AttributeName].Remove` onde `[AttributeName]` representa um atributo de referência e os dados retornados são as remoções de Delta. <br/> DeltaXml <!-- Is bold formatting needed for DeltaXml? --> |
|**RequestDetailsDataSource**| A fonte de dados representa o atributo RequestParameter dos objetos Request. O parâmetro define o número máximo de valores de atributo a serem exibidos por atributo de vários valores. Ele é usado somente no RCDC para Request. `<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />`| Não | DeltaXml |
|**RequestStatusDataSource**| A fonte de dados representa o atributo **RequestStatusDetails** dos objetos Request. Ele é usado somente no RCDC para Request. | Não | DeltaXml |

Para definir uma fonte de dados XML personalizada, use o seguinte XML:

 ```XML
<my:XmlDataSource my:Name="MyCustomData" >
     %Insert custom, properly formatted XML data here%
</my:XmlDataSource>
```

Para usar o XSL de controle de resumo interno, defina a fonte de dados da seguinte maneira:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

Se estiver criando um RCDC para um tipo de recurso personalizado, você poderá usar esse método para renderizar automaticamente uma página de resumo para esse recurso personalizado.

Veja a seguir um exemplo de como criar uma guia de resumo no RCDC, usando o elemento **PrimaryResourceDeltaDataSource** com o elemento **XMLDATASOURCE** usando o xsl interno:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Como alternativa, o usuário pode substituir o elemento XmlDataSource especificado anteriormente com o seguinte formato para definir um layout personalizado de uma página de resumo. Como referência, o XSL de resumo padrão do FIM 2010 está incluído no Apêndice B: XSL de resumo padrão, neste documento.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Esquema para fontes de dados
O esquema XSD a seguir gera os dois tipos de fontes de dados:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="event-element"></a>Elemento Event
Um elemento **Event** define o estado de alteração de um controle. A extensibilidade desse recurso é limitada, porque você não pode escrever uma função personalizada (manipulador) para definir qual será o comportamento depois que um evento for acionado. O mesmo elemento Event pode ser usado no elemento Panel. Para saber mais, veja a seção sobre o painel neste documento.

Este é o esquema XSD para o elemento Event:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```

Um **evento** é um elemento vazio e tem os seguintes atributos:

- **Name**: é o nome exclusivo de um evento. O único evento com suporte em **ObjectControlConfiguration** é o evento Load. Esse evento é acionado quando a página é carregada pela primeira vez.

- **Handler**: é o nome exclusivo de um manipulador. Quando o evento é acionado, geralmente um método de programa é chamado para manipular a alteração do estado do controle. Não há suporte para os seguintes casos:

   - Removendo um manipulador existente de um controle existente.
   - Criando um novo manipulador.
   - Anexando um manipulador a um controle novo ou existente.

Veja a seguir um exemplo de um elemento **Events** :

```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

## <a name="panel-element"></a>Elemento Panel
O elemento **Panel** é o elemento principal em um layout RCDC. Este é o esquema XSD para o elemento Panel:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

O elemento **Panel** contém um elemento recorrente, **GROUPING**. Para saber mais, confira a seção Agrupamento neste documento.

O elemento Panel tem os seguintes atributos:

- **Name**: o nome do Painel. Este é um atributo obrigatório do tipo cadeia de caracteres.

- **DisplayAsWizard**: esse atributo está preterido no momento. O atributo VerbContext correspondente no RCDC controla se o layout de recurso está em modo de Assistente ou Guia. Se estiver definido como 0 (modo de criação), também estará no modo de Assistente. Caso contrário, estará no modo Guia. Para saber mais, veja Introdução à Configuração e Personalização do Portal do FIM na documentação.

- **Caption**: esse atributo está preterido no momento. O usuário pode especificar as legendas para uma página incluindo um Grupo que contém apenas informações de cabeçalho. Para saber mais, confira a seção Agrupamento neste documento.

- **AutoValidate**: é um atributo booliano opcional. Quando é definido como true, a validação é disparada em cada controle na guia atual. Por padrão, se o atributo estiver ausente, ele será definido como true. Ele pode ser usado em combinação com a propriedade RegularExpression. Para saber mais, veja "RegularExpression" em uma seção posterior deste documento.

## <a name="grouping-element"></a>Elemento GROUPING
O elemento **GROUPING** define o layout geral de um painel. Ele atua como um contêiner que agrupa os controles individuais em guias e seções diferentes. Este é o esquema XSD para o elemento Grouping:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```

Há três tipos de elementos de **agrupamento** :

- **Header Grouping**: é opcional. Pode haver apenas um Header Grouping em um **Panel**. Um Header Grouping é exibido na parte superior de um painel como legenda. Somente um UocCaptionControl pode ser usado neste agrupamento. Para obter um exemplo de um Header Grouping, veja a seção Amostra.

- **Agrupamento de conteúdo**: pelo menos um agrupamento de conteúdo é necessário. Pode haver vários Content Groupings em um Panel. Um Content Grouping é exibido como o conteúdo principal de uma página do RCDC. Cada Content Grouping aparece como uma guia no mesmo Panel e pode conter de 1 a 256 controles. Consulte a seção de exemplos para obter um exemplo de um **agrupamento de conteúdo**.

- **Agrupamento de resumo**: um resumo de agrupamento é opcional. Pode haver apenas um Summary Grouping em um Panel. Um Summary Grouping aparece como a última guia de um Panel. Apenas um controle **UocHtmlSummary** pode ser usado em um Summary Grouping para exibir as alterações feitas pelo usuário antes de enviar uma solicitação. Consulte a seção de exemplos para obter um exemplo de um agrupamento de resumo.

Esta tipo de Grouping contém os seguintes elementos:

- **Ajuda**: este elemento fornece texto de ajuda em uma guia. Você também pode usá-lo para adicionar um link a um arquivo de ajuda para a guia.

- **Controls**: para saber mais sobre este elemento, veja a seção sobre controles neste documento. Cada agrupamento deve ter 1 a 256 controles inclusive, dependendo do tipo de agrupamento.

- **Events**: para saber mais sobre este elemento, veja a seção Eventos neste documento. Cada agrupamento pode, como opção, ter um Event. Os Events que têm suporte em um elemento de Grouping são os seguintes:

    - **BeforeLeave**: esse evento é acionado quando o usuário está pronto para sair de uma guia em um Content Grouping.
    - **AfterEnter**: esse evento é acionado quando o usuário está pronto para entrar em uma guia em um agrupamento de conteúdo.

Um agrupamento pode conter os sete atributos a seguir:

- **Name**: é o nome obrigatório do Grouping. O **Name** deve ser exclusivo no **Panel**.

- **Caption**: a **Caption** aparece como a legenda do cabeçalho em um Header Grouping. Ele aparece como a legenda da guia de um agrupamento Content ou Summary.

- **Description**: um atributo de cadeia de caracteres opcional, **Description** funciona apenas quando é usado em um Content Grouping. Use esse elemento para fornecer ao usuário final alguns detalhes sobre as informações dentro da mesma guia.

    >[!NOTE]
    >Se esse atributo for usado em um Summary Grouping, o XML será considerado inválido. Se esse atributo for usado em um Header Grouping, o XML será considerado válido, porém será ignorado.
    >

- **Enabled**: um atributo booliano opcional, Enabled é definido como true quando está ausente. Se habilitado for definido como false, o usuário final verá uma guia desabilitado. Esse atributo só funciona em um agrupamento de conteúdo.

    >[!NOTE]
    >Se esse atributo for usado em um Summary Grouping, o XML será considerado inválido. Se esse atributo for usado em um Header Grouping, o XML será considerado válido, porém será ignorado.
    >

- **Visible**: você pode ocultar uma guia de página do RCDC ou seu título configurando este atributo como false. Por padrão, esse atributo opcional do tipo booliano é definido como true. Esse atributo funciona somente em um Content Grouping.

    >[!NOTE]
    >Quando há apenas um Content Grouping em um Panel, esse recurso não funciona. Quando há mais de um agrupamento de conteúdo em um painel, ele se comporta conforme descrito anteriormente.

- **IsHeader**: é um atributo opcional e booliano que define se Grouping será Header Grouping. Se esse atributo não for especificado, ele será definido como false.

- **IsSummary**: é um atributo opcional e booliano que define se Grouping será Summary Grouping. Se esse atributo não for especificado, ele será definido como false.

### <a name="examples-for-types-of-grouping-elements"></a>Exemplos de tipos de elementos de agrupamento
Esta seção contém exemplos para o elemento agrupamentos.

#### <a name="example-header-grouping"></a>Exemplo: agrupamento de cabeçalho
A figura a seguir mostra um exemplo de agrupamento de cabeçalho:

![Agrupamento de cabeçalhos](media/rcd-configuration-xml-reference/image005.jpg)

O XML a seguir gera um agrupamento de cabeçalho de exemplo. No XML, o agrupamento de cabeçalho é a área com o texto de legenda "Agrupamento de cabeçalho de exemplo".

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

#### <a name="example-content-grouping"></a>Exemplo: agrupamento de conteúdo
A figura a seguir mostra um exemplo de agrupamento de conteúdo:

![Agrupamento de conteúdo](media/rcd-configuration-xml-reference/image007.jpg)

O XML a seguir gera um agrupamento de conteúdo de exemplo. No XML, o agrupamento de conteúdo é a área com o texto de legenda "Agrupamento de conteúdo de exemplo".

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

#### <a name="example-summary-grouping"></a>Exemplo: agrupamento de resumo

A figura a seguir mostra um exemplo de agrupamento de Resumo:

![Agrupamento de resumo](media/rcd-configuration-xml-reference/image010.jpg)

O XML a seguir gera um exemplo de agrupamento de resumo. No XML, o agrupamento de resumo é a área com o texto de legenda "amostra de agrupamento de resumo".

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```

### <a name="help-element"></a>Elemento Help
O elemento **Help** pode ser incluído em um agrupamento ou um elemento Control como um elemento opcional. Se ele for usado em um Grouping, deverá deve ser o primeiro elemento usado. Ele fornece ajuda textual para os usuários finais para ajudá-los a fornecer informações precisas. O seguinte esquema XSD é para o elemento Help:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```

O código de exemplo XML a seguir gera um elemento Help:

```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control-element"></a>Elemento Control
Um elemento GROUPING contém um ou mais elementos **Control** . Controls são os principais elementos em um RCDC. Você pode personalizar o elemento Grouping definindo os diferentes elementos Control que ele contém. O seguinte esquema XSD é para o elemento Control:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Um elemento Control contém os seguintes elementos:

- **Help**: esse elemento é ignorado. Ele funciona somente no Grouping.

- **CustomProperties**: esse elemento não tem suporte.

- **Options**: esse elemento é usado apenas em combinação com os controles **UocDropDownList** ou **UocRadioButtonList**. Não é funcional com outros controles. Veja a seção Opções neste documento para a estrutura desse elemento. Consulte a seção de controles individuais deste documento para ver como as opções são usadas por um controle.

- **Buttons**: esse elemento é usado apenas em combinação com o controle **UocListView**. Não é funcional para outros controles. Para saber mais, confira a seção UocListView neste documento.

- **Propriedades**: esse elemento é usado em todos os controles para especificar comportamentos adicionais de um controle. Para saber mais sobre este elemento, veja a seção Propriedades neste documento.

- **Events**: para a estrutura deste elemento, veja a seção Eventos anteriormente neste documento. Consulte a seção de controles individuais deste documento para ver quais eventos são usados em um controle.

Um elemento Control pode conter os 10 atributos a seguir:

- **Name**: é o nome do controle. O nome de um Control deve ser exclusivo dentro de cada painel. Este é um atributo obrigatório do tipo cadeia de caracteres.

- **TypeName**: esse atributo especifica o tipo de Control. Este é um atributo obrigatório do tipo cadeia de caracteres. Veja a seção Controles individuais neste documento para cada nome de controle.

- **Caption**: você pode usar esse atributo para incluir uma legenda para o controle. A legenda é geralmente o nome para exibição dos dados que o controle está exibindo ou inserindo. Você pode especificar um valor para a legenda explicitamente ou associá-lo a informações de nome de exibição de atributo do esquema. A legenda é exibida mais à esquerda de um controle de tamanho normal. Se um controle ocupa a tela inteira, a legenda é exibida sobre o controle. Este é um atributo opcional de tipo de cadeia de caracteres. Para saber mais sobre como vincular uma fonte de dados a um atributo ou um valor de propriedade, veja a seção sobre propriedades.

    O exemplo a seguir mostra como uma legenda pode ser usada explicitamente:

    ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
    ```

    O exemplo a seguir mostra como uma legenda pode ser usada com uma fonte de dados. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é o esquema. É recomendável vincular o DisplayName do atributo a um atributo Caption.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```

- **Enabled**: é um atributo opcional de tipo booliano. Ao definir o valor desse atributo como false, o usuário poderá desabilitar o Control. O valor padrão é definido como true.

- **Visible**: é um atributo opcional de tipo booliano. Você pode usar esse atributo para ocultar todo o controle. O valor padrão é definido como true.

- **Descrição**: Use esse atributo opcional de tipo de cadeia de caracteres para incluir uma descrição para ajudar o usuário final a entender o que eles devem colocar no controle ou o que o controle faz. Você pode especificar um valor para a descrição explicitamente ou associá-la a informações de descrição de atributo do esquema.

    Description é exibida mais à esquerda de um controle de tamanho normal embaixo da legenda. Se um controle ocupa a tela inteira, a descrição aparece na parte superior do controle abaixo da legenda. Para saber mais sobre como vincular uma fonte de dados a um atributo ou um valor de propriedade, veja a seção Propriedades neste documento.

    O exemplo a seguir mostra como uma descrição pode ser usada explicitamente:

    ```XML
    <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
    ```

    O exemplo mostra como uma descrição pode ser usada com uma fonte de dados. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é o **esquema**. É recomendável vincular a **Description** do atributo a um atributo Description.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
    ```

- **ExpandArea**: esse atributo indica se o controle abrange a tela inteira. Esse é um atributo opcional de tipo booliano. O valor padrão é definido como false.

    >[!NOTE]
    >Os atributos Caption e Description são desabilitados quando esse atributo é definido como true. Use o controle UocLabel para fornecer uma legenda para um controle expandido.
    >

- **Hint**: é um atributo opcional de tipo de cadeia de caracteres. O texto no atributo Hint ajuda o usuário final a decidir o que é uma entrada válida para o controle. A Hint é exibida sob o controle.

- **AutoPostback**: é um atributo opcional de tipo booliano. O valor padrão é false. Se estiver definido como false, atualizar a página poderá não atualizar o controle. Para saber mais sobre AutoPostback, procure a propriedade de controle da interface de usuário do Microsoft ASP.NET do mesmo nome.

- **RightsLevel**: é um atributo opcional de tipo de cadeia de caracteres. Você pode vincular este atributo somente com direitos em linha com uma fonte de dados. O controle é dinamicamente ativado ou desativado, com base nos direitos do usuário. Para saber mais sobre como vincular uma fonte de dados a um atributo ou um valor de propriedade, veja a seção Propriedades neste documento.

    O exemplo mostra como um atributo **RightsLevel** pode ser usado com uma fonte de dados. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é **rights**. Use o nome do atributo como o Path.
    <!--- no example provided -->

### <a name="property-element"></a>Elemento Property
Você pode usar um elemento de **Propriedade** para personalizar ainda mais o comportamento de cada controle. Uma Property é um elemento vazio. O seguinte esquema XSD é para o elemento Property:

```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```

Cada propriedade tem os dois atributos obrigatórios a seguir:

- **Name**: esse atributo de tipo de cadeia de caracteres é o nome exclusivo da Property. Controles diferentes têm propriedades diferentes. Há algumas propriedades comuns que podem ser usadas por todos os controles. Para obter mais informações sobre quais nomes estão disponíveis para um determinado controle, consulte as seções Propriedades comuns e controles individuais deste documento.

- **Value**: especifica o valor da Property. O tipo de dados do valor depende de a qual propriedade ele é atribuído. Veja a seção a seguir para o formato do valor permitido para propriedades específicas.


#### <a name="bind-property-with-data-source-content"></a>Associar Propriedade ao conteúdo da fonte de dados
Algumas propriedades podem ser associadas a informações de uma fonte de dados. Use o formato de cadeia de caracteres a seguir para fazer essa associação. Consulte a descrição das propriedades individuais na seção controles individuais deste documento para saber como associar Propriedades a uma fonte de dados.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

   Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}

   SourceExpression:= “Source=” + [ObjectDataSourceName]

   PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

   ModeExpression:= “Mode=” + [ModeChoice]

   ModeChoice:= “OneWay”|”TwoWay”

   ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

   AttributeName:= valid schema attribute name from the data source.

   AttributePropertyName:= valid property name of a schema attribute from the data source.
```

O XML a seguir mostra como associar uma fonte de dados a um elemento de **Propriedade** :

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
```


<h2 id="common-properties">Propriedades comuns</h2>

Todos os controles RCDC especificados neste documento podem ter as propriedades comuns descritas nesta seção. Você pode usar essas propriedades junto com outras propriedades que são específicas para diferentes controles.

- **Obrigatório**: essa propriedade indica que o campo é um campo obrigatório ou um campo opcional. Um campo obrigatório deve ser preenchido com um valor. Não há suporte para um valor vazio para entrada de cadeia de caracteres. Um campo opcional pode ser deixado vazio. Se esse é um campo obrigatório sem valor preenchido, será exibida uma mensagem de erro no alto do controle de entrada. Você pode especificar explicitamente se um campo é obrigatório ou opcional. Você também pode vincular com as informações de esquema de uma determinada vinculação entre um atributo e um tipo de recurso. Por padrão, se essa propriedade estiver ausente, isso significará que o controle é de entrada opcional.

    O exemplo a seguir usa um valor explícito para esta propriedade:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```

    Este é um exemplo que usa uma fonte de dados dinâmico para essa propriedade. Se você usou o modelo para uma fonte de dados exibida na seção anterior deste documento, sua fonte de dados é o esquema. Use `<attribute name>.Required` como o caminho.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```

- **ReadOnly**: ao definir essa propriedade como true, o usuário final apresentará o controle em um modo somente leitura. Esse é um atributo opcional de tipo booliano. O valor padrão é definido como false. No entanto, às vezes, o comportamento dessa propriedade é substituído pelo tipo de direitos que uma pessoa tem sobre a vinculação de dados com o controle. Por exemplo, se um usuário não tem direitos para atualizar um campo e o campo está vinculado com direitos em linha, o usuário vê os dados em um modo somente leitura mesmo que essa propriedade seja definida como false.

- **RegularExpression**: essa propriedade especifica as restrições que são impostas no valor do controle. Os formatos do valor dessa propriedade são os formatos com suporte no padrão StringRegex do .NET. Para obter mais informações, consulte [Expressões regulares do .NET Framework](https://go.microsoft.com/fwlink/?LinkId=165361). Se o controle for usado para inserir um valor, o valor será verificado em relação à restrição especificada nessa propriedade quando o usuário tentar sair da página atual. A mensagem de erro é exibida na parte superior do controle que tem uma entrada inválida. O usuário pode especificar explicitamente uma expressão regular de cadeia de caracteres. O usuário também pode vinculá-lo com as informações de esquema de um determinado atributo. Por padrão, se essa propriedade estiver ausente, isso significará que o controle não verifica se há restrições nas cadeias de caracteres de entrada.

    O exemplo a seguir usa um valor explícito para esta propriedade:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```

    Este é um exemplo que usa uma fonte de dados dinâmico para essa propriedade. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é o esquema. Use o `<attribute name>.StringRegex` como o caminho.

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```

- **Visible**: é um atributo opcional de tipo booliano. Você pode usar esse atributo para ocultar todo o controle. O valor padrão é definido como true.


<h3 id="options-element">Elemento Options</h3>

O elemento options inclui um ou mais subnós de **opção** . O elemento **Options** é usado somente com os controles **UocRadioButtonList** e **UocDropDownList** . Para obter detalhes sobre como usar esses controles, consulte a seção de controles individuais deste documento.

O seguinte esquema XSD é para o elemento Options:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```

O elemento **Options** tem os seguintes atributos:

- **Value**: é um atributo necessário de tipo de cadeia de caracteres. O atributo de valor deve ser exclusivo dentro do mesmo controle. Somente podem ser usados caracteres que distinguem maiúsculas e minúsculas de A a Z.

- **Legenda**: esse atributo obrigatório é o nome de exibição de cada opção.

- **Dica**: é um atributo opcional. Use esse atributo para fornecer mais informações e dicas para o usuário final.


## <a name="environment-variables"></a>Variáveis de ambiente

As seguintes variáveis de ambiente podem ser usadas em qualquer configuração de RCDC:

| Variável | Descrição |
|---|---|
| `<LoginID>`       | Exibe a ID do usuário que está conectado no momento.           |
| `<LoginDomain>`   | Exibe o domínio do usuário que está conectado no momento.       |
| `<Today>  `       | Exibe a data e hora atuais                                |
| `<FromToday_nnn>` | Exibe a data atual, mais `nnn` e a hora, em que `nnn` é um número inteiro.  |
| `<ObjectID> `     | A ID do recurso principal do RCDC.                                     |
| `<Attribute_xxx>` | Retorna um atributo especificado, xxx, do recurso primário do RCDC. |


## <a name="debug-xml-configuration-files"></a>Depurar arquivos de configuração XML

Ao desenvolver ou modificar arquivos de configuração XML para um RCDC, você pode ajudar a reduzir erros Validando o XML em relação aos arquivos XSD usando um editor como o Microsoft Visual Studio. Para saber mais, veja [Uma introdução às ferramentas de XML no Visual Studio 2005](https://go.microsoft.com/fwlink/?LinkID=74512).


## <a name="customize-help-files"></a>Personalizar arquivos de ajuda

Se você criar novos recursos e atributos, convém atualizar os arquivos de ajuda existentes no Portal do FIM com conteúdo para seus recursos personalizados. Os arquivos de Ajuda no Portal do FIM estão no formato .htm e eles podem ser editados manualmente. Para saber mais sobre como criar atributos personalizados, veja Introdução ao recurso personalizado e gerenciamento de atributos na documentação do FIM 2010.

>[!IMPORTANT]
>As informações sobre os conceitos básicos de formatação ou edição de HTML não são fornecidas neste artigo. Espera-se que os usuários saibam como editar arquivos HTML.

### <a name="location-of-help-files"></a>Local dos arquivos de ajuda
Todos os arquivos de ajuda para o portal do Microsoft Identity Manager 2016 SP1 estão localizados na pasta `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html` no servidor do serviço do mim.

### <a name="locate-a-specific-help-file"></a>Localizar um arquivo de ajuda específico
Todos os arquivos de ajuda para o portal do FIM são nomeados com um GUID (identificador global exclusivo). Para localizar o arquivo correto para o recurso personalizado:

1. No Portal do FIM, abra o arquivo de Ajuda na página do Portal que você deseja personalizar.

2. Clique com o botão direito do mouse no arquivo de ajuda e selecione **Propriedades**.

3. Realce e copie o `<GUID\>.htm` arquivo no campo **endereço da URL** .

4. Navegue até a pasta em que os arquivos de ajuda estão armazenados e pesquise o arquivo.


## <a name="add-content-for-attribute-in-existing-grouping-element"></a>Adicionar conteúdo para o atributo no elemento de Agrupamento existente
Para adicionar conteúdo descritivo para um novo atributo dentro de um elemento de Agrupamento existente (guia):

1. Identifique e localize o arquivo de Ajuda apropriado.

2. Usando um editor de texto, abra o arquivo.

3. Localize onde você deseja adicionar o conteúdo. Normalmente, isso acontece em um parágrafo adicional, por exemplo:

    `<p xmlns="">A new paragraph with customized information.</p>`

    Ele também pode ser um item inserido em uma lista existente, por exemplo:

    ```
    <li class="unordered"><b>First Name</b> – The first name of the User.<br>
    <li class="unordered"><b>Last Name</b> - The last name of the User.<br>
    <li class="unordered"><b>Added a new line</b><br>
    ```

## <a name="add-content-for-existing-grouping-element"></a>Adicionar conteúdo para o elemento de Agrupamento existente
A maioria das páginas do portal do FIM tem vários elementos de agrupamento (ou guias), e os arquivos de ajuda que o acompanham têm seções com indicadores relacionadas a cada elemento de agrupamento. Os indicadores no HTML estão especificados nas seções. Por exemplo, este é o HTML para a guia Informações de Trabalho do arquivo de Ajuda para a página Criar Usuário no Portal do FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Ele é referenciado pelo elemento Grouping **WorkInfo** no arquivo de configuração XML de dados para a **Configuração para a criação de usuário** do RCDC. O `\<GUID\>.htm` nome do arquivo e o indicador são especificados no `my:Link` parâmetro:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

### <a name="simple-control-samples"></a>Exemplos de controle simples
Esta seção fornece exemplos para a criação de controles de caixa de texto simples diferentes.

A figura a seguir mostra alguns controles de caixa de texto simples em modos diferentes:

![Controles de caixa de texto simples](media/rcd-configuration-xml-reference/image016.gif)

O segmento de código a seguir cria o primeiro controle de caixa de texto, que usa texto explícito para todos os atributos e propriedades:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->
```

O segmento de código a seguir cria o segundo controle de caixa de texto, que usa a técnica de vinculação dinâmica para vincular o controle com uma fonte de dados diferente:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```

O segmento de código a seguir cria o terceiro rótulo expandido e o controle de caixa de texto:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

O segmento de código a seguir cria o quarto controle de caixa de texto desabilitado. Embora esse controle não exiba uma diferença visível entre o estado desabilitado e o estado habilitado, o usuário não pode inserir dados na caixa de texto.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```


## <a name="individual-controls"></a>Controles individuais

Esta seção documenta os controles individuais que são fornecidos com o Microsoft Identity Manager 2016 SP1.

### <a name="uocbutton"></a>UocButton

**Nome**: UocButton

**Descrição**: é um controle de botão simples que pode ser usado para acionar determinadas ações. No entanto, como você não pode especificar seu próprio manipulador, o uso deste controle é limitado.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **Texto**: essa propriedade especifica o texto que aparece no botão. Este é um atributo opcional de tipo de cadeia de caracteres. O texto utiliza um valor explícito de cadeia de caracteres.

**Eventos**:

- **OnButtonClicked**: o evento é emitido quando o botão é clicado.

**Exemplo**:

![Controle UocButton](media/rcd-configuration-xml-reference/image017.png)

O seguinte segmento XML produz um botão de controle UocButton simples:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```


### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Nome**: UocCaptionControl

**Descrição**: esse controle é usado para exibir a legenda de uma página do RCDC. Esse controle é projetado para ser usado apenas como um único controle em um agrupamento de cabeçalho. Usá-lo em qualquer outro contexto pode causar problemas de renderização ou erros de portal.

**Modo**: somente leitura (unidirecional)

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **MaxHeight:** essa propriedade especifica a altura máxima do ícone na seção de legenda. Essa propriedade é opcional. Essa propriedade utiliza um valor inteiro em pixels. O valor padrão é 32 pixels.

**Eventos**:

- Não há eventos para este controle.

**Exemplo**:

![Controle UocCaptionControl](media/rcd-configuration-xml-reference/image020.jpg)

O segmento de código a seguir gera uma **legenda de cabeçalho**:

```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->
```

O segmento de código a seguir gera uma **legenda de conteúdo explícita**:

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

O segmento de código a seguir gera uma legenda dinâmica de **nome de exibição** :

```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```


### <a name="uoccheckbox"></a>UocCheckBox

**Nome**: UocCheckBox

**Descrição**: é um controle de caixa de seleção simples. É recomendável que o usuário associe esse controle a dados de tipo booliano. Este controle pode ser usado como um controle somente leitura ou um controle atualizável, com base nos dados ao qual ele está associado.

>[!NOTE]
>Nesta versão, ao usar o controle da caixa de seleção no modo de edição para exibir um atributo booliano, se o atributo não tiver um valor anteriormente atribuído a ele, o Controle de Recurso adicionará um valor **false** ao atributo quando **OK** for clicado no modo de edição. A solução alternativa é sempre criar um atributo booliano que assuma que a não existência seja igual a **false** ou usar outros controles, como um botão de opção para atributos boolianos.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **DefaultValue**: é uma propriedade opcional de tipo booliano. O valor padrão é definido como false. Este campo especifica o comportamento padrão de uma caixa de seleção. Isso pode ser especificado explicitamente.

- **Checked**: é uma propriedade opcional de tipo booliano. O valor padrão é definido como false. Esse valor substitui a propriedade DefaultValue quando ela estiver presente junto com DefaultValue. Este campo especifica o comportamento de uma caixa de seleção. Como DefaultValue, isso pode ser especificado explicitamente ou vinculado a dados do servidor.

- **Text**: é um atributo opcional de tipo de cadeia de caracteres. O texto é mostrado no lado direito da caixa de seleção. Você pode usar essa propriedade para especificar o texto que fornece mais informações para o usuário final.

**Eventos**:

- **CheckedChanged**: quando a caixa de seleção altera seu estado, esse evento é emitido.

**Exemplo**:

No exemplo a seguir, uma vinculação personalizada é criada entre o tipo de recurso personalizado e o atributo **IsConfigurationType**. O XML é usado no RCDC de um tipo de recurso personalizado.

![Controle UocCheckBox](media/rcd-configuration-xml-reference/image022.png)

O segmento de código a seguir produz uma **caixa de seleção dinâmica**, conforme mostrado como Caixa de Seleção Dinâmica na figura anterior. Esse tipo de associação é mais versátil e útil do que uma caixa de seleção explícita. O atributo deve pertencer ao tipo de recurso atual.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Nome**: UocCommonMultiValueControl

**Descrição**: é um controle de caixa de texto de várias linhas que dá suporte à formatação de cadeia de caracteres especial. Cada valor entre as entradas com valores variados é separado por um ponto-e-vírgula (;) ou uma quebra de linha na caixa de texto. É recomendável associar esse controle com dados de tipos de número inteiro, Cadeia de caracteres curta e de valores inteiros. Esse controle oferece suporte ao modo somente leitura e modo atualizável.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **DataType**: é um atributo obrigatório do tipo cadeia de caracteres. Você pode especificar isso como um tipo **String, Integer** ou **DateTime** explicitamente. Você também pode vincular o atributo à propriedade **DataType** do atributo do esquema. Um tipo de referência com vários valores deve ser tratado pelo **UOCListView** ou **UOCIdentityPicker**. Booliano de múltiplos valores não é um tipo de dados com suporte.

- **Rows**: é um atributo opcional de tipo de inteiro. Você pode definir a altura da caixa no número de caracteres. O valor padrão é definido como 1.

- **Columns**: é um atributo opcional de tipo de inteiro. Você pode definir a largura da caixa no número de caracteres. O valor padrão é definido como 20.

- **Value**: é um atributo opcional de tipo de cadeia de caracteres. Você pode vincular este atributo somente com fonte de dados.

**Eventos**:

- **ValueListChanged**: esse evento é disparado quando o valor atual no controle é alterado.

**Exemplo:**

No exemplo a seguir, um atributo de cadeia de caracteres de vários valores chamado **AMultiValueString** é criado e vinculado ao tipo de recurso personalizado. Este exemplo funciona apenas depois que essa vinculação é criada.

![Controle UocCommonMultiValueControl](media/rcd-configuration-xml-reference/image024.jpg)

O segmento de código a seguir gera um controle **UocCommonMultiValueControl** :

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```


### <a name="uocdatetimecontrol"></a>UocDateTimeControl

**Nome**: UocDateTimeControl

**Descrição**: é semelhante a um controle de caixa de texto, mas **Descrição** aceita apenas um determinado formato. No modo somente leitura, ela aparece como um rótulo. Para o formato da cadeia de caracteres de entrada com suporte, veja a propriedade **DateTimeFormat** nesta seção.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **DateTimeFormat**: é um atributo opcional de tipo de cadeia de caracteres. Os formatos com suporte são **DateTime** e **somentedata**. O valor padrão é definido como o formato **DateTime** .

  - **DateTime**: o atributo é formatado como mm/dd/aaaa hh: mm: SS am.
  - **Somentedata**: o atributo é formatado como mm/dd/aaaa.

    >[!NOTE]
    >Os formatos **DateTime** e **somentedata** têm suporte, independentemente do usuário que está especificando a diferença.
    >

- **Value**: é um atributo opcional de tipo de cadeia de caracteres. Você associa esse atributo a uma fonte de dados de recursos. O valor desse atributo deve estar de acordo com o formato de data e hora correto.

**Eventos**:

- **Datetimechanged**: quando o valor de DateTime é alterado, o evento ocorre.

**Exemplo**:

![Controle UocDateTimeControl](media/rcd-configuration-xml-reference/image027.jpg)

O segmento de código a seguir produz o primeiro controle **DateTime**.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```

O segmento de código a seguir produz o segundo controle **DateTime**. Se você tiver usado o código de exemplo na seção de fontes de dados, o atributo **ExpirationTime** será associado a todos os tipos de recurso. Portanto, você pode usá-lo com o código a seguir:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```


### <a name="uocdropdownlist"></a>UocDropDownList

**Nome**: UocDropDownList

**Descrição**: é um controle simples de caixa suspensa. Esse controle é usado para selecionar opções de um conjunto definido de opções. Tipos de dados de cadeia de caracteres, inteiro, datetime e booliano são bons candidatos para este controle.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **ValuePath**: a propriedade para obter o atributo Valor de ItemSource. Quando ItemSource é especificado como personalizado, o caminho de valor é definido como Value. Ele é associado com o campo Value do elemento Option, conforme descrito nesta seção.

- **CaptionPath**: a propriedade para obter o atributo Valor de ItemSource. Quando ItemSource é especificado como personalizado, o caminho de valor é definido como Caption. Ele é associado com o campo Caption do elemento Option, conforme descrito nesta seção.

- **HintPath**: a propriedade para obter o atributo Valor de ItemSource. Quando ItemSource é especificado como personalizado, o caminho de valor é definido como Hint. Ele é associado com o campo de dica do elemento Option, conforme descrito nesta seção.

- **ItemSource**: uma coleção de ListControlItems que define as opções na lista. O usuário pode definir isso explicitamente como personalizado e usar o elemento Option, conforme descrito nesta seção, para especificar o valor da cadeia de caracteres.

- **SelectedValue**: o valor selecionado no momento. Esta é uma propriedade obrigatória do tipo cadeia de caracteres. Essa propriedade é vinculada com dados de cadeia de caracteres da fonte de dados.

**Eventos**:

- **SelectedIndexChanged**: o evento ocorre quando a seleção na caixa suspensa é alterada.

**Opções**:

Para a estrutura de um elemento **Options** , consulte <a href="#options-element">elemento Options</a>.

- **Value**: o valor de um único elemento Options pode ser definido como qualquer cadeia de caracteres que é a entrada válida da fonte de dados à qual o controle é associado.

- **Caption**: pode ser qualquer valor de cadeia de caracteres.

- **Hint**: pode ser qualquer valor de cadeia de caracteres.

**Exemplo**:

![Controle UocDropDownList](media/rcd-configuration-xml-reference/image030.jpg)


![Opções em um controle UocDropDownList](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
>Para que a amostra funcione, você deverá vincular um atributo **Scope** existente de tipo de cadeia de caracteres com o tipo de recurso personalizado ao qual o RCDC se aplica.


O segmento de código a seguir gera uma lista suspensa:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

**Nome**: UocFileDownload

**Descrição**: esse controle contém um hiperlink. Quando o hiperlink é clicado, aparece uma página Salvar Arquivo do Windows. O usuário pode salvar o arquivo em sua unidade local. Também há suporte para a opção Open se o Internet Explorer puder renderizar o formato de arquivo. Os tipos de dados recomendados com os quais usar este controle são cadeia de caracteres formatada (XML) e tipos binários.

>[!NOTE]
>Nesta versão do Microsoft Identity Manager 2016 SP1, o usuário deve fechar a janela do Internet Explorer na qual ele abriu o arquivo e, em seguida, atualizar a página. Depois de atualizar a janela do Internet Explorer, o usuário pode iniciar o download para salvar ou abrir o mesmo arquivo novamente na janela original.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **Text**: é um atributo opcional de tipo de cadeia de caracteres que define o texto do hiperlink. O usuário pode especificar uma cadeia de caracteres explícita para essa propriedade.

- **Value**: é um atributo obrigatório. Especifica a vinculação de atributo no servidor cujo conteúdo deve ser baixado.

- **PromptedFileName**: é um atributo opcional de tipo de cadeia de caracteres. Este é o nome do arquivo que é sugerido para o usuário ao salvar o arquivo baixado.

- **ContentType**: é um atributo obrigatório do tipo cadeia de caracteres. Esse é o tipo de arquivo no qual os dados são salvos. Texto ou binário são as duas opções de cadeia de caracteres com suporte. Se for texto, o valor de retorno será considerado como uma cadeia de caracteres longa. Caso contrário, para binário, o valor de retorno será considerado como byte[]. Se o texto for selecionado, o usuário poderá, como opção, adicionar um sufixo para especificar o tipo de formato no qual o texto está. Por exemplo, text/xml é válido.

>[!NOTE]
>Quando o valor que está associado a este controle está vazio, o controle não tem o hiperlink para ser usado para disparar a ação de download. Isso ocorre porque não há nada para baixar.

**Eventos**:

- Não há eventos para este controle.

**Exemplo**:

![Controle UocFileDownload](media/rcd-configuration-xml-reference/image035.png)

>[!NOTE]
>Antes de carregar esse arquivo de amostra, o usuário deve criar uma vinculação entre um tipo de recurso personalizado e o atributo ConfigurationData existente.

O segmento de código a seguir gera um controle de download de arquivo:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Nome**: UocFileUpload

**Descrição**: esse controle contém uma caixa de texto que exibe a localização do arquivo local para ser carregado, um botão de arquivo de navegação e um botão de carregamento. Quando o usuário final clica em um botão de procura, é exibida uma janela Abrir Arquivo do Windows. O usuário final pode selecionar um arquivo em seu disco local para carregar. Quando o arquivo é selecionado, a localização do arquivo é mostrada na caixa de texto. Quando o botão Carregar é clicado, o arquivo é carregado para a fonte de dados local do cliente. O conteúdo do arquivo ainda não foi enviado para o servidor. Os tipos de dados recomendados com os quais usar este controle são os seguintes: cadeia de caracteres formatada (XML) ou tipos binários.

>[!NOTE]
>Não há nenhuma indicação do andamento ou status do upload. Quando o arquivo for carregado para a fonte de dados local, a caixa de texto estará desmarcada.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **Value**: é um atributo obrigatório. Especifica a vinculação de atributo de esquema no servidor ao qual os dados são carregados.

- **ContentType**: é um atributo opcional de tipo de cadeia de caracteres. Esse é o tipo de dados em que o arquivo é salvo no servidor. Isso pode ser definido para Texto ou Binário. Quando a propriedade estiver ausente, o valor padrão será Binário.

- **MaxFileSize**: é um atributo opcional de tipo de cadeia de caracteres. MaxFileSize define a quantidade de tamanho do arquivo carregado. Por padrão, se a propriedade estiver ausente, o tamanho máximo será 1 megabyte (MB).

- **PromptedForNoValue**: é um atributo opcional de tipo de cadeia de caracteres. Ele define o texto que aparece para o usuário quando um arquivo não está sendo carregado.

**Eventos**:

- **Fileuploadd**: esse evento é emitido quando o arquivo é carregado com êxito.

**Exemplo**:

![Controle UocFileUpload](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
>Para fazer o seguinte código de exemplo funcionar, você deverá criar um novo atributo de tipo binário denominado ABinaryAttribute e, em seguida, criar uma nova associação entre um tipo de recurso personalizado e esse atributo.

O segmento de código a seguir gera um controle de carregamento:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```


### <a name="uocfilterbuilder"></a>UocFilterBuilder

**Nome**: UocFilterBuilder

**Descrição**: é um controle complexo que permite ao usuário renderizar uma expressão XPath do mim 2016. Algumas expressões XPath não são aceitas. Para saber mais sobre como usar o construtor de filtro, veja a Ajuda para o construtor de filtro.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **PermittedObjectTypes**: define uma lista de tipos de recursos a serem mostrados na instrução SELECT de um construtor de filtro. Para saber mais sobre como usar o construtor de filtro, veja a Ajuda do construtor de filtro. A cadeia de caracteres está no formato de ResourceTypea, ResourceTypeB, em que cada tipo de recurso é separado por uma vírgula ', '.

- **Valor**: esse é o valor com o qual o construtor de filtro é renderizado. Há suporte para apenas uma vinculação com dados de tipo de cadeia de caracteres que contém uma expressão XPath. O atributo Filter é um atributo recomendado para vincular este controle.

- **PreviewButtonVisible**: é uma propriedade opcional de tipo booliano. Quando essa propriedade é definida como false, o usuário não vê um botão de visualização. O valor padrão é definido como true. Esse botão pode ser usado em combinação com um controle de exibição de lista para visualizar os resultados de uma expressão XPath.

- **ExcludeGroupMembership**: é uma propriedade booliana. Quando essa propriedade é definida como true, você não pode criar um filtro que usa \<Reference Attribute\> (por exemplo, ResourceId) é membro de \<Group object\> . Em outras palavras, quando essa propriedade é definida como true, você não pode criar um filtro que usa o diretório de associação de grupo.

- **PreviewButtonCaption**: é uma cadeia de caracteres opcional. Quando PreviewButtonVisible está definido como true, você pode usar essa propriedade para dar ao botão um texto personalizado. O texto é exibido no botão de visualização.

**Eventos**:

- **OnFilterChanged**: esse evento é disparado quando o conteúdo do construtor de filtro é alterado.

**Exemplo**:

![Controle UocFilterBuilder](media/rcd-configuration-xml-reference/image044.png)

O código de exemplo a seguir inclui um controle UOCLabel, um construtor de filtro simples com PermittedObjectTypes e um modo de exibição de lista de visualização. Aponte a propriedade ListFilter de exibição de lista e a propriedade valor do construtor de filtro para o mesmo atributo de fonte de dados para vincular os dois.

>[!NOTE]
>Antes de usar esse código de exemplo, crie uma nova associação entre um atributo Filter existente e um tipo de recurso personalizado.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


### <a name="uochtmlsummary"></a>UocHtmlSummary

**Nome**: UocHtmlSummary

**Descrição**: você pode usar esse controle para definir uma página de resumo em uma página do RCDC. Essa página de resumo aparece depois que o usuário final envia uma solicitação. Este controle só pode ser usado em um Summary Grouping e ele deve ser o único controle. É altamente recomendável que você use o código de exemplo fornecido.

>[!NOTE]
>Esse controle não foi testado extensivamente.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **ModificationsXml**: essa propriedade deve ser formatada como {Binding origem = Delta, Path = DeltaXml}, em que Delta é definido no cabeçalho de configuração ObjectDataSource.

- **TransformXsl**: essa propriedade é formatada como {Binding origem = SummaryTransformXsl, Path =/}, em que summaryTransformXsl é definido no cabeçalho de configuração XmlDataSource.

**Eventos**:

- Não há eventos para este controle.

**Exemplo**:

Para obter um exemplo desse controle, consulte o exemplo de um agrupamento de resumo na seção elemento de agrupamento deste documento.


### <a name="uochyperlink"></a>UocHyperLink

**Nome**: UocHyperLink

**Descrição**: é um simples controle de hiperlink. Você pode usar este controle para exibir informações como um hiperlink.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **ObjectReference**: é uma propriedade opcional de tipo de referência. Se um recurso válido estiver referenciado pelo GUID que é definido nesta propriedade, o hiperlink fornecerá ao usuário final uma maneira de acessar o recurso. Isso é mutuamente exclusivo com a propriedade NavigateUrl.

- **Text**: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode usar essa propriedade para definir o texto que aparece como o hiperlink.

- **NavigateUrl**: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode usar essa propriedade para definir a URL de caminho completo ao qual o hiperlink está vinculado. Isso é mutuamente exclusivo com a Propriedade ObjectReference.

**Eventos**:

- Não há eventos para este controle.

**Exemplo**:

![Controle UocHyperLink](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
>Você precisa de um GUID válido de um recurso ao qual vincular. Nesse caso, o segundo hiperlink é gerado com um GUID válido. O primeiro deles pode ser qualquer site da Web.

O segmento de código a seguir gera um hiperlink de redirecionamento:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->
```

O segmento de código a seguir gera um hiperlink que faz referência a um recurso. A referência explícita pode ser substituída pela expressão {Binding Source=object, Path=Creator} para vincular isso a uma fonte de dados. Isso pode ser válido somente quando o gerenciador do recurso existe e é do valor de tipo de referência.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```


### <a name="uocidentitypicker"></a>UocIdentityPicker

**Nome**: UocIdentityPicker

**Descrição**: esse controle consiste em uma caixa de resolução opcional e uma janela de procura. A caixa Resolver opcional consiste em uma caixa de texto opcional para inserir a identidade, um botão Resolver para resolver a identidade e um botão Procurar para solicitar uma janela pop-up de procura. A janela Procurar possibilita que o usuário selecione identidades por meio de um controle de exibição de lista. A identidade selecionada na janela Procurar é refletida na caixa de Resolver.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **UsageKeywords**: é uma propriedade de cadeia de caracteres opcional. Você pode definir uma lista de escopos de pesquisa a serem usados no seletor de recursos fornecendo uma lista das palavras-chave de uso com suporte na estrutura SearchScopeConfiguration, em que cada palavra-chave é separada por um apóstrofo (').

- **Filter**: é uma propriedade de cadeia de caracteres opcional. O usuário fornece uma expressão XPath para definir o escopo do seletor de recurso para exibir apenas os itens que se ajustam a um escopo definido. Essa propriedade é mutuamente exclusiva com a propriedade UsageKeywords. Quando o escopo de pesquisa é aplicado, essa propriedade não tem efeito.

- **ResultObjectType**: é uma propriedade de cadeia de caracteres opcional. O tipo de recurso é usado para renderizar os recursos na lista da caixa de diálogo pop-up. Isso é usado com o Filtro para ajudar o Seletor de Identidade a identificar qual tipo de recurso é retornado pelo Filtro e renderizar os dados de acordo. Essa propriedade é mutuamente exclusiva com a propriedade UsageKeywords. Quando o escopo de pesquisa é aplicado, isso não tem efeito. A cadeia de caracteres que é aceita para essa propriedade é qualquer nome simples, válido e de tipo de recurso, por exemplo, Pessoa. Quando o filtro deve retornar vários tipos de recurso, o Recurso é usado.

- **PreviewTitle**: é o título de visualização usado em uma exibição de lista. Para saber mais sobre essa propriedade, veja a seção UocListView.

- **ListViewTitle**: é uma propriedade de cadeia de caracteres opcional. Você pode usar essa propriedade para definir o texto mostrado na parte superior da exibição de lista como um título.

- **Valor**: é uma propriedade de cadeia de caracteres opcional. É recomendável que você associe isso com um atributo de esquema para conectar o valor a uma fonte de dados.

- **Mode**: é uma propriedade de cadeia de caracteres opcional. Você pode usar essa propriedade para definir se um valor pode ser selecionado pelo Seletor de Identidade ou se podem ser selecionadas várias identidades. SingleResult e MultipleResult são os valores permitidos. Por padrão, ele é definido como SingleResult.

- **ObjectTypes**: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode definir uma lista de tipos de recursos que o usuário final pode resolver entradas em relação à caixa Resolver do Seletor de Identidade. A lista consiste em uma lista de nomes de tipo de recurso separados por uma vírgula ', '.

- **AttributesToSearch**: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode definir uma lista de atributos a serem usados para resolver o item no seletor de identidade, em que a lista é uma lista de atributos de esquema, separados por uma vírgula ', '. Por exemplo, se AttributesToSearch for definido como `DisplayName, Alias` , o usuário poderá pesquisar os itens com `DisplayName = \<search value\>` ou `Alias=\<search value\>` . Os nomes de atributo que são inseridos aqui devem ser atributos válidos nos tipos de recursos de destino da fonte de dados especificada na propriedade Value. Os tipos de recurso de destino podem ser encontrados no campo ObjectTypes. Todos os atributos devem ser válidos em qualquer tipo de recurso determinado que são citados no campo ObjectTypes.

- **ColumnsToDisplay**: é uma propriedade opcional de tipo de cadeia de caracteres. O usuário fornece uma lista de nomes de atributos de esquema, separados por uma vírgula ', '. Os atributos que são definidos aqui compõem a coluna da exibição de lista no Seletor de Identidade.

- **Rows**: é uma propriedade opcional de inteiro. Ele funciona somente quando o modo é definido como MultipleResult. Use essa propriedade para definir a altura da caixa de texto Resolver para um determinado tamanho em unidades de caracteres.

- **MainSearchScreenText**: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto personalizado que é exibido enquanto a pesquisa está em execução na janela Procurar.

**Eventos**:

- **SelectedObjectChanged**: esse evento é emitido quando o usuário altera os recursos selecionados.

**Exemplo**:

![Controle UocIdentityPicker no modo SingleResult](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
>Para que esse exemplo funcione, você deverá criar uma nova vinculação entre o atributo Manager e qualquer tipo de recurso personalizado ao qual esse XML se aplica.

O segmento de código a seguir gera um seletor de identidade no modo SingleResult usando as propriedades Filter e ResultObjectType como parte do RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```

A figura a seguir mostra um seletor de identidade no modo MultipleResult:

![Controle UocIdentityPicker no modo MultipleResult](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
>Para que esse código de exemplo funcione, você deverá vincular o atributo ExplicitMember (um atributo de referência de múltiplos valores) ao tipo de recurso personalizado. Crie escopos de pesquisa com a propriedade UsageKeyword definida como Person e Group.

O segmento de código a seguir cria um seletor de identidade no modo MultipleResult:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

**Nome**: UocLabel

**Descrição**: é um controle de rótulo de texto simples, somente leitura. É recomendável que esse controle seja usado para exibir dados somente leitura.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **Text**: é um atributo de tipo de cadeia de caracteres. Defina essa propriedade, fornecendo um valor explícito de cadeia de caracteres ou vinculando-o a uma fonte de dados. Um exemplo de associação que atribui o valor dessa propriedade é {Binding origem = Object, Path = \<valid attribute name\> .

Para obter um exemplo do controle UocLabel, veja o controle simples na seção de exemplos de Controle simples.


### <a name="uoclistview"></a>UocListView

**Nome**: UocListView

**Descrição**: é um controle de exibição de lista avançado. Ele consiste em uma exibição de lista simples, uma pesquisa opcional simples, um controle de pesquisa avançada opcional, uma caixa de visualização de seleção opcional e uma barra de botões de ação. A pesquisa opcional simples consiste em um escopo de pesquisa e uma caixa de texto de pesquisa simples. O controle de pesquisa avançada é um construtor de filtro. O modo de exibição de lista mostra uma lista pré-renderizada de recursos. Ele também pode mostrar os resultados da pesquisa provenientes dos controles de pesquisa neste controle. A barra de botões de ação define a ação que pode ser obtida com base na seleção na exibição de lista. A caixa de Visualização de Seleção mostra quais itens são selecionados da exibição de lista.

>[!IMPORTANT]
>UocListView não funciona com atributos de referência de valor único. Ele pode ser usado apenas com atributos de referência com vários valores. Para atributos de referência de valor único, veja UocIdentityPicker neste documento.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **SelectedValue**: é uma propriedade opcional de tipo de cadeia de caracteres que está associada a um atributo de referência de valores de multivalors que aceita uma lista de cadeias de caracteres formatadas por GUID.

- **PageSize**: é uma propriedade de inteiro opcional. O usuário pode especificar quantas entradas cabem em uma página em um controle de exibição de lista. O valor padrão é 10 entradas. Qualquer inteiro positivo é válido.

- **UsageKeyword**: é uma propriedade opcional de tipo de cadeia de caracteres. O usuário pode especificar uma lista de palavras-chave que definem o escopo de pesquisa que é usado no controle de exibição de lista de pesquisa. Há recursos de escopo de pesquisa no servidor do FIM 2010. O atributo em uma estrutura SearchScopeConfiguration, chamado UsageKeyword, é usado para agrupar um conjunto de escopos de pesquisa. O modo de exibição de lista consome essa lista de palavras-chave. Cada palavra-chave é separada por vírgula (,). Essa é a palavra-chave de uso usada no escopo de pesquisa correspondente que você deseja mostrar neste modo de exibição de lista. Isso tem efeito somente quando a propriedade ShowSearchControl está definida como true.

- **SearchControlAutoPostback**: é uma propriedade booliana opcional. Defina o valor dessa propriedade como true para executar autopostback quando uma pesquisa é acionada. Por padrão, SearchControlAutoPostback é definido como false.

- **EmptyResultText**: é uma propriedade opcional de tipo de cadeia de caracteres. Por padrão, ele é definido como Nenhum item, mas ela pode ser definida como qualquer valor de cadeia de caracteres. Esse texto aparece quando um resultado de pesquisa está vazio.

- **ButtonHeight**: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer valor inteiro positivo. Esta propriedade define a altura dos botões na barra de ação em pixels. O valor padrão é 32 pixels.

- **ButtonWidth**: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer valor inteiro positivo. Esta propriedade define a largura dos botões na barra de ação em pixels. O valor padrão é 32 pixels.

- **CaptionImageMaxHeight**: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer inteiro positivo. Esta propriedade define a altura máxima do ícone de uma legenda opcional. O valor padrão é 32 pixels.

- **CaptionImageMaxWidth**: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer inteiro positivo. Esta propriedade define a largura máxima do ícone de uma legenda opcional. O valor padrão é 32 pixels.

- **CaptionImageUrl**: é uma propriedade opcional de tipo de cadeia de caracteres. Esta propriedade define uma URL vinculada a uma imagem que aparece como a imagem da legenda.

- **PreviewTitle**: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para definir o texto que aparece na parte superior da caixa de visualização da seleção.

- **EnableSelection**: é uma propriedade opcional de tipo booliano. Você pode usar essa propriedade para definir se uma exibição de lista está no modo de seleção. Se uma exibição de lista está no modo de seleção, uma coluna de caixas de seleção é exibida na coluna mais à esquerda da exibição de lista e uma caixa de visualização de seleção é exibida na parte inferior da exibição de lista. O valor padrão dessa propriedade é definido como true.

- **SingleSelection**: é uma propriedade opcional de tipo booliano. Se o modo de seleção está ativado para o modo de exibição de lista, definir esse valor como true limita o usuário final para selecionar apenas um item na lista. Por padrão, o valor dessa propriedade é definido como false. Isso significa que, por padrão, o usuário final pode selecionar vários itens na lista.

- **RedirectUrl**: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para especificar uma página à qual redirecionar quando um item vinculado por hiperlink é clicado na lista. Essa URL pode conter espaços reservados que são substituídos pelo valor real durante o runtime. Os espaços reservados são os seguintes:

    - {0} objectType
    - {1} objectID
    - {2} displayName

- **ShowTitleBar**: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se a barra de título deve estar visível. O valor padrão dessa propriedade é falso.

- **ShowActionBar**: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se a área da barra de ação deve estar visível. O valor padrão dessa propriedade é true.

- **Exibição prévia**: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se a área de visualização deve estar visível. O valor padrão dessa propriedade é true.

- **ShowSearchControl**: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se o controle de pesquisa deve estar visível. O valor padrão dessa propriedade é true.

- **ResultObjectType**: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para especificar o tipo de objeto esperado dos resultados da pesquisa. O valor padrão dessa propriedade é Resource. Se o resultado de pesquisa contém vários tipos de recurso, esse valor deve ser especificado como Resource.

- **ColumnsToDisplay**: é uma propriedade opcional. Use essa propriedade para especificar os atributos que você deseja que o modo de exibição de lista exiba como colunas. O valor padrão dessa propriedade é DisplayName, ResourceType. Cada coluna é representada pelo nome do sistema de um atributo. Cada coluna é separada por uma vírgula ', '. Você não precisa especificar um valor para essa propriedade quando o modo de exibição de lista é usado no modo de seleção. No modo de seleção, a configuração de coluna vem do atributo SearchScopeColumn do escopo de pesquisa que está selecionado no momento.

- **ListFilter**: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o xpath que é usado para renderizar a exibição de lista e tem efeito somente quando a propriedade ShowSearchControl é definida como false. Quando esse valor é especificado, o modo de exibição de lista usa esse valor de propriedade para consultas e o modo de exibição de lista não está no modo de seleção. O filtro pode ser associado a um atributo de cadeia de caracteres do recurso:

    `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>`

    ou ser uma cadeia de caracteres que contenha alguma variável de ambiente predefinida:

    `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

- **Targetattribute**: é uma propriedade obsoleta. Seu valor deve ser o nome do sistema de um atributo referenciado de vários valores. Recomendamos que essa propriedade não seja mais usada. Por exemplo, em gerenciamento de grupo, em vez de usar:

    `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>`

    Use:

    `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>`

- **ItemClickBehavior**: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para especificar se deseja que o clique em um item de exibição de lista dispare uma postagem de servidor ou exiba um modo de exibição de detalhe do item. Há suporte para dois valores de opção: ModelessDialog e Server. O valor padrão é ModelessDialog.

- **SearchOnLoad**: é uma propriedade opcional de tipo booliano que especifica se o controle de exibição de lista deve ser consultado na carga. Essa propriedade é aplicável somente quando o modo de exibição de lista está no modo de seleção. O valor padrão para essa propriedade é true. Você pode desativá-lo se você espera que o usuário digite geralmente texto em Pesquisar para obter um resultado significativo. Nesse caso, o modo de exibição de lista inicialmente mostra uma mensagem para informar ao usuário como realizar uma pesquisa. O texto pode ser personalizado com as seguintes propriedades:

- **MainSearchScreenText**: essa propriedade opcional de tipo de cadeia de caracteres é aplicável somente quando SearchOnload é definido como true. Essa propriedade pode ser usada para personalizar o texto que aparece no meio da exibição de lista quando o modo de exibição de lista não pesquisa automaticamente. O valor padrão dessa propriedade é localizar os recursos usando a pesquisa, conforme descrito anteriormente. Você pode especificar um valor para tornar o texto mais relevante para seu cenário.

- **SubSearchScreenText**: essa propriedade opcional de tipo de cadeia de caracteres é usada para personalizar o texto que aparece após a propriedade **MainSearchScreenText** . Normalmente, você não precisa especificar um valor para essa propriedade a menos que queira adicionar instruções sobre como usar o modo de exibição de lista.

**Eventos**:

- Não há eventos para este controle.

**Exemplo**:

Para obter exemplos de como usar o modo de exibição de lista junto com o controle UocFilterBuilder como uma lista de visualização, veja as amostras de UocFilterBuilder no início deste documento. O UocListView também pode ser usado sem o construtor de filtro.


### <a name="uocnumericbox"></a>UocNumericBox

**Nome**: UocNumericBox

**Descrição**: essa é uma caixa de texto simples que usa apenas valores inteiros. Esse controle oferece suporte ao modo somente leitura e modo atualizável.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **MaxValue**: é uma propriedade opcional de tipo de inteiro. Use essa propriedade para definir uma validação do lado do cliente para o controle. O valor que o usuário final insere não pode exceder esse valor. Você pode inserir um inteiro explícito ou associá-lo a dados inteiros de uma fonte de dados usando {Binding origem = esquema, caminho = IntegerMaximum}.

- **MinValue**: é uma propriedade opcional de tipo de inteiro. Use essa propriedade para definir uma validação do lado do cliente para o controle. O valor que o usuário final insere não pode ser inferior a esse valor. Você pode inserir um inteiro explícito ou associá-lo a dados inteiros de uma fonte de dados usando {Binding origem = esquema, caminho = IntegerMinimum}.

- **DefaultValue**: é uma propriedade opcional de tipo de inteiro. Use essa propriedade para definir um valor padrão para o controle se o controle for usado para criar novos dados. Este valor só pode ser definido explicitamente como um inteiro estático.

- **Valor**: é uma propriedade opcional de tipo de inteiro. Quando você associa isso a um dado de tipo inteiro de uma fonte de dados, o valor desse atributo é exibido quando a página é carregada e, em seguida, ele é salvo com a fonte de dados após o envio.

**Eventos**:

- **TextChanged**: esse evento é emitido quando o valor atual dentro do controle é alterado.

**Exemplo**:

![Controle UocNumericBox](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
>O código de exemplo a seguir gera a primeira caixa numérica. A caixa numérica não está conectada a uma fonte de dados ou qualquer informação de esquema.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->
```

O código de exemplo a seguir gera a segunda caixa numérica.

>[!NOTE]
>Para esse exemplo funcionar, você primeiro deve criar um atributo novo e de tipo inteiro chamado AnIntegerAttribute e associá-lo ao tipo de recurso personalizado.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

**Nome**: UocPictureBox

**Descrição**: esse controle é usado para renderizar dados de imagem e tipo binário. É recomendável que esse controle seja usado com dados de tipo binário. A imagem pode ser renderizada por uma URL de imagem fornecido, dados de tipo binário ou a origem do atributo que contém os dados de tipo de imagem.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **ImageUrl**: é uma propriedade opcional de tipo de cadeia de caracteres. Insira a URL da imagem de destino.

- **MaxHeight**: é uma propriedade de tipo de cadeia de caracteres opcional. Define a altura máxima da imagem a ser renderizada em pixels.

- **MaxWidth**: é uma propriedade opcional de tipo de cadeia de caracteres. Define a largura máxima da imagem a ser renderizada em pixels.

- **ImageData**: é uma propriedade de tipo binário. Use essa propriedade para vincular uma fonte de dados à imagem exibida. A fonte de dados associada deve ser binária. Você também pode usar este campo para definir explicitamente uma imagem fornecendo dados em um formato de byte[].

- **ImageResource**: é uma propriedade opcional de tipo binário.

- **AlternativeText**: é uma propriedade opcional de tipo de cadeia de caracteres. Essa propriedade é exibida como texto alternativo quando a imagem não pode ser exibida.

**Eventos**:

- Não há eventos para este controle.

**Exemplo**:

>[!NOTE]
>Para usar este exemplo, você deverá ter os dados de uma imagem existente associados ao controle.

O segmento de código a seguir gera um controle de caixa de imagem que associa uma fonte de dados ao controle:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

O segmento de código a seguir gera um controle de caixa de imagem que associa uma imagem de URL ao controle:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```


### <a name="uocradiobuttonlist"></a>UocRadioButtonList

**Nome**: UocRadioButtonList

**Descrição**: é uma lista simples de botões de opção. As opções são mutuamente exclusivas nessa lista. Esse controle é recomendado quando os usuários têm cinco opções ou menos para escolher. Caso contrário, é recomendável UOCListView.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **ValuePath**: o caminho de valor é definido como valor. Ele é associado com o campo Value do elemento Option, conforme descrito nesta seção.

- **CaptionPath**: o caminho de valor é definido como legenda. Ele é associado com o campo de legenda do elemento Option, conforme descrito nesta seção.

- **HintPath**: o caminho de valor é definido como dica. Ele é associado com o campo de dica do elemento Option, conforme descrito nesta seção.

- **SelectedValue**: o valor selecionado no momento. Esta é uma propriedade obrigatória do tipo cadeia de caracteres. Essa propriedade vincula com dados de cadeia de caracteres da fonte de dados.

**Eventos**:

- **SelectedIndexChanged**: o evento ocorre quando o botão de opção selecionado é alterado.

- **CheckedChanged**: quando o botão de opção altera seu estado, esse evento é emitido.

**Opções**:

Só pode haver dois elementos **Options** como opções para esse controle. Para a estrutura de um elemento **Options** , consulte <a href="#options-element">elemento Options</a>.

- **Value**: o campo value em um único elemento Option deve ser definido como true ou false.

- **Legenda**: pode ser qualquer valor de cadeia de caracteres.

- **Dica**: pode ser qualquer valor de cadeia de caracteres.

**Exemplo**:

![Controle UocRadioButtonList](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
>Para esse exemplo funcionar, você deverá criar um novo atributo booliano, ABooleanAttribute e associá-lo ao tipo de recurso personalizado.

O segmento de código a seguir cria uma lista de botões de opção:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton

**Nome**: UocSimpleRadioButton

**Descrição**: é um controle simples de botão de opção. O uso deste controle é semelhante a uma caixa de seleção simples. Há dois botões de opção mostrados lado a lado com os rótulos de texto. É recomendável vincular o controle a dados de tipo booliano.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **TrueText**: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto que aparece quando o botão de opção é selecionado.

- **FalseText**: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto que aparece quando o botão de opção não é selecionado.

- **SelectedItem**: é uma propriedade opcional de tipo booliano. Esse valor indica que o botão de opção está selecionado. Isso pode vincular com dados do tipo booliano de uma fonte de dados. O valor padrão é definido como false.

**Eventos**:

- **CheckedChanged**: quando o botão de opção muda de estado de selecionado para não selecionado ou o oposto, esse sinal é emitido.

**Exemplo**:

![Controle UocSimpleRadioButton](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
>Para fazer esse exemplo funcionar, você deverá criar um novo atributo booliano, ABooleanAttribute e associá-lo ao tipo de recurso personalizado. Os dados do RCDC são aplicados para o mesmo tipo de recurso personalizado.

O segmento de código a seguir gera um botão de opção:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

**Nome**: UocTextBox

**Descrição**: é uma caixa de texto simples que dá suporte à entrada de tipo de cadeia de caracteres. É recomendável que você use este controle para vincular a dados de tipo de cadeia de caracteres.

**Propriedades**:

- Todas as propriedades comuns: para obter informações sobre essas propriedades, consulte <a href="#common-properties">Propriedades comuns</a>.

- **MaxLength**: é um atributo opcional de tipo de inteiro. Essa propriedade especifica o comprimento máximo para uma cadeia de caracteres de entrada. O valor padrão para essa propriedade é 128 caracteres.

- **Text**: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto que aparece na caixa de texto. Você pode definir uma cadeia de caracteres explícita que aparece na caixa de texto durante o carregamento inicial do controle ou associá-lo a um atributo de esquema de um tipo de cadeia de caracteres.

- **Rows**: é uma propriedade opcional de tipo de inteiro. Esta propriedade define a altura da caixa de texto em unidades de caracteres. O valor padrão é um caractere.

- **Columns**: é uma propriedade opcional de tipo de inteiro. Esta propriedade define a largura da caixa de texto em unidades de caracteres. O valor padrão é 20 caracteres.

- **Wrap**: é uma propriedade opcional de tipo booliano. Ao definir o valor dessa propriedade como true, o usuário habilita o recurso de quebra automática de linha na caixa de texto. O valor padrão dessa propriedade é definido como true.

- **UniquenessValidationXPath**: é uma propriedade opcional de tipo de cadeia de caracteres. Ela usa uma expressão de filtro XPath do FIM válida e garante que a entrada de valor pelo usuário seja exclusiva dentro dos recursos que estão no escopo do filtro. Por exemplo, para garantir que o nome de exibição solicitado pelo usuário seja exclusivo em todos os grupos de segurança habilitados para email no banco de dados de serviço do FIM, você usaria o XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. A ação de validação é executada quando o usuário sai da página. Essa propriedade só tem suporte no RCDC para a criação de um recurso.

- **UniquenessErrorMessage**: é uma propriedade opcional de tipo de cadeia de caracteres. Essa cadeia de caracteres é usada para exibir uma mensagem de erro se a validação de UniquenessValidationXPath falhar, e pode ser texto explícito ou uma variável de recurso de cadeia de caracteres. Se essa propriedade não for especificada, a mensagem de erro padrão para uma validação com falha será "%VALUE% já existe. Tente um diferente".

**Eventos**:

- **TextChanged**: esse evento é emitido quando o texto dentro da caixa de texto é alterado.

**Exemplo**:

Veja a seção de exemplos de controle simples para obter um exemplo completo deste controle.


<br/>
<h2 id="appendix-a">Apêndice A: esquema XSD padrão</h2>

Esta seção mostra o esquema XSD completo para todos os RCDCs padrão fornecidos com o Microsoft Identity Manager 2016 SP1.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```

<br/>
<h2 id="appendix-b">Apêndice B: XSL de resumo padrão</h2>

Esta seção mostra o resumo de XSL completo que é fornecido com o Microsoft Identity Manager 2016 SP1.

```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
