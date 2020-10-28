---
title: Referência do desenvolvedor do BHOLD para Microsoft Identity Manager 2016 | Microsoft Docs
description: Referência do BHOLD para desenvolvedores
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: edf9f88a4d96d212cef4aa41cbad7d0b4446534f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757460"
---
# <a name="bhold-developer-reference-for-microsoft-identity-manager-2016"></a>Referência do desenvolvedor do BHOLD para o Microsoft Identity Manager 2016

O módulo BHOLD-core pode processar comandos de script. Isso pode ser feito usando diretamente o bscript.dll em um projeto do .NET. Também interagir com a interface b1scriptservice. asmx do serviço Web. 

Antes de um script ser executado, todas as informações dentro do script devem ser coletadas para compor esse script. Essas informações podem ser coletadas das seguintes fontes:

   * Entrada do usuário
   * Dados do BHOLD
   * Aplicativos
   * Outros

Os dados do BHOLD podem ser recuperados usando a função GetInfo do objeto script. Há uma lista completa de comandos que podem apresentar todos os dados armazenados no banco de BHOLD. No entanto, os dados apresentados estão sujeitos às permissões de exibição do usuário conectado. O resultado está na forma de um documento XML que pode ser analisado.

Outra fonte de informações pode ser um dos aplicativos que são controlados pelo BHOLD. O snap-in de aplicativo tem uma função especial, o FunctionDispatch, que pode ser usado para apresentar informações específicas do aplicativo. Isso também é apresentado como um documento XML.

Por fim, se não houver outra maneira, o script poderá conter comandos diretamente para outros aplicativos ou sistemas. NoThenstallation de software extra no servidor BHOLD podem prejudicar a segurança de todo o sistema.

Todas essas informações são colocadas em um documento XML e atribuídas ao objeto de script BHOLD. O objeto combina este documento com uma função predefinida. A função predefinida é um documento XSL que traduz o documento de entrada de script em um documento de comando do BHOLD.

![Processamento de script BHOLD](media/mim2016-bhold-developer-reference/image001.png)

Os comandos são executados na mesma ordem que no documento. Se uma função falhar, todos os comandos executados serão revertidos.

## <a name="script-object"></a>Objeto script
Esta seção descreve como usar o objeto script.

### <a name="retrieve-bhold-information"></a>Recuperar informações de BHOLD
A função **GetInfo** é usada para recuperar informações dos dados disponíveis no sistema de autorização BHOLD. A função requer um nome de função e, eventualmente, um ou mais parâmetros. Se essa função for realizada com sucesso, um objeto BHOLD ou uma coleção será retornada na forma de um documento XML.

Se a função não for concluída com sucesso, a função GetInfo retornará uma cadeia de caracteres vazia ou um erro. A descrição e o número do erro podem ser usados para obter mais informações sobre a falha.

A função GetInfo ' FunctionDispatch ' pode ser usada para recuperar informações de um aplicativo controlado pelo sistema BHOLD. Essa função requer três parâmetros: a ID do aplicativo, a função de expedição como está definida no ASI e um documento XML com informações de suporte para o ASI. Se a função for realizada com sucesso, o resultado estará disponível em formato XML no objeto de resultado.

O trecho abaixo é um exemplo simples de C# de GetInfo:

```C#
ScriptProcessor myScriptProcessor = new ScriptProcessor();
myScriptProcessor.Initializae("CORP\\b1user");
myScriptProcessor.GetInfo("OrgUnit", "1");
```

Da mesma forma, o objeto **BScript** também pode ser acessado por meio do serviço Web `b1scriptservice` . Isso é feito adicionando uma referência Web ao projeto usando http:// <server> : 5151/BHOLD/Core/b1scriptservice. asmx, em que <server> é o servidor com os binários do BHOLD instalados. Para obter mais informações, consulte [adicionando uma referência de serviço Web a um projeto do Visual Studio](https://msdn.microsoft.com/library/d9w023sx(v=vs.71).aspx).

O exemplo a seguir mostra como usar a função **GetInfo** de um serviço Web. Esse código recupera a unidade organizacional que tem um OrgID de 1 e, em seguida, exibe o nome dessa unidade organizacional na tela.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

namespace bhold_console
{
    class Program
    {
        static void Main(string[] args)
        {
             var bholdService = new BHOLDCORE.B1ScriptService();
             bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";
             string orgname= "";

             if (args.Length == 3)
             {
                 //Use explicit credentials from command line
                 bholdService.UseDefaultCredentials = false;
                 bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                 bholdService.PreAuthenticate = true;
             }
             else
             {
                 bholdService.UseDefaultCredentials = true;
                 bholdService.PreAuthenticate = true;
             }

             //Load BHOLD information into an xml document and loop through document to find the bholdDescription value
             var myOrgUnit = new System.Xml.XmlDocument();
             myOrgUnit.LoadXml(bholdService.GetInfo("OrgUnit","1","","");

            XmlNodeList myList = myOrgUnit.SelectNodes(("//item");

            foreach (XmlNode myNode in myList)
            {
                for (int i = 0; i < myNode.ChildNodes.Count; i++)
                {
                    if (myNode.ChildNodes[i].InnerText.ToString() == "bholdDescription")
                    {
                        orgname = myNode.ChildNodes[i + 1].InnerText.ToString();
                    }
                }
            }

            System.Console.WriteLine("The Organizational Unit Name is: " + orgname);

        }
    }
}
```

O exemplo de VBScript a seguir usa o serviço Web via SOAP e usando GetInfo. Para obter exemplos adicionais de SOAP 1,1, SOAP 1,2 e HTTP POST, consulte a seção de referência gerenciada do BHOLD ou você pode navegar até o serviço Web diretamente de um navegador e exibi-los lá.

```VB
Dim SOAPRequest
Dim SOAPParameters
Dim SOAPResponse
Dim xmlhttp

Set xmlhttp = CreateObject("Microsoft.XMLHTTP")

xmlhttp.open "POST", "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx", False, "CORP\Administrator", "abc123*2k"

xmlhttp.setRequestHeader "Content-type", "text/xml; charset=utf-8"
xmlhttp.setRequestHeader "SOAPAction", "http://B1/B1ScriptService/GetInfo"

SOAPRequest = "<?xml version='1.0' encoding='utf-8'?> <soap:Envelope" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsi=""http://" & vbCRLF
SOAPRequest = SOAPRequest & " www.w3.org/2001/XMLSchema-instance""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsd=""http://www.w3.org/2001/XMLSchema""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/"">" & vbCRLF
SOAPRequest = SOAPRequest & " <soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " <GetInfo xmlns=""http://B1/B1ScriptService"">" & vbCRLF
SOAPRequest = SOAPRequest & " <functionName>OrgUnit</functionName>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter1>1</parameter1>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter2></parameter2>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter3></parameter3>" & vbCRLF
SOAPRequest = SOAPRequest & " </GetInfo>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Envelope>"
MsgBox SOAPRequest

xmlhttp.send SOAPRequest 

SOAPResponse = xmlhttp.responseText

MsgBox SOAPResponse
```

## <a name="execute-scripts"></a>Executar scripts

A função **ExecuteScript** do objeto **BScript** pode ser usada para executar scripts. Essa função requer dois parâmetros. O primeiro parâmetro é o documento XML que contém as informações personalizadas a serem usadas pelo script. O segundo parâmetro é o nome do script predefinido a ser usado. InIn o diretório de scripts predefinidos do BHOLD, aqui deve ser um documento XSL com o mesmo nome da função, mas com a extensão. Xsl.

Se a função não for concluída com sucesso, a função ExecuteScript retornará o valor false. A descrição e o número do erro podem ser usados para saber o que deu errado. Veja a seguir um exemplo de como usar o método Web ExecuteXML. Esse método invoca ExecuteScript.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            var bholdService = new BHOLDCORE.B1ScriptService();
            bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";

            if (args.Length == 3)
            {
                //Use explicit credentials from command line
                bholdService.UseDefaultCredentials = false;
                bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                bholdService.PreAuthenticate = true;
            }
            else
            {
                bholdService.UseDefaultCredentials = true;
                bholdService.PreAuthenticate = true;
            }
            System.Console.WriteLine( "Add user #3 to role #44, result: {0}", bholdService.ExecuteXml(roleAddUser("44", "3")) );
            System.Console.WriteLine("Add user D1 to role 'MR-OU2 No Users', result: {0}", bholdService.ExecuteXml(roleAddUser("MR-OU2 No Users", "D1")));

        }

        private static System.Xml.XmlNode roleAddUser(string roleId, string userId)
        {
            var script = new System.Xml.XmlDocument();
            script.LoadXml(string.Format("<functions>"
                                        +"  <function name='roleadduser' roleid='{0}' userid='{1}' />"
                                        +"</functions>",
                                        roleId,
                                        userId)
                           );
            return script.DocumentElement;
```

## <a name="bholdscriptresult"></a>BholdScriptResult

Essa função GetInfo está disponível depois que a função **ExecuteScript** é executada. A função retorna uma cadeia de caracteres formatada XML que contém o relatório de execução completo. O nó de script contém a estrutura XML do script executado.

Para cada função que falha durante a execução do script, uma função de nó é adicionada com o nome dos nós. ExecuteXML e o erro é adicionado ao final do documento todas as IDs geradas são adicionadas.

Observe que apenas as funções, que contêm um erro, são adicionadas. Um número de erro de ' 0 ' significa que a função não foi executada. 

```
<Bhold>
  <Script>
    <functions>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>     
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>      
    </functions>
  </Script>
  <Function>
    <Name>OrgUnitadd</Name>
    <ExecutedXML>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>
    </ExecutedXML>
    <Error Number="5" Description="Violation of UNIQUE KEY constraint 'IX_OrgUnits'. Cannot insert duplicate key in object 'dbo.OrgUnits'.
The statement has been terminated."/>
  </Function>
  <Function>
    <Name>roleaddOrgUnit</Name>
    <ExecutedXML>
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>
    </ExecutedXML>
    <Error Number="0" Description=""/>
  </Function>
  <IDS>
    <ID name="@ID@">35</ID>
  </IDS>
</Bhold>
```

## <a name="id-parameters"></a>Parâmetros de ID
Os parâmetros de ID obtêm tratamento especial. Valores não numéricos são usados como valor de pesquisa para localizar as entidades correspondentes no repositório de dados BHOLD. Quando o valor de pesquisa não é exclusivo, a primeira entidade que está em conformidade com o valor de pesquisa é retornada.

Para distinguir valores de pesquisa numéricos de IDs, é possível usar um prefixo. Quando os seis primeiros caracteres do valor de pesquisa forem iguais a ' no_id: ', esses caracteres serão removidos antes de o valor ser usado para pesquisa. Os caracteres curinga do SQL '% ' podem ser usados.

Os campos a seguir são usados com o valor de pesquisa:

| Tipo ID   | Campo de pesquisa |
|---|---|
| OrgUnitID | Descrição  |
| roleID    | Descrição  |
| taskID    | Descrição  |
| userID    | DefaultAlias |

## <a name="script-access-and-permissions"></a>Acesso e permissões de script
O código do lado do servidor no Active Server páginas é usado para executar os scripts. Portanto, o acesso ao script significa acesso a essas páginas. O sistema BHOLD mantém informações sobre os pontos de entrada das páginas personalizadas. Essas informações incluem a página inicial e a descrição da função (há suporte para vários idiomas).

Um usuário está autorizado a ser capaz de inserir as páginas personalizadas e executar um script. Cada ponto de entrada é apresentado como uma tarefa. Cada usuário que obteve essa tarefa por meio de uma função ou de uma unidade é capaz de executar a função correspondente.

Uma nova função no menu apresenta todas as funções personalizadas que podem ser executadas pelo usuário. Como um script pode executar ações no sistema BHOLD sob uma identidade diferente do usuário conectado. É possível conceder permissão para executar uma ação específica sem ter a supervisão sobre qualquer objeto. Por exemplo, isso pode ser útil para um funcionário que só tenha permissão para inserir novos clientes na empresa. Esses scripts também podem ser usados para criar páginas de Autoregistro.

## <a name="command-script"></a>Script de comando
O script de comando contém uma lista de funções que são executadas pelo sistema BHOLD. A lista é escrita em um documento XML que está de acordo com as seguintes definições:


|   Script de comando   |              `<functions>functions</functions>`               |
|--------------------|---------------------------------------------------------------|
|     funções      |                      função {function}                      |
|      função      | <função Name = "FunctionName" functionparameters [retornar] (/> \| > parameterlist </função>) |
|    functionName    | Um nome de função válido, conforme descrito nas seções a seguir. |
| funçãoparameters |                     { functionParameter }                     |
| functionParameter  |               parameterName = "ParameterValue"                |
|   parameterName    |                    Um nome de parâmetro válido.                    |
|   Meter   |                      @variable@ \| Value                      |
|       value        |                   Um valor de parâmetro válido.                    |
|   parameterList    |          <parameters> {parameterItem}  </parameters>          |
|   parameterItem    | <parameter name="parameterName"> Meter </parameter>  |
|       return       |                      Return = " @variable @"                      |
|      variável      |                    Um nome de variável personalizado.                    |

O XML tem as seguintes traduções de caracteres especiais:

| XML | Caractere |
|---|---|
| ```&amp;```  | & |
| ```&lt;```   | < |
| ```&gt;```   | > |
| ```&quot;``` | " |
| ```&apos;``` | ' |

Esses caracteres XML podem ser usados em identificadores, mas não são recomendados.

O código a seguir mostra um exemplo de um documento de comando válido com três funções:

```
<functions>

   <functionname="OrgUnitAdd" parentID="34" description="Acme Inc." orgtypeID="5" return="@UnitID@" />

   <function name="UserAdd" description="John Doe" alias="jdoe" languageID="1" OrgUnitID="@UnitID@" />

   <function name="TaskAddFile" taskID="93" path="/customers/purchase">
      <parameters>
         <parameter name="history"> True</parameter>
      </parameters>
   </function>

</functions>
```

A função **OrgUnitAdd** armazena a ID da unidade criada em uma variável chamada UnitID. Essa variável é usada como entrada para a função UserAdd. O valor retornado dessa função não é usado. As seções a seguir descrevem todas as funções disponíveis, os parâmetros necessários e seus valores de retorno.

## <a name="execute-functions"></a>Executar funções
Esta seção descreve como usar as funções execute.

### <a name="abaattributeruleadd"></a>ABAAttributeRuleAdd
Crie uma nova regra de atributo em um tipo de atributo específico. As regras de atributo só podem ser vinculadas a um tipo de atributo.

A regra de atributo especificada pode ser vinculada a todos os tipos de atributo possíveis. 

O RuleType não pode ser alterado com o comando "ABAattributeruletypeupdate". Requer que a descrição do atributo seja exclusiva.


|    Argumentos    |                                                                                                                                                                                                                  Type                                                                                                                                                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Descrição   |                                                                                                                                                                                                                  Texto                                                                                                                                                                                                                  |
|    RuleType     | Especifique o tipo de regra de atributo. Dependendo do tipo de regra de atributo, os outros argumentos devem ser incluídos. Os seguintes valores de tipo de regra são válidos:<ul><li>0: expressão regular (adicionar argumento "valor").</li><li>1: valor (adicionar argumentos "operador" e "valor").</li><li>2: lista de valores.</li><li>3: Range (adicione os argumentos "RangeMin" e "RangeMax").</li><li>4: idade (adicionar argumentos "operador" e "valor").</li></ul> |
|  InvertResult   | ```["0"|"1"|"N"|"Y"]``` |
| AttributeTypeid |                                                                                                                                                                                                                  Texto                                                                                                                                                                                                                  |

| Argumentos opcionais | Tipo |
|---|---|
| Operador | Texto <br>**Observação** : esse argumento será obrigatório se **RuleType** for 1 ou 4. Os valores possíveis são ' = ', ' < ' ou ' > '. As marcas XML precisam usar " &gt; " para ' > ' e " &lt; " para ' < '. |
| RangeMin | Número <br>**Observação** : esse argumento será obrigatório se **RuleType** for 3. |
| RangeMax | Número <br>**Observação** : esse argumento será obrigatório se **RuleType** for 3. |
| Valor    | Texto <br>**Observação** : esse argumento será obrigatório se **RuleType** for 0, 1 ou 4. O argumento deve ser um valor numérico ou alfanumérico. |
| Tipo de retorno AttributeRuleID | Texto   |


### <a name="applicationadd"></a>applicationadd
Cria um novo aplicativo, retorna a ID do novo aplicativo.

| Argumentos | Type |
|---|---|
| descrição |      |
| computador     |      |
| module      |      |
| parâmetro   |      |
| protocolo    |      |
| Nome de Usuário    |      |
| password    |      |
| svroleID (opcional)                | Se esse argumento não estiver presente, será usada uma função de supervisor do usuário atual. |
| Applicationaliasformula (opcional) | A fórmula do alias é usada para criar um alias para um usuário quando ele é atribuído a uma permissão do aplicativo. O alias será criado se o usuário ainda não tiver um alias para esse aplicativo. Se nenhum valor for fornecido, o alias padrão do usuário será usado como alias para o aplicativo. A fórmula é formatada como ` [<<objecttype>>.<<nameofobjecttypeattribute>>(startindexoffset,length offset)]` . O deslocamento é opcional. Somente atributos de usuário e aplicativo podem ser usados. O texto livre pode ser usado. Os caracteres reservados são colchetes à esquerda ([) e colchete direito (]). Por exemplo: `[Application.bholdDescription]\[User.bholdDefAlias(1,5)]`. |
| Tipo de retorno | ID do novo aplicativo. |

### <a name="attributesetvalue"></a>AttributeSetValue
Define o valor de um tipo de atributo conectado ao tipo de objeto. Requer que as descrições do tipo de objeto e do tipo de atributo sejam exclusivas.

| Argumentos | Type |
|---|---|
| Objecttypeid     | Texto |
| ObjectID         | Texto |
| AttributeTypeid  | Texto |
| Valor            | Texto |
| Tipo de retorno      | Type |

### <a name="attributetypeadd"></a>AttributeTypeAdd
Insere um novo tipo de atributo/propriedade.


|        Argumentos        |                                               Type                                                |
|-------------------------|---------------------------------------------------------------------------------------------------|
|       Datatypeid        |                                               Texto                                                |
| Descrição (= identidade) | Texto <br>**Observação** : palavras reservadas não podem ser usadas, incluindo ' a ', ' frm ', ' ID ', ' usr ' e ' bhold '. |
|        MaxLength        |                                       Número em [1,.., 255]                                        |
| ListOfValues (booliano)  |                                          ```["0"|"1"|"N"|"Y"]```                               |
|      DefaultValue       |                                               Texto                                                |
|       Tipo de retorno       |                                               Type                                                |
|     AttributeTypeid     |                                               Texto                                                |

### <a name="attributetypesetadd"></a>AttributeTypeSetAdd
Insere um novo conjunto de tipos de atributo. Requer que a descrição de um conjunto de tipos de atributo seja exclusiva.

| Argumentos | Type |
|---|---|
| Descrição (= identidade) |Texto |
| Tipo de retorno | Type |
| AttributeTypeSetID | Texto |

### <a name="attributetypesetaddattributetype"></a>AttributeTypeSetAddAttributeType
Insere um novo tipo de atributo em um conjunto de tipos de atributo existente. Requer que as descrições do tipo de atributo e o tipo de atributo sejam exclusivos.


|     Argumentos      |                              Type                              |
|--------------------|----------------------------------------------------------------|
| AttributeTypeSetID |                              Texto                              |
|  AttributeTypeid   |                              Texto                              |
|       Order        |                             Número                             |
|     LocationID     | Texto <br>**Observação** : o local é "grupo" ou "único". |
|     Obrigatório      |                    ```["0"|"1"|"N"|"Y"]```                  |
|    Tipo de retorno     |                              Type                              |

### <a name="objecttypeaddattributetypeset"></a>ObjectTypeAddAttributeTypeSet
Adiciona um tipo de atributo definido a um tipo de objeto. Requer que a descrição do tipo de objeto e o conjunto de tipos de atributo sejam exclusivos. Os tipos de objeto são: System, OrgUnit, User e Task.

| Argumentos | Type |
|---|---|
| Objecttypeid | Texto | 
| AttributeTypeSetID | Texto | 
| Order | Número | 
| Visible | <ul><li>0: o tipo de atributo definido é visível.</li><li>2: o tipo de atributo definido é visível quando o botão **mais informações** é selecionado.</li><li>1: o conjunto de tipos de atributo é invisível.</li></ul> | 
| Tipo de retorno | Type | 

### <a name="orgunitadd"></a>OrgUnitadd
Cria uma nova unidade organizacional, retorna a ID da nova unidade organizacional.

| Argumentos | Type |
|---|---|
| descrição | | 
| orgtypeID | | 
| parentID | | 
| OrgUnitinheritedroles (opcional) | | 
| Tipo de retorno | Type | 
| ID da nova unidade | O parâmetro OrgUnitinheritedroles <br>tem o valor Sim ou não. | 

### <a name="orgunitaddsupervisor"></a>OrgUnitaddsupervisor
Tornar um usuário um supervisor de uma unidade organizacional.

| Argumentos | Type |
|---|---|
| svroleID | O argumento userID também pode ser usado. Nesse caso, a função de supervisor padrão é selecionada. Uma função de supervisor padrão tem um nome como **__svrole** seguido por um número. O argumento userID pode ser usado para compatibilidade com versões anteriores. | 
| OrgUnitID | | 

### <a name="orgunitadduser"></a>OrgUnitadduser
Tornar um usuário membro de uma unidade organizacional.

| Argumentos | Type |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="orgunitdelete"></a>OrgUnitdelete
Remove uma unidade organizacional.

| Argumentos | Type |
|---|---|
| OrgUnitID | | 

### <a name="orgunitdeleteuser"></a>OrgUnitdeleteuser
Remove um usuário como membro de uma unidade organizacional.

| Argumentos | Type |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="roleadd"></a>roleadd
Cria uma nova função.

| Argumentos | Type |
|---|---|
| Descrição | | 
| svrole | | 
| svroleID (opcional) | Se esse argumento não estiver presente, será usada uma função de supervisor do usuário atual. | 
| ContextAdaptable (opcional) | ```["0","1","N","Y"]``` | 
| MaxPermissions (opcional) | Integer | 
| MaxRoles (opcional) | Integer | 
| MaxUsers (opcional) | Integer | 
| Tipo de retorno | Type | 
| ID da nova função | | 

### <a name="roleaddorgunit"></a>roleaddOrgUnit
Atribui uma função a uma unidade organizacional.


|    Argumentos    |                                      Type                                      |
|-----------------|--------------------------------------------------------------------------------|
|    OrgUnitID    |                                     roleID                                     |
| inheritThisRole | ' true ' ou ' false ' indica se a função é proposta para unidades subjacentes. |

### <a name="roleaddrole"></a>roleaddrole
Atribui uma função como uma subfunção de outra função.

| Argumentos | Type |
|---|---|
| roleID | | 
| subroleid | | 

### <a name="roleaddsupervisor"></a>roleaddsupervisor
Tornar um usuário um supervisor de uma função.

| Argumentos | Type |
|---|---|
| svroleID | O argumento userID também pode ser usado. Nesse caso, a função de supervisor padrão é selecionada. Uma função de supervisor padrão tem um nome como **__svrole** seguido por um número. O argumento userID pode ser usado para compatibilidade com versões anteriores. | 
| roleID | | 

### <a name="roleadduser"></a>roleadduser
Atribui uma função a um usuário. A função não pode ser uma função adaptável de contexto quando nenhum contextoid é fornecido.

| Argumentos | Type |
|---|---|
| userID | | 
| roleID | | 
| durationtype (opcional) | Pode conter os valores ' Free ', ' hours ' e ' Days '. | 
| durationLength (opcional) | Necessário quando durationtype é ' hours ' ou ' Days '. deve conter o valor inteiro para o número de horas ou dias em que a função é atribuída a um usuário. | 
| Iniciar (opcional) | Data e hora em que a função é atribuída. Quando esse atributo é omitido, a função é atribuída imediatamente. O formato de data é ' AAAA-MM-DDThh: NN: SS ", no qual somente o ano, mês e dia são necessários. por exemplo, "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| fim (opcional) | Data e hora em que a função é revogada. Quando durationtype e durationLength são fornecidos, esse valor é ignorado. O formato de data é ' AAAA-MM-DDThh: NN: SS ", no qual somente o ano, mês e dia são necessários. por exemplo, "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| linkreason | Necessário quando o início, o fim ou a duração são fornecidos, caso contrário, ignorados. | 
| ContextId (opcional) | ID da unidade organizacional, necessária somente para funções adaptáveis de contexto. | 

### <a name="roledelete"></a>roledelete
Exclui uma função.

| Argumentos | Type |
|---|---|
| roleID | | 

### <a name="roledeleteuser"></a>roledeleteuser
Remove a atribuição de função a um usuário. As funções herdadas pelo usuário são revogadas por este comando.

| Argumentos | Type |
|---|---|
| userID | | 
| roleID | | 
| ContextId (opcional) |  | 

### <a name="roleproposeorgunit"></a>roleproposeOrgUnit
Propõe uma função para atribuí-la aos membros e ao OrgUnits de um OrgUnit.

| Argumentos | Type |
|---|---|
| OrgUnitID | | 
| roleID | | 
| durationtype (opcional) | Pode conter os valores ' Free ', ' hours ' e ' Days '. | 
| durationLength | Obrigatório quando durationtype é ' hours ' ou ' Days ', deve conter o valor inteiro para o número de horas ou dias que a função é atribuída a um usuário. | 
| durationFixed | ' true ' ou ' false ' indica se a atribuição dessa função a um usuário deve ser igual a durationLength. | 
| inheritThisRole | ' true ' ou ' false ' indica se a função é proposta para unidades subjacentes. | 

### <a name="taskadd"></a>taskadd
Cria uma nova tarefa, retorna a ID da nova tarefa.

| Argumentos | Type |
|---|---|
| applicationID | | 
| descrição | Texto com um máximo de 254 caracteres. | 
| tarefa | Texto com um máximo de 254 caracteres. | 
| tokenGroupID | | 
| svroleID (opcional) | Se esse argumento não estiver presente, será usada uma função de supervisor do usuário atual. | 
| contextAdaptable (opcional) | ```["0","1","N","Y"]``` | 
| subconstrução (opcional) | ```["0","1","N","Y"]``` | 
| AuditAction (opcional) | <ul><li>0: desconhecido (padrão)</li><li>1: ReportOnly</li><li>2: AlertAppAll</li><li>3: AlertAppObsolete</li><li>4: AlertAppMissing</li><li>5: EnforceAppAll</li><li>6: EnforceAppObsolete</li><li>7: EnforceAppMissing</li><li>8: AlertEnforceAppAll</li><li>9: AlertEnforceAppObsolete</li><li>10: AlertEnforceAppMissing</li><li>11: importal</li></ul> | 
| auditalertmail (opcional) | O endereço de email para receber alertas sobre essa permissão são enviados pelo auditor. Se esse argumento não estiver presente, o endereço de email de alerta do auditor será usado. | 
| MaxRoles (opcional) | Integer | 
| MaxUsers (opcional) | Integer | 
| Tipo de retorno | ID da nova tarefa. | 

### <a name="taskadditask"></a>taskadditask
Indique que duas tarefas são incompatíveis.

| Argumentos | Type |
|---|---|
| taskID | | 
| taskID2 | | 

### <a name="taskaddrole"></a>taskaddrole
Atribui uma tarefa a uma função.

| Argumentos | Type |
|---|---|
| roleID | | 
| taskID | | 

### <a name="taskaddsupervisor"></a>taskaddsupervisor
Tornar um usuário um supervisor de uma tarefa.

| Argumentos | Type |
|---|---|
| svroleID | O argumento userID também pode ser usado. Nesse caso, a função de supervisor padrão é selecionada. Uma função de supervisor padrão tem um nome como **__svrole** seguido por um número. O argumento userID pode ser usado para compatibilidade com versões anteriores. | 
| taskID | | 

### <a name="useradd"></a>useradd
Cria um novo usuário, retorna a ID do novo usuário.

| Argumentos | Type |
|---|---|
| descrição | | 
| alias | | 
| languageID | <ul><li>1: Inglês</li><li>2: Holandês</li></ul> | 
| OrgUnitID | | 
EndDate (opcional) |O formato de data é ' AAAA-MM-DDThh: NN: SS ", no qual somente o ano, mês e dia são necessários. por exemplo, "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| desabilitado (opcional) | <ul><li>0: habilitado</li><li>1: desabilitado</li></ul> | 
| MaxPermissions (opcional) | Integer | 
| MaxRoles (opcional) | Integer | 
| Tipo de retorno | ID do novo usuário. | 

### <a name="useraddrole"></a>UserAddRole
Adiciona uma função de usuário.
<!--- missing content -->

| Argumentos | Type |
|---|---|
|  | | 

### <a name="userdeleterole"></a>UserDeleteRole 
Exclui uma função de usuário.
<!--- missing content -->

| Argumentos | Type |
|---|---|
|  | | 

### <a name="userupdate"></a>Userupdate
Atualiza um usuário.

| Argumentos | Type |
|---|---|
| UserID | | 
| Descrição (opcional)|  | 
| Linguagem | <ul><li>1: Inglês</li><li>2: Holandês</li></ul> | 
| userdesabilitoud (opcional) |<ul><li>0: habilitado</li><li>1: desabilitado</li></ul> | 
| Userenddate (opcional) | O formato de data é ' AAAA-MM-DDThh: NN: SS ", no qual somente o ano, mês e dia são necessários. por exemplo, "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| Nome (opcional) | | 
| MiddleName (opcional) | | 
| Sobrenome (opcional) | | 
| maxPermissions (opcional) | Integer |
| maxRoles (opcional) | Integer | 


## <a name="getinfo-functions"></a>Funções GetInfo
O conjunto de funções descritas nesta seção pode ser usado para recuperar informações armazenadas no sistema BHOLD. Cada função pode ser chamada usando a função GetInfo do objeto BScript. Alguns objetos exigem parâmetros. Os dados retornados estão sujeitos às permissões de exibição e aos objetos supervisionados do usuário conectado.

### <a name="getinfo-arguments"></a>Argumentos GetInfo

| Name | Descrição |
|---|---|
| de dimensionamento da Web | Retorna uma lista de aplicativos. | 
| attributetypes | Retorna uma lista de tipos de atributo. | 
| orgtypes | Retorna uma lista de tipos de unidade organizacional. | 
| OrgUnits | Retorna uma lista de unidades organizacionais sem os atributos das unidades organizacionais. | 
| OrgUnitproposedroles | Retorna uma lista de funções propostas vinculadas à unidade organizacional. | 
| OrgUnitroles | Retorna uma lista de funções vinculadas diretamente da unidade organizacional especificada | 
| Objecttypeattributetypes | | 
| permissões | | 
| permissionusers | | 
| funções | Retorna uma lista de funções. | 
| roletasks | Retorna uma lista de tarefas da função fornecida. | 
| tarefas | Retorna todas as tarefas conhecidas por BHOLD. | 
| usuários | Retorna uma lista de usuários. | 
| usersroles | Retorna a lista de funções de supervisor vinculadas do usuário especificado. | 
| userpermissions | Retorna a lista de permissões do usuário fornecido. | 

### <a name="orgunit-info"></a>Informações de OrgUnit

| Name | parâmetros | Tipo de retorno |
|---|---|---|
| OrgUnit | OrgUnitID | OrgUnit | 
| OrgUnitasiattributes | OrgUnitID | Coleção | 
| OrgUnits| Filter (opcional), proptypeid (opcional)<br> Procura unidades que contêm a cadeia de caracteres descrita em **filtro** no proptype descrito em **proptypeid** . Se essa ID for omitida, o filtro se aplicará à descrição da unidade. Se nenhum filtro for fornecido, todas as unidades visíveis serão retornadas. | Coleção | 
| OrgUnitOrgUnits | OrgUnitID | Coleção | 
| OrgUnitparents | OrgUnitID | Coleção | 
| OrgUnitpropertyvalues | OrgUnitID | Coleção | 
| OrgUnitproptypes |  | Coleção | 
| OrgUnitusers | OrgUnitID | Coleção | 
| OrgUnitproposedroles | OrgUnitID | Coleção | 
| OrgUnitroles | OrgUnitID | Coleção | 
| OrgUnitinheritedroles | OrgUnitID | Coleção | 
| OrgUnitsupervisors | OrgUnitID | Coleção | 
| OrgUnitinheritedsupervisors| OrgUnitID | Coleção | 
| OrgUnitsupervisorroles | OrgUnitID | Coleção | 

### <a name="role-information"></a>Informações de função

| Name | parâmetros | Tipo de retorno |
|---|---|---|
| função | roleID | Objeto | 
| funções | filtro (opcional) | Coleção | 
| roleasiattributes | roleID | Coleção | 
| roleOrgUnits | roleID | Coleção | 
| roleparentroles | roleID | Coleção | 
| rolesubroles | roleID | Coleção | 
| rolesupervisors | roleID| Coleção | 
| rolesupervisorroles | roleID | Coleção | 
| roletasks | roleID | Coleção | 
| roleusers | roleID | Coleção | 
| rolesupervisorroles | roleID | Coleção | 
| proposedroleOrgUnits | roleID | Coleção | 
| proposedroleusers | roleID | Coleção | 

### <a name="permission---task-information"></a>Permissão-informações da tarefa

| Name | parâmetros | Tipo de retorno |
|---|---|---|
| permissão | TaskID | Permissão | 
| permissões | filtro (opcional) | Coleção | 
| permissionasiattributes | TaskID | Coleção | 
| permissionattachments | TaskID | Coleção | 
| permissionattributetypes | - | Coleção | 
| permissionparams | TaskID | Coleção | 
| permissionroles | TaskID | Coleção | 
| permissionsupervisors | TaskID | Coleção | 
| permissionsupervisorroles | TaskID | Coleção | 
| permissionusers | TaskID | Coleção | 
| task | TaskID | Tarefa | 
| tarefas| filtro (opcional) | Coleção | 
| taskattachments | TaskID | Coleção | 
| taskparams | TaskID | Coleção | 
| taskroles | TaskID | Coleção | 
| tasksupervisors | TaskID | Coleção | 
| tasksupervisorroles | TaskID | Coleção | 
| taskusers | TaskID | Coleção | 

### <a name="user-information"></a>Informações do usuário

| Name | parâmetros | Tipo de retorno |
|---|---|---|
| usuário | UserID | Usuário | 
| usuários | Filter (opcional), attributetypeid (opcional) <br>Procura por usuários que contêm no AttributeType especificado por AttributeType a cadeia de caracteres especificada pelo filtro. Se essa ID for omitida, o filtro se aplicará ao alias padrão do usuário. Se nenhum filtro for fornecido, todos os usuários visíveis serão retornados. Por exemplo:<ul><li>`GetInfo("users")` Retorna todos os usuários.</li><li>`GetInfo("users", "%dmin%")` Retorna todos os usuários com a cadeia de caracteres "DMín" no alias padrão.<br></li><li>Suponha que os usuários tenham um atributo extra chamado `"City".GetInfo("users", "%msterda%", "City")` . Essa chamada retorna todos os usuários que têm a cadeia de caracteres "msterda" no atributo City.</li></ul> | UserCollection | 
| usersapplications | UserID | Coleção | 
| Userpermissions | UserID | Coleção | 
| UserRoles | UserID | Coleção | 
| usersroles | UserID | Coleção | 
| userstasks | UserID | Coleção | 
| usersunits | UserID | Coleção | 
| usertarefas | UserID | Coleção | 
| userunidades | UserID | Coleção | 

### <a name="return-types"></a>Tipos de retorno
Nesta seção, os tipos de retorno da função GetInfo são descritos.

| Name | Tipo de retorno |
|---|---|
| Coleção |```=<ITEMS>{<ITEM description="..." id="..." />}</ITEMS>``` | 
| Objeto |```=<ITEM type="…" description="..." />``` | 
| OrgUnit |```= <ITEM id="…" description="..." orgtype="..." parent="..."> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Permissão |```= <ITEM id="…" description="…" name="…" tokengroup="…" application="…" > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Funções |```= <ITEMS> {<ITEM id="…" description="…" />} </ITEMS>``` | 
| Função |```= <ITEM id="…" description="… " > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Tarefa | Consulte a permissão | 
| Usuários | ```= <ITEMS> {<ITEM description="…" id="…" alias="…" />} </ITEMS>``` | 
| Usuário |```= <ITEM id="…" description="…" alias="…" firstname="…" lastname="…" uuid="…" language="…"> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 

## <a name="script-sample"></a>Exemplo de script

Uma empresa tem um servidor BHOLD e deseja um script automatizado que cria novos clientes. As informações sobre a empresa e seu gerente de compra são inseridas em uma página da Web personalizada. Cada cliente é apresentado no modelo como uma unidade sob a unidade de clientes. O gerente de compra é também um membro como um supervisor desta unidade. É criada uma função que fornece aos proprietários o direito de comprar no nome do novo cliente.

No entanto, esse cliente não existe no aplicativo. Há uma função especial implementada no ASI FunctionDispatch que cria uma nova conta de cliente no aplicativo de compra. Cada cliente tem um tipo de cliente.

Os tipos possíveis também podem ser apresentados pela função FunctionDispatch. O AA escolhe o tipo correto para o novo cliente.

Crie uma função e uma tarefa para apresentar os privilégios de compra. O verdadeiro privilégio de compra é apresentado pelo ASI como um arquivo `/customers/customer id/purchase` . Esse arquivo deve ser vinculado à nova tarefa.

A página Active Server que reúne as informações é parecida com esta:

```VB
<%@ Language=VBScript %>
<% Option Explicit %>
<html>
<body>
<form action="MySubmit.asp" method=post>
<input type="hidden" name="OrgUnitID" 
     value="<% = Request("ID") %>">
Company <input type="text" name="Description"> <br>
Type <select name="OrgType">
<%Dim oOrgType
For Each oOrgType on bscript.getinfo("Orgtypes") %>
<option value="<% = oOrgType.OrgTypeID %>">
<% = oOrgType.Description %>
</option> <%
Next %>
</select>  <br>
Manager <input type="text" name=" manager"> <br>
Alias <input type=" text" name=" alias"> <br>
e-mail <input type=" text" name=" email"> <br>
<input type="submit">
</form>
</body>
</html>
```

Todas as páginas personalizadas teriam que fazer é solicitar as informações corretas e criar um documento XML com as informações solicitadas. Neste exemplo, a página mysubmit transforma os dados no documento XML, atribui-os ao **b1script. O objeto Parameters** e finalmente chama a `b1script.ExecuteScript("MyScript")` função.

O script de entrada a seguir mostra este exemplo:

```
<customer>
<description>ACME inc.</description>
<orgtype>5<orgtype>
<name>John Doe</name>
<alias>jdoe</alias>
<email>jdoe@acme.com</email>
</customer>
```

Este script de entrada não contém nenhum comando para BHOLD. Isso ocorre porque esse script não é executado diretamente por BHOLD; em vez disso, essa é a entrada para uma função predefinida. Essa função predefinida converte esse objeto em um documento XML com comandos BHOLD. Esse mecanismo refina o usuário de enviar scripts para o sistema BHOLD que contêm funções que o usuário não tem permissão para executar, como expedições SETUSER e function para um ASI.

```
<?xml version="1.0" encoding="utf-8" ?> 
- <functions xmlns="http://tempuri.org/BscriptFunctions.xsd">

  <function name="roleadduser" roleid="" userid="" /> 
  <function name="roledeleteuser" roleid="" userid="" /> 
  </functions>
```

## <a name="next-steps"></a>Próximas etapas
- [Guia de conceitos do BHOLD](../bhold/bhold-concepts-guide.md)
- [Histórico de versão do BHOLD](version-bhold-history.md)
