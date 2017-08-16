---
title: "Referência do XML de Configuração de Exibição de Controle de Recurso | Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Referência do XML de Configuração de Exibição de Controle de Recurso

Os recursos de Configuração de Exibição de Controle de Recurso (RCDC) são recursos definidos pelo usuário que você pode usar para controlar como os outros recursos no repositório de dados do Microsoft Identity Manager 2016 SP1 (MIM) são exibidos na interface do usuário (IU) para o usuário final. Cada recurso RCDC contém um arquivo de configuração XML que pode ser alterado para adicionar, modificar ou remover o texto e os controles da interface do usuário. Embora o MIM 2016 SP1 forneça vários recursos RCDC padrão, você também pode criar recursos RCDC personalizados. Para saber mais sobre como usar a interface de usuário do RCDC no Portal do FIM, veja [Introdução à Configuração e Personalização do Portal do FIM](http://go.microsoft.com/fwlink/?LinkID=165848) na documentação do FIM.


## <a name="known-issues"></a>Problemas conhecidos

Não há suporte para o valor padrão em vários controles de Configuração de Exibição de Controle de Recurso

Nesta versão, não há suporte para definir valores padrão nos controles em um Controle de Recursos, exceto o controle de botão de opção. Você pode contornar esse problema para uma caixa de lista suspensa especificando um valor padrão que não esteja associado com nenhum valor para forçar o usuário a alterar a seleção. Para contornar esse problema com outros controles, você precisará usar um fluxo de trabalho de autorização para fornecer um valor padrão durante o envio da solicitação.

## <a name="basic-structure"></a>Estrutura básica

Os dados XML de um recurso RCDC consistem no elemento XML **ObjectControlConfiguration.**

>[!NOTE]
Para conhecer o esquema XSD completo, veja o Apêndice A: Esquema XSD padrão, neste documento.

Este é o esquema XSD para o elemento ObjectControlConfiguration:

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



O elemento **ObjectControlConfiguration** contém o seguinte:

1.  **ObjectDataSource**: esse elemento especifica o TypeName de uma classe de fonte de dados que usa o Controle de Recursos (RC). Para obter uma descrição e a definição de esquema, veja a próxima seção de fontes de dados neste documento. Um elemento **ObjectControlConfiguration** pode conter até 32 nós do elemento **ObjectDataSource**.

2.  **XmlDataSource**: é uma fonte de dados simples que é mais comumente usada para especificar o design de uma página de resumo. Para obter uma descrição e a definição de esquema, veja a próxima seção de fontes de dados neste documento. Um elemento **ObjectControlConfiguration** pode conter até 32 nós do elemento **XmlDataSource**.

3.  **Panel**: o administrador pode personalizar o layout da página de RCDC modificando elementos dentro dos elementos do painel. Para saber mais, veja a seção sobre o painel neste documento. Um elemento **ObjectControlConfiguration** deve ter apenas um elemento de painel.

4.  **Events**: os administradores não podem fornecer code-behind personalizado, esse recurso é limitado. Esse é o Event que um painel ou controle pode emitir, com base em uma alteração de estado. Para saber mais, veja a seção de eventos neste documento. Um elemento **ObjectControlConfiguration** pode conter, opcionalmente, um elemento **Event**. Em geral, o uso de **Events** personalizados não tem suporte a menos que isso seja especificamente desenvolvido em melhorias posteriores.

## <a name="data-sources"></a>Fontes de dados

O Microsoft Identity Manager usa fontes de dados como uma maneira de associar dados a componentes de interface do usuário. Isso ajuda a facilitar a separação dos dados da camada de apresentação. Há dois tipos de fontes de dados nos dados de configuração de recursos do RCDC: **ObjectDataSource** e **XmlDataSource**.

-   **ObjectDataSources** especifica uma classe .NET da Microsoft que fornece os dados para o RC. Há um conjunto fixo de tipos disponíveis de ObjectDataSources desde que o administrador possa optar por consumir ao criar RCDCs.

-   **XMLDataSources** fornece uma maneira simples de estruturar dados com base em XML e pode ser usado por administradores para fornecer dados personalizados. Os dados XML devem ser especificados diretamente no RCDC, a menos que você use a estrutura XML interna e predefinida. A estrutura XML interna é usada para gerar páginas de resumo no RC.

No RCDC, você pode vincular essas fontes de dados para atributos dos controles da interface do usuário que são especificados no RCDC para gerar a interface do usuário.

### <a name="objectdatasource"></a>ObjectDataSource

O Microsoft Identity Manager fornece os tipos comuns de origem de dados na tabela a seguir que estão disponíveis para todos os tipos de recurso (exceto onde observado).

| TypeName                        | Descrição     | Dá suporte à vinculação bidirecional | Sintaxe de vinculação com suporte        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Isso representa o recurso do FIM 2010 que está sendo criado, exibido ou editado. O caminho na cadeia de caracteres de vinculação é o nome do atributo. Observe que o tipo de recurso é especificado pelo atributo TargetObjectType do RCDC em vez do atributo RCDC.ConfigurationData. | Sim                     | [AttributeName] Valor do atributo de objeto fornecido pelo seu nome.    |
| PrimaryResourceDeltaDataSource  | Esta fonte de dados cria o XML delta que compara o estado original e o estado atual do recurso do FIM 2010. O XML delta gerado é consumido pelo controle do resumo do RC para renderizar a interface do usuário para a solicitação que o usuário está enviando.                                    | Não                      | DeltaXml: </br> Isso é usado com o controle de resumo para exibir o delta.                                                 |
| PrimaryResourceRightsDataSource | Esta fonte de dados fornece os direitos em linha para cada atributo do recurso do FIM 2010. Isso permite que o RC determine antes do envio quais permissões que o usuário tem nesse atributo de envio e, em seguida, renderiza a interface do usuário para esse atributo adequadamente.                     | Não                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Esta fonte de dados pode ser usada para acessar informações relacionadas ao esquema, como nome de exibição, descrição, se o atributo é necessário, bem como informações do tipo de recurso.                                                                                             | Não                      | [AttributeName].**Required** Valor booliano que indica se o atributo deve ter um valor para ser considerado válido. <br/> [AttributeName].**DisplayNameString** Valor que indica o nome para exibição da vinculação <br/>[AttributeName].**DescriptionString** Valor que indica a descrição da vinculação <br/>[AttributeName].StringRegexString Valor que indica a expressão regular da cadeia de caracteres de uma vinculação. <br/>[AttributeName].**DisplayName** <br/> [AttributeName].**Description** <br/> [AttributeName]. [AttributeName].**IntergerValueMinimum** <br/>[AttributeName].**IntergerValueMaximum** <br/>[AttributeName].**LocalizedAllowedValues**|
| DomainDataSource                | Essa fonte de dados fornece uma enumeração de domínios, com base nos recursos de configuração de domínio. Observe que essa fonte de dados pode ser usada somente em RCDCs de recursos de grupo e de usuário.                                                                           | Sim                     | Domain           |

Veja a seguir um trecho de RCDC de exemplo que associa três fontes de dados ao controle UocTextBox para editar o atributo de descrição de um grupo:

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

### <a name="xmldatasource"></a>XMLDataSource

Usando um **XMLDataSource**, você pode especificar dados personalizados que o RCDC pode consumir para um determinado recurso. Nesse caso, os dados XML devem ser especificados no RCDC. Como alternativa, essa fonte de dados pode ser usada para fazer referência a uma estrutura de dados XML interna para renderizar a interface do usuário para páginas de resumo. Você controla o tipo de **XMLDataSource** a ser usado ao defini-lo no RCDC.


| TypeName                 | Descrição   | | |
|--------------------------|------------|
| **XMLDataSource**            | A fonte de dados representa os dados XML. Ele pode estar em formatos XSL ou XSL incorporado: <br/>**Formato de XSL:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl"> </my:XmlDataSource><br/> **Formato XSL incorporado:** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |Não | ```Xpath[;namespaces]``` <br/> Onde Xpath é um xpath XML válido para selecionar a anotação necessária, geralmente "/" (raiz) <br/>Namespaces é uma lista opcional de cadeias de caracteres prefix=URI, delimitadas por ponto e vírgula, se necessário para que o xpath funcione com XML de namespace. |
| **ReferenceDeltaDataSource** | A fonte de dados representa deltas de atributos de referência com vários valores. Ele é usado somente em RCDC para Group e Set. <br/> Embora a fonte de dados não esteja limitada a Groups ou Sets, ela requer alterações de código no host do RCDC para enviar esses deltas. Atualmente, Group e Set são os únicos hosts que reconhecem esta fonte de dados.  | Sim                      | [AttributeName].Add onde [AttributeName] representa um atributo de referência e os dados retornados são as adições de delta. <br/> Exemplo: [AtributoDeReferência].Add <br/>Exemplo: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName].Remove onde [AttributeName] representa um atributo de referência e os dados retornados são as remoções de delta. <br/> DeltaXml |
|**RequestDetailsDataSource**| A fonte de dados representa o atributo RequestParameter dos objetos Request. O parâmetro define o número máximo de valores de atributo a ser exibido por atributo com vários valores. Ele é usado somente no RCDC para Request. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Não | DeltaXml |
|**RequestStatusDataSource**| A fonte de dados representa o atributo **RequestStatusDetails** dos objetos Request. <br/>Ele é usado somente no RCDC para Request.  | Não | DeltaXml |

-   Para definir uma fonte de dados XML personalizada:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Para usar o XSL de controle de resumo interno, defina a fonte de dados da seguinte maneira:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Se estiver criando um RCDC para um tipo de recurso personalizado, você poderá usar esse método para renderizar automaticamente uma página de resumo para esse recurso personalizado.

Este é um exemplo de como criar uma guia de resumo no RCDC usando o PrimaryResourceDeltaDataSource com o XMLDataSource usando o XSL interno:

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
Este é o esquema XSD para os dois tipos de fontes de dados:

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

## <a name="events"></a>Eventos
Um Evento define o estado de um alteração de um Controle. A extensibilidade desse recurso é limitada, porque você não pode escrever uma função personalizada (manipulador) para definir qual será o comportamento depois que um evento for acionado. O mesmo elemento Event pode ser usado no elemento Panel. Para saber mais, veja a seção sobre o painel neste documento. Este é o esquema XSD para o elemento Event:

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


Um Event é um elemento vazio e ele tem dois atributos.

**Atributos:**

1.  **Name**: é o nome exclusivo de um evento. O único evento com suporte em **ObjectControlConfiguration** é o evento Load. Esse evento é acionado quando a página é carregada pela primeira vez.

2.  **Handler**: é o nome exclusivo de um manipulador. Quando o evento é acionado, geralmente um método de programa é chamado para manipular a alteração do estado do controle. Não há suporte para os seguintes casos: remover um manipulador existente de um controle existente, criar um novo manipulador e anexar a um controle novo ou existente.

Exemplo:

Este é um exemplo de um elemento Events.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


O elemento Panel é o elemento principal em um layout do RCDC. Este é o esquema XSD para o elemento Panel:

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

Esse elemento contém um elemento recorrente, Grouping. Para saber mais, confira a seção Agrupamento neste documento.

O elemento Panel contém quatro atributos:

1.  **Name**: o nome do Painel. Este é um atributo obrigatório do tipo cadeia de caracteres.

2.  **DisplayAsWizard**: esse atributo está preterido no momento. O atributo VerbContext correspondente no RCDC controla se o layout de recurso está em modo de Assistente ou Guia. Se estiver definido como 0 (modo de criação), também estará no modo de Assistente. Caso contrário, estará no modo Guia. Para saber mais, veja Introdução à Configuração e Personalização do Portal do FIM na documentação.

3.  **Caption**: esse atributo está preterido no momento. O usuário pode especificar as legendas para uma página incluindo um Grupo que contém apenas informações de cabeçalho. Para saber mais, confira a seção Agrupamento neste documento.

4.  **AutoValidate**: é um atributo booliano opcional. Quando estiver definido como true, a validação será acionada em relação a cada controle na guia atual. Por padrão, se o atributo estiver ausente, ele será definido como true. Ele pode ser usado em combinação com a propriedade RegularExpression. Para saber mais, veja "RegularExpression" em uma seção posterior deste documento.

## <a name="grouping"></a>Agrupamento

O elemento Grouping define o layout geral do Panel. Ele atua como um contêiner que agrupa os controles individuais em guias e seções diferentes. Este é o esquema XSD para o elemento Grouping:

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


Há três tipos de **Groupings**:

1.  **Header Grouping**: é opcional. Pode haver apenas um Header Grouping em um **Panel**. Um Header Grouping é exibido na parte superior de um painel como legenda.
    Apenas um UocCaptionControl tem permissão para ser usado neste agrupamento. Para obter um exemplo de um Header Grouping, veja a seção Amostra.

2.  **Content Groupings**: pelo menos um Content Grouping é necessário. Pode haver vários Content Groupings em um Panel. Um Content Grouping é exibido como o conteúdo principal de uma página do RCDC. Cada Content Grouping aparece como uma guia no mesmo Panel e pode conter de 1 a 256 controles. Veja a seção Amostra abaixo para obter um exemplo de um **Content Grouping**.

3.  **Summary Groupings**: é opcional. Pode haver apenas um Summary Grouping em um Panel. Um Summary Grouping aparece como a última guia de um Panel. Apenas um controle **UocHtmlSummary** pode ser usado em um Summary Grouping para exibir as alterações feitas pelo usuário antes de enviar uma solicitação. Veja a seção sobre amostras abaixo para obter um exemplo de um Summary Grouping.

Esta tipo de Grouping contém os seguintes elementos:

1.  **Help**: esse elemento fornece o texto de Ajuda em uma guia. Você também pode usá-lo para adicionar um link a um arquivo de Ajuda para a guia.

2.  **Controls**: para saber mais sobre este elemento, veja a seção sobre controles neste documento. Cada agrupamento deve ter 1 a 256 controles inclusive, dependendo do tipo de agrupamento.

3.  **Events**: para saber mais sobre este elemento, veja a seção Eventos neste documento. Cada agrupamento pode, como opção, ter um Event. Os Events que têm suporte em um elemento de Grouping são os seguintes:

    - **BeforeLeave**: esse evento é acionado quando o usuário está pronto para sair de uma guia em um Content Grouping.
    - **AfterEnter**: esse evento é acionado quando o usuário está pronto para entrar em uma guia em um agrupamento de conteúdo.

Atributos:

1.  **Name**: é o nome obrigatório do Grouping. O **Name** deve ser exclusivo no **Panel**.

2.  **Caption**: a **Caption** aparece como a legenda do cabeçalho em um Header Grouping. Ele aparece como a legenda da guia de um agrupamento Content ou Summary.

3.  **Description**: um atributo de cadeia de caracteres opcional, **Description** funciona apenas quando é usado em um Content Grouping. Use esse elemento para fornecer ao usuário final alguns detalhes sobre as informações dentro da mesma guia.

  >[!NOTE]
  Se esse atributo for usado em um Summary Grouping, o XML será considerado inválido. Se esse atributo for usado em um Header Grouping, o XML será considerado válido, porém será ignorado.

4.  **Enabled**: um atributo booliano opcional, Enabled é definido como true quando está ausente. Se Enabled for definido como false, o usuário final verá uma guia Disabled. Esse atributo funciona somente em um agrupamento Content.

  >[!NOTE]
  Se esse atributo for usado em um Summary Grouping, o XML será considerado inválido. Se esse atributo for usado em um Header Grouping, o XML será considerado válido, porém será ignorado.

5.  **Visible**: você pode ocultar uma guia de página do RCDC ou seu título configurando este atributo como false. Por padrão, esse atributo opcional do tipo booliano é definido como true. Esse atributo funciona somente em um Content Grouping.

  >[!NOTE]
  Quando há apenas um Content Grouping em um Panel, esse recurso não funciona. Quando há mais de um Content Grouping em um Panel, ele se comporta conforme descrito acima.

6.  **IsHeader**: é um atributo opcional e booliano que define se Grouping será Header Grouping. Se esse atributo não for especificado, ele será definido como false.

7.  **IsSummary**: é um atributo opcional e booliano que define se Grouping será Summary Grouping. Se esse atributo não for especificado, ele será definido como false.

![XML de configuração de RCD](media/rcd-configuration-xml-reference/image005.jpg)

O código de exemplo XML a seguir gera o Header Grouping anterior. O Header Grouping é a área com o Sample Header Grouping de texto.

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

![XML de configuração de RCD](media\rcd-configuration-xml-reference/image007.jpg)

O código de exemplo XML a seguir gera o Content Grouping anterior. O Content Grouping é a guia mais à esquerda com o texto **Sample Content Grouping**.

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

![XML de configuração de RCD](media/rcd-configuration-xml-reference/image010.jpg)

O código de exemplo XML a seguir gera o Summary Grouping anterior. O Summary Grouping é a guia mais à direita com o texto **Summary**.

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
### <a name="help"></a>Ajuda

O elemento Help, como opção, pode ser incluído em um elemento Grouping ou Control. Se ele for usado em um Grouping, deverá deve ser o primeiro elemento usado. Ele fornece ajuda textual para os usuários finais para ajudá-los a fornecer informações precisas. Este é o esquema XSD para o elemento Help:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Veja a seguir um exemplo de elemento Help:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Control

Um elemento Grouping contém um ou mais elementos Control. Controls são os principais elementos em um RCDC. Você pode personalizar o elemento Grouping definindo os diferentes elementos Control que ele contém. Este é o esquema XSD para o elemento Control:

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

Um Control contém os seguintes elementos:

1.  **Help**: esse elemento é ignorado. Ele funciona somente no Grouping.

2.  **CustomProperties**: esse elemento não tem suporte.

3.  **Options**: esse elemento é usado apenas em combinação com os controles **UocDropDownList** ou **UocRadioButtonList**. Não é funcional com outros controles. Veja a seção Opções neste documento para a estrutura desse elemento. Veja o Control individual para ver como ele é usado no contexto de um Control.

4.  **Buttons**: esse elemento é usado apenas em combinação com o controle **UocListView**. Não é funcional para outros controles. Para saber mais, confira a seção UocListView neste documento.

5.  Properties: esse elemento é usado em todos os Controls para especificar comportamentos adicionais de um Control. Para saber mais sobre este elemento, veja a seção Propriedades neste documento.

6.  **Events**: para a estrutura deste elemento, veja a seção Eventos anteriormente neste documento. Veja a definição de Control individual para determinar qual evento é usado nesse controle.

Um Control contém os seguintes atributos:

1.  **Name**: é o nome do controle. O nome de um Control deve ser exclusivo dentro de cada painel. Este é um atributo obrigatório do tipo cadeia de caracteres.

2.  **TypeName**: esse atributo especifica o tipo de Control. Este é um atributo obrigatório do tipo cadeia de caracteres. Veja a seção Controles individuais neste documento para cada nome de controle.

3.  **Caption**: você pode usar esse atributo para incluir uma legenda para o controle.
    A legenda é geralmente o nome para exibição dos dados que o controle está exibindo ou inserindo. Você pode especificar um valor para a legenda explicitamente ou associá-lo a informações de nome de exibição de atributo do esquema. A legenda é exibida mais à esquerda de um controle de tamanho normal. Se um controle ocupa a tela inteira, a legenda é exibida sobre o controle. Este é um atributo opcional de tipo de cadeia de caracteres. Para saber mais sobre como vincular uma fonte de dados a um atributo ou um valor de propriedade, veja a seção sobre propriedades.

   O exemplo a seguir mostra como uma legenda pode ser usada explicitamente:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     O exemplo a seguir mostra como uma legenda pode ser usada com uma fonte de dados. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é o esquema. É recomendável vincular o DisplayName do atributo a um atributo Caption.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled: é um atributo opcional de tipo booliano. Ao definir o valor desse atributo como false, o usuário poderá desabilitar o Control. O valor padrão é definido como true.

5.  Visible: é um atributo opcional de tipo booliano. Você pode usar esse atributo para ocultar todo o controle. O valor padrão é definido como true.

6.  Description: use esse atributo opcional de tipo de cadeia de caracteres para incluir uma descrição para ajudar o usuário final a compreender o que deve ser colocado no controle ou o que o controle faz. Você pode especificar um valor para a descrição explicitamente ou associá-la a informações de descrição de atributo do esquema. <br/>Description é exibida mais à esquerda de um controle de tamanho normal embaixo da legenda. Se um controle ocupa a tela inteira, a descrição aparece na parte superior do controle abaixo da legenda. Para saber mais sobre como vincular uma fonte de dados a um atributo ou um valor de propriedade, veja a seção Propriedades neste documento.

7.  O exemplo a seguir mostra como uma descrição pode ser usada explicitamente:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  O exemplo mostra como uma descrição pode ser usada com uma fonte de dados. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é o **esquema**. É recomendável vincular a **Description** do atributo a um atributo Description.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: esse atributo indica se o controle abrange a tela inteira. Esse é um atributo opcional de tipo booliano. O valor padrão é definido como false.

    >[!NOTE]
    Os atributos Caption e Description são desabilitados quando esse atributo é definido como true. Você deve usar o controle UocLabel para fornecer uma legenda para um controle expandido.
9. **Hint**: é um atributo opcional de tipo de cadeia de caracteres. O texto no atributo Hint ajuda o usuário final a decidir o que é uma entrada válida para o controle. A Hint é exibida sob o controle.

10.  **AutoPostback**: é um atributo opcional de tipo booliano. O valor padrão é falso. Se estiver definido como false, atualizar a página poderá não atualizar o controle. Para saber mais sobre AutoPostback, procure a propriedade de controle da interface de usuário do Microsoft ASP.NET do mesmo nome.

11. **RightsLevel**: é um atributo opcional de tipo de cadeia de caracteres. Você pode vincular este atributo somente com direitos em linha com uma fonte de dados. O controle é dinamicamente ativado ou desativado, com base nos direitos do usuário. Para saber mais sobre como vincular uma fonte de dados a um atributo ou um valor de propriedade, veja a seção Propriedades neste documento.

    O exemplo mostra como um atributo **RightsLevel** pode ser usado com uma fonte de dados. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é **rights**. Use o nome do atributo como o Path.

### <a name="properties"></a>Propriedades

Você pode usar uma Property para personalizar ainda mais o comportamento de cada controle. Uma Property é um elemento vazio. Este é o esquema XSD para o elemento Property:
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



Cada propriedade tem dois atributos obrigatórios:

1.  **Name**: esse atributo de tipo de cadeia de caracteres é o nome exclusivo da Property.
    Controles diferentes têm propriedades diferentes. Há algumas propriedades comuns que podem ser usadas por todos os controles. Para saber mais sobre quais nomes estão disponíveis para um determinado controle, veja a seção Propriedades comuns e controle individuais.

2.  **Value**: especifica o valor da Property. O tipo de dados do valor depende de a qual propriedade ele é atribuído. Veja a seção a seguir para o formato do valor permitido para propriedades específicas.

Algumas propriedades podem ser vinculadas a informações de uma fonte de dados. Para fazer isso, você precisa usar o seguinte formato de cadeia de caracteres. Veja as propriedades individuais na seção Controles individuais para descobrir como vinculá-las a uma fonte de dados.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**EXEMPLO:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Propriedades comuns
-----------------

Todos os controles do RCDC especificados neste documento podem ter as propriedades comuns a seguir. Você pode usar essas propriedades junto com outras propriedades que são específicas para diferentes controles.

1.  Obrigatório: esta propriedade indica se este é um campo obrigatório ou opcional. Um campo obrigatório deve ser preenchido com um valor. Não há suporte para um valor vazio no caso de entrada de cadeia de caracteres. Um campo opcional pode ser deixado vazio. Se esse é um campo obrigatório sem valor preenchido, será exibida uma mensagem de erro no alto do controle de entrada. Você pode especificar explicitamente se um campo é obrigatório ou opcional. Você também pode vincular com as informações de esquema de uma determinada vinculação entre um atributo e um tipo de recurso. Por padrão, se essa propriedade estiver ausente, isso significará que o controle é de entrada opcional.

   Veja a seguir um exemplo que usa um valor explícito para essa propriedade:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Este é um exemplo que usa uma fonte de dados dinâmico para essa propriedade. Se você usou o modelo para uma fonte de dados exibida na seção anterior deste documento, sua fonte de dados é o esquema. Use \<nome do atributo\>.Required como o Path.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: ao definir essa propriedade como true, o usuário final apresentará o controle em um modo somente leitura. Esse é um atributo opcional de tipo booliano.
    O valor padrão é definido como false. No entanto, às vezes, o comportamento dessa propriedade é substituído pelo tipo de direitos que uma pessoa tem sobre a vinculação de dados com o controle. Por exemplo, se um usuário não tem direitos para atualizar um campo e o campo está vinculado com direitos em linha, o usuário vê os dados em um modo somente leitura mesmo que essa propriedade seja definida como false.

3.  **RegularExpression**: essa propriedade especifica as restrições que são impostas no valor do controle. Os formatos do valor dessa propriedade são os formatos com suporte no padrão StringRegex do .NET. Para saber mais, veja [Expressões regulares do .NET Framework](http://go.microsoft.com/fwlink/?LinkId=165361). Se o controle é usado para inserir um valor, o valor é verificado em relação à restrição que está especificada nesta propriedade quando o usuário tenta sair da página atual.
    A mensagem de erro é exibida na parte superior do controle que tem uma entrada inválida. O usuário pode especificar explicitamente uma expressão regular de cadeia de caracteres. O usuário também pode vinculá-lo com as informações de esquema de um determinado atributo. Por padrão, se essa propriedade estiver ausente, isso significará que o controle não verifica se há restrições nas cadeias de caracteres de entrada.
    Veja a seguir um exemplo que usa um valor explícito para essa propriedade:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Este é um exemplo que usa uma fonte de dados dinâmico para essa propriedade. Se você usou o modelo para uma fonte de dados exibida anteriormente neste documento, sua fonte de dados é o esquema. Use o <attribute name>.StringRegex como o Path.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible: é um atributo opcional de tipo booliano. Você pode usar esse atributo para ocultar todo o controle. O valor padrão é definido como true.

### <a name="options"></a>Opções

O elemento Options inclui um ou mais subnós de Option. O elemento Options é usado apenas com os controles UocRadioButtonList e UocDropDownList. Para obter detalhes sobre como usá-los, veja a seção de controle individual. Este é o esquema XSD para o elemento Options:

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


Atributos:

1.  Value: é um atributo obrigatório do tipo cadeia de caracteres. O atributo de valor deve ser exclusivo dentro do mesmo controle. Somente podem ser usados caracteres que distinguem maiúsculas e minúsculas de A a Z.

2.  Caption: esse atributo obrigatório é o nome de exibição de cada Option.

3.  Hint: é um atributo opcional. Use esse atributo para fornecer mais informações e dicas para o usuário final.

### <a name="environment-variables"></a>Variáveis de ambiente

As variáveis de ambiente na tabela a seguir podem ser usadas em qualquer configuração do RCDC.

| variável | descrição |
|--------|--------|
| `<LoginID>`       | Exibe a ID do usuário que está conectado no momento.           |
| `<LoginDomain>`   | Exibe o domínio do usuário que está conectado no momento.       |
| `<Today>  `       | Exibe a data e hora atuais                                |
| `<FromToday_nnn>` | Exibe a data atual, além de nnn e hora. nnn é um inteiro.  |
| `<ObjectID> `     | A ID do recurso principal do RCDC.                                     |
| `<Attribute_xxx>` | Retorna um atributo especificado, xxx, do recurso primário do RCDC. |

### <a name="debugging-xml-configuration-files"></a>Depurando arquivos de configuração de XML


Quando você estiver desenvolvendo ou modificando os arquivos de configuração de XML para um RCDC, poderá ajudar a reduzir os erros ao validar o XML em relação a arquivos XSD, usando um editor como o Microsoft Visual Studio®. Para saber mais, veja [Uma introdução às ferramentas de XML no Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>Personalização de um arquivo da Ajuda

Se você criar novos recursos e atributos, convém atualizar os arquivos de ajuda existentes no Portal do FIM com conteúdo para seus recursos personalizados. Os arquivos de Ajuda no Portal do FIM estão no formato .htm e eles podem ser editados manualmente.

>[!IMPORTANT]
Para saber mais sobre como criar atributos personalizados, veja Introdução ao recurso personalizado e gerenciamento de atributos na documentação do FIM 2010.

>[!IMPORTANT]
Esta seção fornece informações sobre os conceitos básicos de formatação ou edição de HTML. Para modificar os arquivos de Ajuda, você deverá estar familiarizado com a edição de HTML


**Local dos arquivos de Ajuda**: todos os arquivos de Ajuda para o Portal do Microsoft Identity Management 2016 SP1 ficam na seguinte pasta no servidor do serviço do MIM:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Como localizar o arquivo de Ajuda apropriado**: todos os arquivos de Ajuda para o Portal do FIM são nomeados com um identificador global exclusivo (GUID). Para localizar o arquivo correto para o recurso personalizado:

1.  No Portal do FIM, abra o arquivo de Ajuda na página do Portal que você deseja personalizar.

2.  Clique com botão direito do mouse no arquivo de Ajuda e, em seguida, clique em **Propriedades**.

3.  Realce e copie o arquivo `<GUID\>.htm` no campo 'Endereço de URL'.

4.  Navegue até a pasta onde os arquivos da Ajuda estão armazenados e procure o arquivo.

**Adicionando conteúdo a um novo atributo**: para adicionar conteúdo descritivo para um novo atributo dentro de um elemento de Grouping existente (guia):

1.  Identifique e localize o arquivo de Ajuda apropriado.

2.  Usando um editor de texto, abra o arquivo.

3.  Localize onde você deseja adicionar o conteúdo. Normalmente, isso acontece em um parágrafo adicional, por exemplo:

`<p xmlns="">A new paragraph with customized information.</p>`

Ele também pode ser um item inserido em uma lista existente, por exemplo:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Adicionando conteúdo para um novo elemento Grouping**: a maioria das páginas do Portal do FIM tem vários elementos Grouping (ou guias) e os arquivos da Ajuda que acompanham têm seções marcadas que se relacionam com cada elemento Grouping. Os indicadores no HTML estão especificados nas seções. Por exemplo, este é o HTML para a guia Informações de Trabalho do arquivo de Ajuda para a página Criar Usuário no Portal do FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Ele é referenciado pelo elemento Grouping **WorkInfo** no arquivo de configuração XML de dados para a **Configuração para a criação de usuário** do RCDC. Observe que o nome de arquivo `\<GUID\>.htm` e indicador estão especificados no parâmetro my:Link:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Exemplos de controle simples**

A figura a seguir mostra um exemplo de controles de caixa de texto simples diferentes em modos diferentes.

Exemplo:

![](media/rcd-configuration-xml-reference/image016.gif)

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

O segmento de código a seguir cria o quarto controle de caixa de texto desabilitado.
Embora esse controle não exiba uma diferença visível entre o estado desabilitado e o estado habilitado, o usuário não pode inserir dados na caixa de texto.

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

### <a name="uocbutton"></a>UocButton

**Nome**: UocButton

**Descrição**: é um controle de botão simples que pode ser usado para acionar determinadas ações. No entanto, como você não pode especificar seu próprio manipulador, o uso deste controle é limitado.

**Propriedades**:

1.  **Todas as propriedades comuns**: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  **Texto**: essa propriedade especifica o texto que aparece no botão. Este é um atributo opcional de tipo de cadeia de caracteres. O texto utiliza um valor explícito de cadeia de caracteres.

Evento:

   • **OnButtonClicked**: o evento é emitido quando o botão é clicado.

Exemplo:

![](media/rcd-configuration-xml-reference/image017.png)


O segmento XML a seguir produz o botão simples:

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

**Descrição**: esse controle é usado para exibir a legenda de uma página do RCDC. Esse controle foi projetado para ser usado apenas como um único controle em um agrupamento de cabeçalho.
Usá-lo em qualquer outro contexto pode causar problemas de renderização ou erros de portal.

**Modo**: somente leitura (unidirecional)

**Propriedades:**

1.  **Todas as propriedades comuns**: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  **MaxHeight:** essa propriedade especifica a altura máxima do ícone na seção de legenda. Essa propriedade é opcional. Essa propriedade utiliza um valor inteiro em pixels. O valor padrão é 32 pixels.

Exemplo:

![](media/rcd-configuration-xml-reference/image020.jpg)

O segmento de código a seguir gera o **Header Caption**:
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


Este segmento de código gera o **Explicit Content Caption:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

O segmento de código a seguir gera a legenda dinâmica **Display Name**:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Nome**: UocCheckBox

Descrição: é um controle simples de caixa de seleção. É recomendável que o usuário vincule este controle com os dados de tipo booliano. Este controle pode ser usado como um controle somente leitura ou um controle atualizável, com base nos dados ao qual ele está associado.

>[!NOTE]
Nesta versão, ao usar o controle da caixa de seleção no modo de edição para exibir um atributo booliano, se o atributo não tiver um valor anteriormente atribuído a ele, o Controle de Recurso adicionará um valor **false** ao atributo quando **OK** for clicado no modo de edição. A solução alternativa é sempre criar um atributo booliano que pressupõe que a não existência seja o mesmo que **false** ou usar outros controles, como um botão de opção para atributos boolianos.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  **DefaultValue**: é uma propriedade opcional de tipo booliano. O valor padrão é definido como false. Este campo especifica o comportamento padrão de uma caixa de seleção.
    Isso pode ser especificado explicitamente.

3.  **Checked**: é uma propriedade opcional de tipo booliano. O valor padrão é definido como false. Esse valor substitui a propriedade DefaultValue quando ela estiver presente junto com DefaultValue. Este campo especifica o comportamento de uma caixa de seleção. Como DefaultValue, isso pode ser especificado explicitamente ou vinculado a dados do servidor.

4.  **Text**: é um atributo opcional de tipo de cadeia de caracteres. O texto é mostrado no lado direito da caixa de seleção. Você pode usar essa propriedade para especificar o texto que fornece mais informações para o usuário final.

**Eventos**:

   • CheckedChanged: quando a caixa de seleção muda seu estado, esse evento é emitido.

No exemplo a seguir, uma vinculação personalizada é criada entre o tipo de recurso personalizado e o atributo **IsConfigurationType**. O XML é usado no RCDC de um tipo de recurso personalizado.

Exemplo:

![](media/rcd-configuration-xml-reference/image022.png)

O segmento de código a seguir produz uma **caixa de seleção dinâmica**, conforme mostrado como Caixa de Seleção Dinâmica na figura anterior. Esse tipo de vinculação é geralmente mais versátil e útil do que uma caixa de seleção explícita. O atributo deve pertencer ao tipo de recurso atual.

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

**Descrição**: é um controle de caixa de texto de várias linhas que dá suporte à formatação de cadeia de caracteres especial. Cada valor entre as entradas com valores múltiplos é separado por ponto e vírgula; ou uma quebra de linha na caixa de texto. É recomendável que esse controle seja vinculado a dados de tipos inteiros e de cadeia de caracteres curtos e de valores múltiplos. Esse controle oferece suporte ao modo somente leitura e modo atualizável.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  **DataType**: é um atributo obrigatório do tipo cadeia de caracteres. Você pode especificar isso como um tipo **String, Integer** ou **DateTime** explicitamente. Você também pode vincular o atributo à propriedade **DataType** do atributo do esquema. Um tipo de referência com vários valores deve ser tratado pelo **UOCListView** ou **UOCIdentityPicker**. Booliano de múltiplos valores não é um tipo de dados com suporte.

3.  **Rows**: é um atributo opcional de tipo de inteiro. Você pode definir a altura da caixa no número de caracteres. O valor padrão é definido como 1.

4.  **Columns**: é um atributo opcional de tipo de inteiro. Você pode definir a largura da caixa no número de caracteres. O valor padrão é definido como
    20.

5.  **Value**: é um atributo opcional de tipo de cadeia de caracteres. Você pode vincular este atributo somente com fonte de dados.

Eventos:

   • **ValueListChanged**: esse evento é acionado quando o valor atual muda no controle.

No exemplo a seguir, um atributo de cadeia de caracteres de vários valores chamado **AMultiValueString** é criado e vinculado ao tipo de recurso personalizado. Este exemplo funciona apenas depois que essa vinculação é criada.

**Exemplo:**

![](media/rcd-configuration-xml-reference/image024.jpg)

O segmento de código a seguir gera a imagem anterior:

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

1.  **Todas as propriedades comuns**: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  **DateTimeFormat**: é um atributo opcional de tipo de cadeia de caracteres. Os formatos com suporte são DateTime ou DateOnly. O valor padrão é definido como o formato DateTime.
    a. Formato DateTime: esse atributo é formatado como mm/dd/aaaa hh:mm:ss AM.

      <[!NOTE]
      Ambos os formatos **DateTime** e **DateOnly** têm suporte, independentemente do usuário que está especificando a diferença.
3.  **Value**: é um atributo opcional de tipo de cadeia de caracteres. Você associa esse atributo a uma fonte de dados de recursos. O valor desse atributo deve estar de acordo com o formato de data e hora correto.

Eventos:

   • **DateTimeChanged**: quando o valor de datetime muda, ocorre o evento.

Exemplo:

![](media/rcd-configuration-xml-reference/image027.jpg)

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

Descrição: é um controle simples de menu suspenso. Esse controle geralmente é usado quando você quer selecionar de um conjunto definido de opções. Tipos de dados de cadeia de caracteres, inteiro, datetime e booliano são bons candidatos para este controle.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  **ValuePath**: a propriedade para obter o atributo Valor de ItemSource. Quando ItemSource é especificado como personalizado, o caminho de valor é definido como Value. Ele é vinculado com o campo Value do elemento Option que é definido neste documento.

3.  **CaptionPath**: a propriedade para obter o atributo Valor de ItemSource. Quando ItemSource é especificado como personalizado, o caminho de valor é definido como Caption. Ele é vinculado com o campo Caption do elemento Option que é definido neste documento.

4.  **HintPath**: a propriedade para obter o atributo Valor de ItemSource. Quando ItemSource é especificado como personalizado, o caminho de valor é definido como Hint. Ele é vinculado com o campo Hint do elemento Option que é definido neste documento.

5.  **ItemSource**: uma coleção de ListControlItems que define as opções na lista. O usuário pode definir isso explicitamente como Custom e usar o elemento Option para especificar o valor de cadeia de caracteres.

6.  **SelectedValue**: o valor selecionado no momento. Esta é uma propriedade obrigatória do tipo cadeia de caracteres. Essa propriedade é vinculada com dados de cadeia de caracteres da fonte de dados.

Eventos:

  • SelectedIndexChanged: o evento ocorre quando a seleção na caixa de lista suspensa é alterada.

Opções:

Para a estrutura de um elemento Option, veja a seção "Opção" neste documento.

1.  **Value**: o valor de um único elemento Option pode ser definido para qualquer cadeia de caracteres de entrada válida da fonte de dados ao qual o controle esteja vinculado.

2.  **Caption**: pode ser qualquer valor de cadeia de caracteres.

3.  **Hint**: pode ser qualquer valor de cadeia de caracteres.

Exemplo:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Para que a amostra funcione, você deverá vincular um atributo **Scope** existente de tipo de cadeia de caracteres com o tipo de recurso personalizado ao qual o RCDC se aplica.


Este segmento de código gera a lista suspensa:

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

Nome: UocFileDownload

Descrição: esse controle contém um hiperlink. Quando o hiperlink é clicado, aparece uma página Salvar Arquivo do Windows. O usuário pode salvar o arquivo em sua unidade local.
Também há suporte para a opção Open se o Internet Explorer puder renderizar o formato de arquivo. Os tipos de dados recomendados com os quais usar este controle são cadeia de caracteres formatada (XML) e tipos binários.

>[!NOTE]
Nesta versão do Microsoft Identity Manager 2016 SP1, o usuário deve fechar a janela do Internet Explorer, no qual abriu o arquivo e, em seguida, atualizar a página. Depois de atualizar a janela do Internet Explorer, o usuário pode iniciar o download para salvar ou abrir o mesmo arquivo novamente na janela original.


Propriedades:

1.  **Todas as propriedades comuns**: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  **Text**: é um atributo opcional de tipo de cadeia de caracteres que define o texto do hiperlink. O usuário pode especificar uma cadeia de caracteres explícita para essa propriedade.

3.  **Value**: é um atributo obrigatório. Especifica a vinculação de atributo no servidor cujo conteúdo deve ser baixado.

4.  **PromptedFileName**: é um atributo opcional de tipo de cadeia de caracteres. Este é o nome do arquivo que é sugerido para o usuário ao salvar o arquivo baixado.

5.  **ContentType**: é um atributo obrigatório do tipo cadeia de caracteres. Esse é o tipo de arquivo no qual os dados são salvos. Texto ou binário são as duas opções de cadeia de caracteres com suporte. Se for texto, o valor de retorno será considerado como uma cadeia de caracteres longa.
    Caso contrário, para binário, o valor de retorno será considerado como byte[]. Se o texto for selecionado, o usuário poderá, como opção, adicionar um sufixo para especificar o tipo de formato no qual o texto está. Por exemplo, text/xml é válido.


>[!NOTE]
Quando o valor que está associado a este controle está vazio, o controle não tem o hiperlink para ser usado para disparar a ação de download. Isso ocorre porque não há nada para baixar.


**Eventos**:

Não há eventos para este controle.

**Exemplo**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Antes de carregar esse arquivo de amostra, o usuário deve criar uma vinculação entre um tipo de recurso personalizado e o atributo ConfigurationData existente.


O segmento de código a seguir gera o controle de download de arquivo na imagem anterior:

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
Não há nenhuma indicação do andamento ou status do upload. Quando o arquivo for carregado para a fonte de dados local, a caixa de texto estará desmarcada.


Propriedades:

1.  Todas as propriedades comuns: para obter informações, veja a seção de propriedades comuns deste documento.

2.  Value: é um atributo obrigatório. Especifica a vinculação de atributo de esquema no servidor ao qual os dados são carregados.

3.  ContentType: é um atributo opcional de tipo de cadeia de caracteres. Esse é o tipo de dados em que o arquivo é salvo no servidor. Isso pode ser definido para Texto ou Binário. Quando a propriedade estiver ausente, o valor padrão será Binário.

4.  MaxFileSize: é um atributo opcional de tipo de cadeia de caracteres. MaxFileSize define o tamanho máximo em que o arquivo pode ser carregado. Por padrão, se a propriedade estiver ausente, o tamanho máximo será 1 megabyte (MB).

5.  PromptedForNoValue: é um atributo opcional de tipo de cadeia de caracteres. Ele define o texto que aparece para o usuário quando um arquivo não está sendo carregado.

Eventos:

   • FileUploaded: esse evento é emitido quando o arquivo é carregado com êxito.

Exemplo:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Para fazer o seguinte código de exemplo funcionar, você deverá criar um novo atributo de tipo binário denominado ABinaryAttribute e, em seguida, criar uma nova associação entre um tipo de recurso personalizado e esse atributo.


O segmento de código a seguir gera o controle de upload na imagem anterior:
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

Nome: UocFilterBuilder

Descrição: é um controle complexo que permite ao usuário renderizar uma expressão XPath do MIM 2016. Algumas expressões XPath não são aceitas. Para saber mais sobre como usar o construtor de filtro, veja a Ajuda para o construtor de filtro.

Propriedades:

1.  Todas as propriedades comuns: para obter mais informações, veja a seção de propriedades comuns deste documento.

2.  PermittedObjectTypes: define uma lista de tipos de recurso a ser mostrada na instrução de seleção de um construtor de filtro. Para saber mais sobre como usar o construtor de filtro, veja a Ajuda do construtor de filtro. A cadeia de caracteres está no formato de ResourceTypeA, ResourceTypeB, onde cada tipo de recurso é separado por vírgula (,).

3.  Valor: é o valor com o qual o construtor de filtro é renderizado.
    Há suporte para apenas uma vinculação com dados de tipo de cadeia de caracteres que contém uma expressão XPath. O atributo Filter é um atributo recomendado para vincular este controle.

4.  PreviewButtonVisible: é uma propriedade opcional de tipo booliano. Quando essa propriedade é definida como false, o usuário não vê um botão de visualização. O valor padrão é definido como true. Esse botão pode ser usado em combinação com um controle de exibição de lista para visualizar os resultados de uma expressão XPath.

5.  ExcludeGroupMembership: é uma propriedade booliana. Quando essa propriedade é definida como true, você não pode criar um filtro que usa \<Atributo de Referência\> (por exemplo, ResourceID) é membro do \<objeto Grupo\>. Em outras palavras, quando essa propriedade é definida como true, você não pode criar um filtro que usa o diretório de associação de grupo.

6.  PreviewButtonCaption: é uma cadeia de caracteres opcional. Quando PreviewButtonVisible está definido como true, você pode usar essa propriedade para dar ao botão um texto personalizado. O texto é exibido no botão de visualização.

Eventos:

   • OnFilterChanged: esse evento é acionado quando o conteúdo do construtor de filtro é alterado.

Exemplo:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Antes de usar esse código de exemplo, crie uma nova associação entre um atributo Filter existente e um tipo de recurso personalizado.


Este é o código de exemplo que inclui um controle UOCLabel, um construtor de filtro simples com PermittedObjectTypes e um modo de exibição de lista de visualização. Você deve apontar a propriedade ListFilter de exibição de lista e a propriedade Value do construtor de filtro para o mesmo atributo de fonte de dados para vincular os dois.

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


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Nome: UocHtmlSummary

Descrição: você pode usar este controle para definir uma página de resumo em uma página do RCDC.
Essa página de resumo aparece depois que o usuário final envia uma solicitação. Este controle só pode ser usado em um Summary Grouping e ele deve ser o único controle. É altamente recomendável que você use o código de exemplo fornecido.

>[!NOTE]
Esse controle não foi testado extensivamente.


Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  ModificationsXml: esta propriedade deve ser formatada como {Binding Source=delta, Path=DeltaXml}, onde delta é definido no cabeçalho de configuração ObjectDataSource.

3.  TransformXsl: essa propriedade normalmente é formatada como {Binding Source=summaryTransformXsl, Path=/}, onde summaryTransformXsl é definido no cabeçalho de configuração XmlDataSource.

Para obter uma amostra existente deste controle, veja "Summary Grouping" na seção de agrupamento no início deste documento.

### <a name="uochyperlink"></a>UocHyperLink

Nome: UocHyperLink

Descrição: é um controle simples de hiperlink. Você pode usar este controle para exibir informações como um hiperlink.

Propriedades:

1.  Todas as propriedades comuns: para obter informações, veja a seção de propriedades comuns deste documento.

2.  ObjectReference: é uma propriedade opcional do tipo de referência. Se um recurso válido estiver referenciado pelo GUID que é definido nesta propriedade, o hiperlink fornecerá ao usuário final uma maneira de acessar o recurso. Isso é mutuamente exclusivo com a propriedade NavigateUrl (abaixo).

3.  Text: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode usar essa propriedade para definir o texto que aparece como o hiperlink.

4.  NavigateUrl: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode usar essa propriedade para definir a URL de caminho completo ao qual o hiperlink está vinculado. Isso é mutuamente exclusivo com a propriedade ObjectReference (acima).

Exemplo:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
Você precisa de um GUID válido de um recurso ao qual vincular. Nesse caso, o segundo hiperlink é gerado com um GUID válido. O primeiro deles pode ser qualquer site da Web.


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

Nome: UocIdentityPicker

Descrição: esse controle consiste em uma caixa Resolver opcional e uma janela Procurar. A caixa Resolver opcional consiste em uma caixa de texto opcional para inserir a identidade, um botão Resolver para resolver a identidade e um botão Procurar para solicitar uma janela pop-up de procura. A janela Procurar possibilita que o usuário selecione identidades por meio de um controle de exibição de lista. A identidade selecionada na janela Procurar é refletida na caixa de Resolver.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  UsageKeywords: é uma propriedade opcional de cadeia de caracteres. Você pode definir uma lista de escopos de pesquisa a serem usados no Seletor de Recurso fornecendo uma lista de palavras-chave de uso que têm suporte da estrutura SearchScopeConfiguration, onde cada palavra-chave é separada por um (‘).

3.  Filter: é uma propriedade opcional de cadeia de caracteres. O usuário fornece uma expressão XPath para definir o escopo do seletor de recurso para exibir apenas os itens que se ajustam a um escopo definido. Esta propriedade é mutuamente exclusiva com a propriedade UsageKeywords (acima). Quando o escopo de pesquisa é aplicado, essa propriedade não tem efeito.

4.  ResultObjectType: é uma propriedade opcional de cadeia de caracteres. O tipo de recurso é usado para renderizar os recursos na lista da caixa de diálogo pop-up. Isso é usado com o Filtro para ajudar o Seletor de Identidade a identificar qual tipo de recurso é retornado pelo Filtro e renderizar os dados de acordo. Esta propriedade é mutuamente exclusiva com a propriedade UsageKeywords (veja acima). Quando o escopo de pesquisa é aplicado, isso não tem efeito. A cadeia de caracteres que é aceita para essa propriedade é qualquer nome simples, válido e de tipo de recurso, por exemplo, Pessoa.
    Quando o filtro deve retornar vários tipos de recurso, o Recurso é usado.

5.  PreviewTitle: é o título de visualização usado em uma exibição de lista. Para saber mais sobre essa propriedade, veja a seção UocListView.

6.  ListViewTitle: é uma propriedade opcional de cadeia de caracteres. Você pode usar essa propriedade para definir o texto mostrado na parte superior da exibição de lista como um título.

7.  Value: é uma propriedade opcional de cadeia de caracteres. É recomendável que você associe isso com um atributo de esquema para conectar o valor a uma fonte de dados.

8.  Mode: é uma propriedade opcional de cadeia de caracteres. Você pode usar essa propriedade para definir se um valor pode ser selecionado pelo Seletor de Identidade ou se podem ser selecionadas várias identidades. SingleResult e MultipleResult são os valores permitidos. Por padrão, ele é definido como SingleResult.

9.  ObjectTypes: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode definir uma lista de tipos de recursos que o usuário final pode resolver entradas em relação à caixa Resolver do Seletor de Identidade. A lista é composta por uma lista de nomes de tipo de recurso separados por vírgula (,).

10. AttributesToSearch: é uma propriedade opcional de tipo de cadeia de caracteres. Você pode definir uma lista de atributos a ser usado para resolver o item no Seletor de Identidade, onde a lista é uma lista de atributos de esquema separados por vírgula (,). Por exemplo, se AttributesToSearch for definido como DisplayName, Alias, isso significará que o usuário é capaz de pesquisar itens com DisplayName = \<pesquisar valor\> ou Alias =\<pesquisar valor\>. É recomendável que os nomes de atributo que são inseridos aqui sejam atributos válidos em tipos de recurso de destino da fonte de dados que está localizada no Valor. Os tipos de recurso de destino podem ser encontrados no campo ObjectTypes. Todos os atributos devem ser válidos em qualquer tipo de recurso determinado que são citados no campo ObjectTypes.

11. ColumnsToDisplay: é uma propriedade opcional de tipo de cadeia de caracteres. O usuário fornece uma lista de nomes de atributo de esquema separados por vírgula (,). Os atributos que são definidos aqui compõem a coluna da exibição de lista no Seletor de Identidade.

12. Rows: é uma propriedade opcional de tipo de inteiro. Ele funciona somente quando o modo é definido como MultipleResult. Use essa propriedade para definir a altura da caixa de texto Resolver para um determinado tamanho em unidades de caracteres.

13. MainSearchScreenText: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto personalizado que é exibido enquanto a pesquisa está em execução na janela Procurar.

Eventos:

 • SelectedObjectChanged: esse evento é emitido quando o usuário altera os recursos selecionados.

Exemplo:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Para que esse exemplo funcione, você deverá criar uma nova vinculação entre o atributo Manager e qualquer tipo de recurso personalizado ao qual esse XML se aplica.


O segmento de código a seguir gera um Seletor de Identidade em modo único usando as propriedades Filter e ResultObjectType como parte do RCDC:

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



Exemplo:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Para que esse código de exemplo funcione, você deverá vincular o atributo ExplicitMember (um atributo de referência de múltiplos valores) ao tipo de recurso personalizado. Você também deve criar escopos de pesquisa com a propriedade UsageKeyword definida como Pessoa e Grupo.


O segmento de código a seguir cria o controle na imagem anterior:

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

Nome: UocLabel

Descrição: é um controle simples, somente leitura, de rótulo de texto. É recomendável que esse controle seja usado para exibir dados somente leitura.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  Text: é um atributo do tipo cadeia de caracteres. Defina essa propriedade, fornecendo um valor explícito de cadeia de caracteres ou vinculando-o a uma fonte de dados. Uma associação de exemplo que atribui o valor dessa propriedade é {Binding Source=object, Path=\<valid attribute name\>.

Para obter um exemplo do controle UocLabel, veja o controle simples na seção de exemplos de Controle simples.

### <a name="uoclistview"></a>UocListView

Nome: UocListView

Descrição: é um controle avançado de exibição de lista. Ele consiste em uma exibição de lista simples, uma pesquisa opcional simples, um controle de pesquisa avançada opcional, uma caixa de visualização de seleção opcional e uma barra de botões de ação. A pesquisa opcional simples consiste em um escopo de pesquisa e uma caixa de texto de pesquisa simples. O controle de pesquisa avançada é um construtor de filtro. O modo de exibição de lista mostra uma lista pré-renderizada de recursos. Ele também pode mostrar os resultados da pesquisa provenientes dos controles de pesquisa neste controle. A barra de botões de ação define a ação que pode ser obtida com base na seleção na exibição de lista. A caixa de Visualização de Seleção mostra quais itens são selecionados da exibição de lista.

>[!IMPORTANT]
UocListView não funciona com atributos de referência de valor único. Ele pode ser usado apenas com atributos de referência com vários valores. Para atributos de referência de valor único, veja UocIdentityPicker neste documento.


Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  SelectedValue: é uma propriedade opcional de tipo de cadeia de caracteres, que geralmente está vinculada a um atributo de referência com vários valores aceitando uma lista de cadeias de caracteres de formato de GUID.

3.  PageSize: é uma propriedade opcional de tipo de inteiro. O usuário pode especificar quantas entradas serão ajustadas em uma página em um controle de exibição de lista. O valor padrão é 10 entradas. Qualquer inteiro positivo é válido.

4.  UsageKeyword: é uma propriedade opcional de tipo de cadeia de caracteres. O usuário pode especificar uma lista de palavras-chave que definem o escopo de pesquisa que é usado no controle de exibição de lista de pesquisa. Há recursos de escopo de pesquisa no servidor do FIM 2010. O atributo em uma estrutura SearchScopeConfiguration, chamado UsageKeyword, é usado para agrupar um conjunto de escopos de pesquisa. O modo de exibição de lista consome essa lista de palavras-chave. Cada palavra-chave é separada por vírgula (,).
    Essa é a usagekeyword usada no escopo de pesquisa correspondente que você deseja mostrar neste modo de exibição de lista. Isso tem efeito somente quando a propriedade ShowSearchControl está definida como true.

5.  SearchControlAutoPostback: é uma propriedade Booliana opcional. Defina o valor dessa propriedade como true para executar autopostback quando uma pesquisa é acionada. Por padrão, SearchControlAutoPostback é definido como false.

6.  EmptyResultText: é uma propriedade opcional de tipo de cadeia de caracteres. Por padrão, ele é definido como Nenhum item, mas ela pode ser definida como qualquer valor de cadeia de caracteres. Esse texto aparece quando um resultado de pesquisa está vazio.

7.  ButtonHeight: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer valor inteiro positivo. Esta propriedade define a altura dos botões na barra de ação em pixels. O valor padrão é 32 pixels.

8.  ButtonWidth: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer valor inteiro positivo. Esta propriedade define a largura dos botões na barra de ação em pixels. O valor padrão é 32 pixels.

9.  CaptionImageMaxHeight: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer inteiro positivo. Esta propriedade define a altura máxima do ícone de uma legenda opcional. O valor padrão é 32 pixels.

10. CaptionImageMaxWidth: é uma propriedade opcional de tipo de inteiro. Defina o valor dessa propriedade como qualquer inteiro positivo. Esta propriedade define a largura máxima do ícone de uma legenda opcional. O valor padrão é 32 pixels.

11. CaptionImageUrl: é uma propriedade opcional de tipo de inteiro. Esta propriedade define uma URL vinculada a uma imagem que aparece como a imagem da legenda.

12. PreviewTitle: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para definir o texto que aparece na parte superior da caixa de visualização da seleção.

13. EnableSelection: é uma propriedade opcional de tipo booliano. Você pode usar essa propriedade para definir se uma exibição de lista está no modo de seleção. Se uma exibição de lista está no modo de seleção, uma coluna de caixas de seleção é exibida na coluna mais à esquerda da exibição de lista e uma caixa de visualização de seleção é exibida na parte inferior da exibição de lista. O valor padrão dessa propriedade é definido como true.

14. SingleSelection: é uma propriedade opcional de tipo booliano. Se o modo de seleção está ativado para o modo de exibição de lista, definir esse valor como true limita o usuário final para selecionar apenas um item na lista. Por padrão, o valor dessa propriedade é definido como false. Isso significa que, por padrão, o usuário final pode selecionar vários itens na lista.

15. RedirectUrl: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para especificar uma página à qual redirecionar quando um item vinculado por hiperlink é clicado na lista. Essa URL pode conter espaços reservados que são substituídos pelo valor real durante o tempo de execução. Os espaços reservados são os seguintes:

     ◦ {0} – objectType

     ◦ {1} – objectID

     ◦ {2} – displayName

16.  ShowTitleBar: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se a barra de título deve estar visível. O valor padrão dessa propriedade é falso.

17.  ShowActionBar: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se a área da barra de ação deve estar visível. O valor padrão dessa propriedade é true.

18.  ShowPreview: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se a área de visualização deve estar visível. O valor padrão dessa propriedade é true.

19.  ShowSearchControl: é uma propriedade opcional de tipo booliano. Use essa propriedade para especificar se o controle de pesquisa deve estar visível. O valor padrão dessa propriedade é true.

20.  ResultObjectType: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para especificar o tipo de objeto esperado dos resultados da pesquisa. O valor padrão dessa propriedade é Resource. Se o resultado de pesquisa contém vários tipos de recurso, esse valor deve ser especificado como Resource.

21.  ColumnsToDisplay: é uma propriedade opcional. Use essa propriedade para especificar os atributos que você deseja que o modo de exibição de lista exiba como colunas. O valor padrão dessa propriedade é DisplayName, ResourceType. Cada coluna é representada pelo nome do sistema de um atributo. Cada coluna é separada por vírgula (,). Você não precisa especificar um valor para essa propriedade quando o modo de exibição de lista é usado no modo de seleção. No modo de seleção, a configuração de coluna vem do atributo SearchScopeColumn do escopo de pesquisa que está selecionado no momento.

22.  ListFilter: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o xpath que é usado para renderizar a exibição de lista e tem efeito somente quando a propriedade ShowSearchControl é definida como false. Quando esse valor é especificado, o modo de exibição de lista usa esse valor de propriedade para consultas e o modo de exibição de lista não está no modo de seleção. O filtro ou pode ser vinculado a um atributo de cadeia de caracteres do recurso, da seguinte maneira:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     ou ser uma cadeia de caracteres que contém algumas variáveis de ambiente predefinidas, da seguinte maneira: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: é uma propriedade obsoleta. Seu valor deve ser o nome do sistema de um atributo referenciado de vários valores. Recomendamos que essa propriedade não seja mais usada. Por exemplo, em gerenciamento de grupo, em vez do seguinte:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Deve ser: `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: é uma propriedade opcional de tipo de cadeia de caracteres. Use essa propriedade para especificar se deseja que o clique em um item de exibição de lista dispare uma postagem de servidor ou exiba um modo de exibição de detalhe do item. Há suporte para dois valores de opção: ModelessDialog e Server. O valor padrão é ModelessDialog.

25.  SearchOnLoad: é uma propriedade opcional, de tipo booliano que especifica se o controle de exibição de lista deve consultar ao carregar. Essa propriedade é aplicável somente quando o modo de exibição de lista está no modo de seleção. O valor padrão para essa propriedade é true. Você pode desativá-lo se você espera que o usuário digite geralmente texto em Pesquisar para obter um resultado significativo. Nesse caso, o modo de exibição de lista inicialmente mostra uma mensagem para informar ao usuário como realizar uma pesquisa. O texto pode ser personalizado com as seguintes propriedades:

26.  MainSearchScreenText: essa propriedade opcional de tipo de cadeia de caracteres é aplicável somente quando SearchOnload é definida como true. Essa propriedade pode ser usada para personalizar o texto que aparece no meio da exibição de lista quando o modo de exibição de lista não pesquisa automaticamente. O valor padrão desta propriedade é Localizar os recursos desejados usando a pesquisa acima. Você pode especificar um valor para tornar o texto mais relevante para seu cenário.

27.  SubSearchScreenText: essa propriedade opcional de tipo de cadeia de caracteres é usada para personalizar o texto que aparece abaixo de MainSearchScreenText. Normalmente, você não precisa especificar um valor para essa propriedade a menos que queira adicionar instruções sobre como usar o modo de exibição de lista.

Para obter exemplos de como usar o modo de exibição de lista junto com o controle UocFilterBuilder como uma lista de visualização, veja as amostras de UocFilterBuilder no início deste documento. O UocListView também pode ser usado sem o construtor de filtro.

### <a name="uocnumericbox"></a>UocNumericBox

Nome: UocNumericBox

Descrição: é uma caixa de texto simples que usa somente valores inteiros. Esse controle oferece suporte ao modo somente leitura e modo atualizável.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  MaxValue: é uma propriedade opcional de tipo de inteiro. Use essa propriedade para definir uma validação do lado do cliente para o controle. O valor que o usuário final insere não pode exceder esse valor. Você pode inserir um inteiro explícito ou associar isso com dados de inteiro de uma fonte de dados usando {Binding Source=schema Path=IntegerMaximum}.

3.  MinValue: é uma propriedade opcional de tipo de inteiro. Use essa propriedade para definir uma validação do lado do cliente para o controle. O valor que o usuário final insere não pode ser inferior a esse valor. Você pode inserir um inteiro explícito ou associar isso com dados de inteiro de uma fonte de dados usando {Binding Source=schema, Path=IntegerMinimum}.

4.  DefaultValue: é uma propriedade opcional de tipo de inteiro. Use essa propriedade para definir um valor padrão para o controle se o controle for usado para criar novos dados. Este valor só pode ser definido explicitamente como um inteiro estático.

5.  Value: é uma propriedade opcional de tipo de inteiro. Quando você associa isso a um dado de tipo inteiro de uma fonte de dados, o valor desse atributo é exibido quando a página é carregada e, em seguida, ele é salvo com a fonte de dados após o envio.

Manipulador:

   • TextChanged: esse evento é emitido quando o valor atual muda dentro do controle.

Exemplo:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
O código de exemplo a seguir gera a primeira caixa numérica. A caixa numérica não está conectada a uma fonte de dados ou qualquer informação de esquema.

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
Para esse exemplo funcionar, você primeiro deve criar um atributo novo e de tipo inteiro chamado AnIntegerAttribute e associá-lo ao tipo de recurso personalizado.

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

Nome: UocPictureBox

Descrição: esse controle é usado para renderizar dados de tipo binário de imagem. É recomendável que esse controle seja usado com dados de tipo binário. A imagem pode ser renderizada por uma URL de imagem fornecido, dados de tipo binário ou a origem do atributo que contém os dados de tipo de imagem.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  ImageUrl: é uma propriedade opcional de tipo de inteiro. Insira a URL da imagem de destino.

3.  MaxHeight: é uma propriedade opcional de tipo de inteiro. Define a altura máxima da imagem a ser renderizada em pixels.

4.  MaxWidth: é uma propriedade opcional de tipo de inteiro. Define a largura máxima da imagem a ser renderizada em pixels.

5.  ImageData: é uma propriedade de tipo binário. Use essa propriedade para vincular uma fonte de dados à imagem exibida. A fonte de dados associada deve ser binária.
    Você também pode usar este campo para definir explicitamente uma imagem fornecendo dados em um formato de byte[].

6.  ImageResource: é uma propriedade opcional de tipo binário.

7.  AlternativeText: é uma propriedade opcional de tipo de cadeia de caracteres. Essa propriedade é exibida como texto alternativo quando a imagem não pode ser exibida.

Exemplo:

>[!NOTE]
Para usar este exemplo, você deverá ter os dados de uma imagem existente associados ao controle.


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

Nome: UocRadioButtonList

Descrição: é uma lista simples de botão de opção. As opções são mutuamente exclusivas nessa lista. Esse controle é recomendado quando os usuários têm cinco opções ou menos para escolher. Caso contrário, é recomendável UOCListView.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  ValuePath: o caminho de valor é definido como Value. Ele é vinculado com o campo Value do elemento Option que é definido neste documento.

3.  CaptionPath: o caminho de valor é definido como Caption. Ele é vinculado com o campo Caption do elemento Option que é definido neste documento.

4.  HintPath: o caminho de valor é definido como Hint. Ele é vinculado com o campo Hint do elemento Option que é definido neste documento.

5.  SelectedValue: o valor selecionado no momento. Esta é uma propriedade obrigatória do tipo cadeia de caracteres. Essa propriedade vincula com dados de cadeia de caracteres da fonte de dados.

Eventos:

1.  SelectedIndexChanged

2.  CheckedChanged

Opções:

Só pode haver dois elementos de opção em Opções para este controle.

1.  Value: o campo de valor em um único elemento de opção deve ser definido como True ou False.

2.  Caption: pode ser qualquer valor de cadeia de caracteres.

3.  Hint: pode ser qualquer valor de cadeia de caracteres.

Exemplo:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Para esse exemplo funcionar, você deverá criar um novo atributo booliano, ABooleanAttribute e associá-lo ao tipo de recurso personalizado.

O segmento de código a seguir cria a lista do botão de opção na imagem anterior:

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


Nome: UocSimpleRadioButton

Descrição: é um controle simples de botão de opção. O uso deste controle é semelhante a uma caixa de seleção simples. Há dois botões de opção mostrados lado a lado com os rótulos de texto. É recomendável vincular o controle a dados de tipo booliano.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento

2.  TrueText: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto que aparece quando o botão de opção é selecionado.

3.  FalseText: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto que aparece quando o botão de opção não é selecionado.

4.  SelectedItem: é uma propriedade opcional de tipo booliano. Esse valor indica que o botão de opção está selecionado. Isso pode vincular com dados do tipo booliano de uma fonte de dados. O valor padrão é definido como false.

Eventos:

   • CheckedChanged: quando o botão de opção muda de estado de selecionado para desmarcado, ou o oposto, este sinal é emitido.

Exemplo:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Para fazer esse exemplo funcionar, você deverá criar um novo atributo booliano, ABooleanAttribute e associá-lo ao tipo de recurso personalizado. Os dados do RCDC são aplicados para o mesmo tipo de recurso personalizado.

O segmento de código a seguir gera o botão de opção na imagem anterior:

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

Nome: UocTextBox

Descrição: é uma caixa de texto simples que oferece suporte à entrada de tipo de cadeia de caracteres. É recomendável que você use este controle para vincular a dados de tipo de cadeia de caracteres.

Propriedades:

1.  Todas as propriedades comuns: para saber mais sobre essa propriedade, veja a seção de propriedades comuns deste documento.

2.  MaxLength: é um atributo opcional de tipo de inteiro. Essa propriedade especifica o comprimento máximo para uma cadeia de caracteres de entrada. O valor padrão para essa propriedade é 128 caracteres.

3.  Text: é uma propriedade opcional de tipo de cadeia de caracteres. Este é o texto que aparece na caixa de texto. Você pode definir uma cadeia de caracteres explícita que aparece na caixa de texto durante o carregamento inicial do controle ou associá-lo a um atributo de esquema de um tipo de cadeia de caracteres.

4.  Rows: é uma propriedade opcional de tipo de inteiro. Esta propriedade define a altura da caixa de texto em unidades de caracteres. O valor padrão é 1 caractere.

5.  Columns: é uma propriedade opcional de tipo de inteiro. Esta propriedade define a largura da caixa de texto em unidades de caracteres. O valor padrão é 20 caracteres.

6.  Wrap: é uma propriedade opcional de tipo booliano. Ao definir o valor dessa propriedade como true, o usuário habilita o recurso de quebra automática de linha na caixa de texto. O valor padrão dessa propriedade é definido como true.

7.  UniquenessValidationXPath: é uma propriedade opcional de tipo de inteiro. Ele usa uma expressão de filtro FIM XPath válida e garante que o valor de entrada do usuário é exclusivo dentro dos recursos que estão no escopo do filtro.
    Por exemplo, para garantir que o nome de exibição solicitado pelo usuário seja exclusivo em todos os grupos de segurança habilitados para email no banco de dados de serviço do FIM, você usaria o XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. A ação de validação é executada quando o usuário sai da página. Essa propriedade só tem suporte no RCDC para a criação de um recurso.

8.  UniquenessErrorMessage: é uma propriedade opcional de tipo de inteiro. Essa cadeia de caracteres é usada para exibir uma mensagem de erro se a validação de UniquenessValidationXPath falhar, e pode ser texto explícito ou uma variável de recurso de cadeia de caracteres. Se essa propriedade não for especificada, a mensagem de erro padrão para uma validação com falha será "%VALUE% já existe. Tente um diferente".

Eventos:

   • TextChanged: esse evento é emitido quando o texto dentro da caixa de texto foi alterado.

Veja a seção de exemplos de controle simples para obter um exemplo completo deste controle.

## <a name="appendix-a-default-xsd-schema"></a>Apêndice A: Esquema XSD padrão

Este é o esquema XSD completo para todos os RCDCs padrão que são fornecidos com o Microsoft Identity Manager 2016 SP1:

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



## <a name="appendix-b-default-summary-xsl"></a>Apêndice B: XSL de resumo padrão

Este é o XSL de resumo completo que é fornecido com o Microsoft Identity Manager 2016 SP1:
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
