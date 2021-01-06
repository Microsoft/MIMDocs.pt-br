---
title: Histórico de lançamento de versão do conector | Microsoft Docs
description: Este tópico lista todas as versões dos Conectores do FIM (Forefront Identity Manager) e MIM (Microsoft Identity Manager)
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/31/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f0b61059f9010523fa4f7b6a6ced987e5ab2dc49
ms.sourcegitcommit: 8f81767ec92e1b80658aaebb9463aa4d62396d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927686"
---
# <a name="connector-version-release-history"></a>Histórico de lançamento de versão do conector

Os conectores vinculam fontes de dados conectadas específicas a Microsoft Identity Manager (MIM) e Azure AD Connect. (No Forefront Identity Manager, os conectores eram conhecidos como agentes de gerenciamento.) Muitos dos conectores, como conectores para provisionar usuários em Active Directory, são entregues como parte da instalação do serviço de sincronização do MIM e do pacote de instalação do Azure AD Connect. Além disso, mais conectores, como servidores de diretório de terceiros, são enviados como um download separado para que possam ser atualizados com mais frequência para adicionar suporte à conexão com versões atualizadas do MIM de sistemas de destino de terceiros.  

> [!NOTE]
> Este tópico se baseia principalmente nos conectores FIM e MIM. A menos que explicitamente chamado abaixo, esses conectores não têm suporte para instalação no Azure AD Connect. Os conectores liberados são pré-instalados no Azure AD Connect ao atualizar para a compilação especificada.


Este tópico lista todas as versões do pacote de conectores genéricos que foram liberados separadamente do MIM.  Para obter uma lista de conectores com suporte com o MIM, consulte [conectores com suporte no mim 2016 SP2](../supported-management-agents.md).  Alguns parceiros criaram seus próprios conectores dessa maneira, e uma lista completa está disponível no wiki [FIM 2010 e MIM 2016: agentes de gerenciamento de parceiros](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx).


Links relacionados:

* [Baixar os Conectores mais recentes](https://go.microsoft.com/fwlink/?LinkId=717495)
* [Conector LDAP Genérico](microsoft-identity-manager-2016-connector-genericldap.md)
* [Conector do SQL Genérico](microsoft-identity-manager-2016-connector-genericsql.md)
* [Conector dos Serviços Web](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws)
* [Conector do PowerShell](microsoft-identity-manager-2016-connector-powershell.md)
* [Conector do Lotus Domino](microsoft-identity-manager-2016-connector-domino.md)
* Documentação de referência [do conector do repositório de perfis de usuário do SharePoint](https://go.microsoft.com/fwlink/?LinkID=331344)

## <a name="1113470-december-2020"></a>1.1.1347.0 (dezembro de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector do Graph
  - Correção de um problema com o conector enviando incorretamente convites B2B ao criar um grupo habilitado para email ou um contato

## <a name="1113460-november-2020"></a>1.1.1346.0 (novembro de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector do Graph
  - Correção de um problema com o cache do conector local corrompido causando falhas de execução de importação Delta
  - Correção de um problema com entradas duplicadas relatadas pelo conector durante a execução da importação completa, causando erros de descoberta
  - Corrigido um problema com a importação incorreta de tipos de dados complexos, por exemplo, *employeeOrgData*
- Conector SQL genérico
  - Corrigido um problema com falha de autenticação nativa do SQL devido à propriedade da cadeia de conexão do DSN *TrustedConnection* definida como *false* 
- Conector LDAP Genérico
  - Corrigido um problema com o processamento de entradas *OpenLDAP* *accessLog* na importação Delta, causando alterações incorretas de associação de grupo e outros erros

## <a name="1113020-september-2020"></a>1.1.1302.0 (setembro de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector do Graph
  - Corrigido um problema com a atualização de esquema para o ponto de extremidade */beta*

## <a name="1113010-august-2020"></a>1.1.1301.0 (agosto de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector do Graph
  - Correção de um bug com falhas de importação causadas por valor de atributo *UserType* vazio
  - Correção de um bug com falhas de importação Delta o cache do conector não está sendo legível causado por erros de descoberta
  - Os tempos limite do serviço e do proxy relatados podem ser repetidos automaticamente
  - Descoberta de esquema atualizada para ponto de extremidade */beta* 
### <a name="enhancements"></a>Aprimoramentos
- Conector do Graph
  - Adição de suporte para leitura e gravação de valores de atributos de extensão de diretório personalizados
  - Suporte adicionado para ler membros de *entidade de serviço* de grupos no ponto de extremidade */beta* 
  - Melhorias de desempenho para execuções de importação Delta relacionadas à descoberta de esquema
  - O conector do Graph agora pode convidar usuários de membros externos

  > [!NOTE]
  > Se você estiver usando o convite de convidado no Build 1.1.1170.0 do conector, atualize suas regras de sincronização com a seguinte lógica:

  - Fluxos de saída
    - Um usuário é convidado ao exportar a criação do usuário, e a exportação inclui um atributo de *email* , mas não um *atributo userPrincipalName*.  Se o *userPrincipalName* for fornecido, um usuário será criado em vez de ser convidado
    - O atributo *UserType* só define se um usuário se tornará um *membro* ou um *convidado* (o padrão é *membro* , se não definido)
  - Fluxos de entrada
    - Os valores de atributo *userPrincipalName* de usuários externos são renderizados ' no estado em que se encontram '

## <a name="1111700-april-2020"></a>1.1.1170.0 (abril de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector SQL genérico
   - Correção de um bug com estratégia de exportação baseada em consulta e atualizações de atributos com valores múltiplos
- Conector do Lotus Notes
   - Grupos de notas secundárias os catálogos de endereços não são mais excluídos pelo processo *AdminP* . A operação de exclusão direta é usada agora
- Conector LDAP Genérico
   - Correção de um bug com atributos de operações de diretório LDAP, por exemplo, *pwdUpdateTime*, não visível no esquema
### <a name="enhancements"></a>Aprimoramentos
- Conector do Graph   
   - Os UPNs de usuários convidados externos não são mais renderizados ' no estado em que se encontram '. em vez disso, eles são mostrados no espaço do conector para se parecer com emails
   - Suporte adicionado para provisionamento de usuários convidados B2B

   Para convidar usuários de convidados B2B, você precisa:
   - Conceder permissões para convidar convidados para seu aplicativo do Azure AD associado ao conector do Graph
   - Concluir a seção de configuração do conector para convidar usuários externos: definir URL de redirecionamento de convite (obrigatório) e escolher se deseja enviar emails de convite
   - Defina atributos obrigatórios em sua regra de sincronização de saída:
     - "Convidado" =>*UserType* (somente fluxo inicial)
     - Endereço de email externo =>*userPrincipalName*
     - Customion ("CN =" + csObjectID + ", OBJECT = User") =>*DN* (somente fluxo inicial)
     - csObjectID = *ID* de>(somente fluxo inicial)

## <a name="1111300-february-2020"></a>1.1.1130.0 (fevereiro de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector do Graph
   - A exportação não falha mais ao tentar adicionar um membro a um grupo que já foi adicionado
   - o suporte ao atributo virtual ' export_password ' é corrigido, não é mais necessário configurar o fluxo de atributos ' password '
   - Fluxo de exportação fixo de atributos de cadeia de caracteres de valores múltiplos
   - Correção de vários bugs afetando a importação Delta
   - Vários bugs de formato DateTime em potencial corrigidos
- Conector SQL genérico
   - Vários bugs de interface do usuário corrigidos
   - Corrigido o tratamento de referências incorretas durante a importação Delta
   - Correção de um bug com a estratégia de importação Delta do SQL Controle de Alterações e tabelas de valores múltiplos para importar alterações de associação de grupo corretamente
   - Correção de um bug com valores de atributo não sendo limpos na exportação
   - Corrigido um bug com o último elemento do atributo de referência de valores múltiplos que não está sendo excluído na exportação
   - Correção de um bug com atualização de esquema fazendo com que os atributos de referência sejam definidos para cadeias de caracteres
   - Correção de um bug com valores de parâmetros de procedimento armazenados sendo truncados para 397 bytes
   - Correção de um bug com tabelas Oracle e a detecção de esquema de exibições diferenciam maiúsculas de minúsculas
- Conector do Lotus Notes
   - Desempenho aprimorado ao importar membros do grupo
   - A importação completa não falha mais com ' erros de referência nula '
   - Corrigido um bug com a exclusão da caixa de correio do Notes quando a ACL está definida
   - Nomes de grupo vazios não causam mais falhas de importação Delta
   - Correção de um bug com caracteres não imprimíveis deixados em atributos após exclusões de valores de cadeia de caracteres
- Conector LDAP Genérico
   - A importação Delta não mostra mais o valor literal ' replace ' quando nenhum valor é definido no Changelog de origem
### <a name="enhancements"></a>Aprimoramentos
- Conector do Graph
   - Suporte adicionado para nuvens soberanas e capacidade de configurar o logon e as URLs de recurso de grafo
   - Os atributos sem suporte são filtrados e ocultados nas propriedades do conector após a descoberta do esquema

## <a name="4418001-july-2019"></a>4.4.1800.1 (julho de 2019)
### <a name="enhancements"></a>Melhorias:
- Conector de armazenamento de perfil de usuário do SharePoint
   - Adicionado suporte para o SharePoint Server 2019. O conector está disponível como um download no [centro de download da Microsoft](https://go.microsoft.com/fwlink/?linkid=279713) 

## <a name="119530-june-2019"></a>1.1.953.0 (junho de 2019)

### <a name="fixed-issues"></a>Problemas corrigidos:
- Conector LDAP Genérico
   - A importação Delta não falhará mais se o campo de alterações estiver vazio no diretório da Internet da Oracle
- Conector SQL genérico
   - Correção de um problema com a estratégia de importação de consulta SQL personalizada usando parâmetros StartIndex e EndIndex, resultando na importação da primeira página
   - Correção de um problema com a estratégia de importação de tabela/exibição ao ler do MS SQL, resultando na importação da primeira página
- Conector do grafo:
   - Os contatos organizacionais agora são tratados corretamente, o email não está mais ausente
   - Corrigido um problema com as operações de importação e exportação de atributo do Gerenciador. O conector não falha mais quando o Gerenciador está vazio. O valor do Gerenciador é atualizado corretamente no Azure AD
   - Corrigido um problema com a importação Delta de valores vazios
   - Correção de um problema com o travamento do conector na importação Delta quando o arquivo de cache local está corrompido
   - Correção de vários problemas com a exportação incorreta de valores vazios ou quando apenas o caso de valor de atributo foi alterado
   - O conector agora lida com as operações de exclusão/adição para o mesmo atributo durante a execução correta da exportação
   - Correção de vários problemas com importações e exportações de execução longa quando os tokens de acesso expiraram durante a execução do conector. O conector agora renova tokens de acesso quando necessário sem falha
   - Correção de um problema com o último membro de um grupo não estar sendo excluído
### <a name="enhancements"></a>Melhorias:
- Conector SQL genérico
   - o parâmetro commandTimeout de um leitor de dados está definido para corresponder ao tempo limite do conector. Se você tiver consultas de execução longa, levando mais de 30 segundos para serem concluídas, você poderá aumentar o valor do parâmetro "tempo limite de comando" na seção "conectividade"
- Conector do grafo: 
   - Adicionada estratégia de importação completa de associação de grupo multi-threaded para melhorar o desempenho da importação. A importação Delta permanece uma operação de thread único
   - Suporte adicionado para tipos de esquema complexos atributos resultantes como OnPremisesExtentionAttributes. * disponíveis agora
   - Suporte adicionado para o atributo export_password para evitar erros de exportação-alteração-não reimportados e não mostrar a senha inicial no espaço do conector. O comportamento é semelhante a outros conectores do ECMA2
   - Um manipulador foi adicionado para dar suporte à limitação de solicitações HTTP. Quando a réplica do Azure AD recebe muitas solicitações de um cliente, ela pode responder com Retry-After instrução. O conector será pausado e tentará novamente em vez de falhar
   - O perfil de importação Delta não será mais iniciado se os filtros de consulta forem definidos. Se você quiser importar somente objetos específicos do Azure AD, por exemplo, usuários com o sobrenome iniciado com um *, A funcionalidade de importação Delta será bloqueada


## <a name="119130-january-2019"></a>1.1.913.0 de \( janeiro de 2019\)

### <a name="fixed-issues"></a>Problemas corrigidos:

* SQL genérico:
    * Foi corrigido um bug de regressão no conector do SQL genérico em que ele só lê os primeiros 5000 objetos.
* Conector do grafo:
    * Corrigido um problema em que o conector de API do Graph falhou ao ler/gravar de um locatário ou criar um novo conector quando a opção beta está selecionada na conectividade.

### <a name="enhancements"></a>Melhorias:

* Conector do grafo:
    * Defina o protocolo TLS 1,2 como o padrão para o conector do Graph.
* Conector do SQL genérico:
    * O conector do SQL genérico foi testado com Oracle 12C e Oracle 18C.

## <a name="119110"></a>1.1.911.0

> [!NOTE]  
> Esta versão de Build do conector é apenas para uso em implantações Azure AD Connect e não é fornecida para uso em implantações do MIM.
> 
> Em comparação com a versão anterior do conector, ele não contém melhorias ou atualizações para clientes do MIM.

-  Atualiza os conectores não padrão (por exemplo, conector LDAP genérico e conector do SQL genérico) fornecidos com Azure AD Connect.  

## <a name="118610"></a>1.1.861.0

### <a name="fixed-issues"></a>Problemas corrigidos:
* Alterar os títulos de todas as configurações de conector do ' Forefront para a Microsoft '

* Lotus Notes: 
    * Erros em COM o Lotus às vezes não retornam erros
* LDAP genérico:
    * Gldap símbolo extra para DI com o servidor de diretório de PING
* Serviços Web genéricos:
    * Erro ao exportar a análise de JSON
* SQL genérico:
    * Exportar atributo binário
    * Tipos de objeto não podem ser subcadeias uns dos outros
    * As alterações na tabela de valores múltiplos não serão rastreadas na operação de "importação Delta", se "estratégia Delta" for "Controle de Alterações"
* Conector do Graph (visualização pública)
    * Erro em exclusões de grupo
    * Atualizar User-Agent para o cabeçalho http
    * O conector não valida a ID do cliente e o segredo do cliente
    * O nome do locatário preenchido deve ser cortado

### <a name="enhancements"></a>Melhorias:
* Lotus Notes: * Adicione a capacidade de aumentar o tempo limite por meio da interface do usuário
* Conector do Graph (visualização pública) * o atributo de senha é filtrado na importação para eliminar "exportar-não-reimportado".
    * Adicione suporte de $filter parâmetro de consulta limitado a operações com todos os filtros que funcionam na consulta Delta, também funcionará no conector * atualizado para usar nextLink diretamente em vez de extrair skipToken para obter detalhes de paginação [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/paging)

## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Problemas corrigidos:
* ConnectorsLog System.Diagnostics.EventLogInternal.InternalWriteEvent(Mensagem: um dispositivo anexado ao sistema não está funcionando) resolvido
* Nessa versão de conectores, será necessário atualizar o redirecionamento de associação de 3.3.0.0-4.1.3.0 para 4.1.4.0 em miiserver.exe.config
* Serviços Web genéricos:
    * A resposta JSON Válida não pôde ser salva na ferramenta de configuração foi resolvido
* SQL genérico:
    * A exportação sempre gera apenas a consulta atualização para a operação de exclusão. Adicionado para gerar uma consulta exclusão
    * A consulta SQL, que obtém os objetos para a operação de importação Delta, se ' estratégia Delta ' for ' Controle de Alterações ' foi corrigida. Nesta implementação de limitação conhecida: a importação Delta com o modo ' Controle de Alterações ' não controla alterações em atributos com valores múltiplos
    * Adição de possibilidade de gerar uma consulta de exclusão para o caso, quando for necessário excluir o último valor do atributo com valores diferentes e essa linha não contiver nenhum outro dado, exceto o valor, o que é necessário para excluir.
    * Manipulação System.ArgumentException quando implementado parâmetros OUTPUT por SP
    * Consulta incorreta para fazer a operação de exportação no campo, que tem o tipo varbinary (max)
    * Problema com a variável parameterList foi inicializado duas vezes (nas funções ExportAttributes e GetQueryForMultiValue)


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Problemas corrigidos:

* Lotus Notes:
  * Opção Filtrar certificados personalizados
  * Na importação da classe ImportOperations, foi corrigida a definição de quais operações podem ser executadas no modo “Exibições” e quais no modo “Pesquisa”.
* LDAP genérico:
  * O diretório OpenLDAP usa DN como âncora em vez de entryUUI. Nova opção para o conector do GLDAP, que permite modificar a âncora
* SQL genérico:
  * Corrigido a exportação no campo, que tem o tipo varbinary (max).
  * Ao adicionar dados binários de uma fonte de dados ao objeto CSEntry, a função DataTypeConversion a função falhava em zero bytes. A função DataTypeConversion da classe CSEntryOperationBase foi corrigida.




### <a name="enhancements"></a>Melhorias:

* SQL genérico:
  * A capacidade de configurar o modo de execução do procedimento armazenado com parâmetros nomeados ou não nomeados foi adicionado em uma janela de configuração do agente de gerenciamento do Generic SQL na página “Parâmetros Globais”. Na página "parâmetros globais", há uma caixa de seleção com o rótulo "usar parâmetros nomeados para executar um procedimento armazenado", que é responsável pelo modo de executar procedimento armazenado com parâmetros nomeados ou não.
    * Atualmente, a capacidade de executar o procedimento armazenado com parâmetros nomeados funciona apenas para bancos de dados IBM DB2 e MSSQL. Para bancos de dados Oracle e MySQL, essa abordagem não funciona: 
      * A sintaxe SQL do MySQL não dá suporte a parâmetros nomeados em procedimentos armazenados.
      * O driver ODBC para o Oracle não dá suporte a parâmetros nomeados para parâmetros nomeados em procedimentos armazenados)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Problemas corrigidos:

* Serviços Web genéricos:
  * Correção de um problema que estava impedindo um projeto SOAP de ser criado quando havia dois ou mais pontos de extremidade.
* SQL genérico:
  * Na operação de importação, o GSQL não estava convertendo o tempo corretamente, quando salvo no espaço do conector. O formato de data e hora padrão para o espaço do conector do GSQL foi alterado de "aaaa-MM-dd hh:mm:ssZ'' para "aaaa-MM-dd HH:mm:ssZ'.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Problemas corrigidos:

* Serviços Web genéricos:
  * A ferramenta WSconfig não converteu corretamente a matriz JSON de "solicitação de exemplo" para o método de serviço REST. Isso causou problemas com a serialização dessa matriz Json para a solicitação REST.
  * A Ferramenta de Configuração do Conector do Serviço Web não dá suporte ao uso de símbolos de espaço em nomes de atributo JSON 
    * Um padrão de substituição pode ser adicionado manualmente ao arquivo de WSConfigTool.exe.config, por exemplo,  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > A chave JSONSpaceNamePattern necessária, pois para exportação você receberá o erro a seguir: Mensagem: Nome vazio não é válido. 

* Lotus Notes:
  * Se a opção **permitir certificadores personalizados para organizações/unidades organizacionais** estiver desabilitada, o conector falhará durante a exportação (atualização) depois que todos os atributos do fluxo de exportação forem exportados para o Domino, mas no momento da exportação, um KeyNotFoundException será retornado para sincronização. 
    * Isso acontece porque a operação de renomeação falha ao tentar alterar o DN (atributo UserName) alterando um dos atributos abaixo:  
      - LastName
      - Nome
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  * Quando a opção **Permitir certificadores personalizados para a organização/as unidades organizacionais** está habilitada, mas os certificadores necessários ainda estão vazios, ocorre uma KeyNotFoundException.

### <a name="enhancements"></a>Melhorias:

* SQL genérico:
  * **Cenário: reprojetado implementado:** recurso "*"
  * **Descrição da solução:** abordagem alterada para a [manipulação de atributos de referência de vários valores](microsoft-identity-manager-2016-connector-genericsql.md).


### <a name="fixed-issues"></a>Problemas corrigidos:

* Serviços Web genéricos:
  * Não é possível importar a configuração do servidor se o conector WebService estiver presente
  * O Conector WebService não está funcionando com vários Serviços Web

* SQL genérico:
  * Nenhum tipo de objeto é listado para o atributo de referência de valor único
  * A importação delta na estratégia de Controle de Alterações exclui o objeto quando o valor é removido da tabela de vários valores
  * OverflowException no conector GSQL com DB2 no AS/400

Lotus:
  * Adicionada a opção de habilitar/desabilitar a pesquisa de UOs antes de abrir a página GlobalParameters

## <a name="114430"></a>1.1.443.0

Lançamento: março de 2017

### <a name="enhancements"></a>Aprimoramentos

* SQL genérico:</br>
  **Sintomas de cenário:** são uma limitação conhecida com o conector do SQL onde podemos permitir somente uma referência a um tipo de objeto e exigir uma referência cruzada com membros. </br>
  **Descrição da solução:** na etapa de processamento de referências onde a opção "*" é escolhida, todas as combinações de tipos de objeto são retornadas para o mecanismo de sincronização.

> [!Important]
> - Isso cria vários espaços reservados
> - Isso é necessário para garantir que o nome seja exclusivo entre tipos de objetos cruzados.


* LDAP genérico:</br>
  **Cenário:** quando apenas alguns contêineres são selecionados na partição específica e a pesquisa ainda será realizada na partição inteira. Específico será filtrado pelo serviço de sincronização, mas não pelo MA, que pode causar degradação do desempenho. </br>

  **Descrição de solução:** o código do conector GLDAP é modificado para passar por todos os contêineres e objetos de pesquisa em cada um deles, em vez de pesquisar na partição inteira.


* Lotus Domino:

  **Cenário:** o suporte para exclusão de email Domino para a remoção de pessoa durante uma exportação. </br>
  **Solução:** suporte para exclusão de email configurável para a remoção de pessoa durante uma exportação.

### <a name="fixed-issues"></a>Problemas corrigidos:
* Serviços Web genéricos:
  * Ao alterar a URL do serviço em projetos SAP WSconfig padrão por meio da ferramenta de configuração do WebService, o seguinte erro ocorrerá: não foi possível encontrar uma parte do caminho

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* LDAP genérico:
  * O conector de GLDAP não verá todos os atributos no AD LDS
  * Quebra de assistente quando não é detectado nenhum atributo UPN do esquema de diretório LDAP
  * Quando o atributo de "objectclass" não está selecionado, há falha na importação Delta com erros de descoberta não presentes durante a importação completa
  * Uma página de configuração "configurar partições e hierarquias" não mostra nenhum objeto que seja igual à partição para servidores de romance no Generic  
  LDAP MA. Eles exibem apenas objetos de partição RootDSE.


* SQL genérico:
  * Fixo para bug de atributo com multivalores de importação Delta de marca d'água de SQL não importado
  * Ao exportar valores excluídos/adicionados de atributo com multivalores, eles não são excluídos/adicionados na fonte de dados.  


* Lotus Notes:
  * Um campo específico "Nome completo" é mostrado corretamente no metaverso, no entanto, o valor do atributo se torna nulo ou vazio quando exportado para notas.
  * Fixo para o erro de certificador de duplicata
  * Quando o objeto sem dados é selecionado no conector Lotus Domino com outros objetos e, em seguida, recebemos o erro de descoberta ao executar a importação completa.
  * Às vezes, no final da execução da importação Delta no conector Lotus Domino, o serviço Microsoft.IdentityManagement.MA.LotusDomino.Service.exe retorna uma mensagem de erro de aplicativo.
  * A associação de grupo geral funciona bem e é mantida, exceto ao executar a exportação para tentar remover um usuário da associação que ele mostra como bem-sucedido com uma atualização, mas o usuário não é realmente removido da associação no Lotus Notes.
  * Uma oportunidade de escolher o modo de exportação como "acrescentar item na parte inferior" foi adicionada na GUI de configuração do Lotus MA para acrescentar novos itens na parte inferior durante a exportação para atributos com valores múltiplos.
  * O conector adicionará a lógica necessária para excluir o arquivo da pasta de email e o cofre de ID.
  * Exclua a associação que não estiver funcionando para membro NAB cruzado.
  * Os valores devem ser excluídos com êxito do atributo de multivalores

## <a name="111170"></a>1.1.117.0
Lançamento: março de 2016

**Novo Conector**  
Versão inicial do [Conector do SQL Genérico](microsoft-identity-manager-2016-connector-genericsql.md).

**Novos recursos:**

* Conector LDAP Genérico:
  * Adicionado suporte para importação delta com o Isode.
* Conector dos Serviços Web:
  * Atualizadas as atividades csEntryChangeResult e setImportErrorCode para permitir que erros no nível de objeto sejam retornados para o mecanismo de sincronização.
  * Atualizados os modelos SAP6 e SAP6User para usar a nova funcionalidade de erro no nível de objeto.
* Conector do Lotus Domino:
  * Para exportar, é necessário um certificador por catálogo de endereços. Agora é possível usar a mesma senha para todos os certificadores, a fim de facilitar o gerenciamento.

**Problemas corrigidos:**

* Conector LDAP Genérico:
  * Para o IBM Tivoli DS, alguns atributos de referência não foram detectados corretamente.
  * Para o Open LDAP durante a importação delta, os espaços em branco no início e no final das cadeias de caracteres foram truncados.
  * Para o Novell e NetIQ, houve falha em uma exportação que movia um objeto entre UOs/contêineres e ao mesmo tempo renomeava o objeto.
* Conector dos Serviços Web:
  * Se o serviço Web tinha vários pontos de extremidade para a mesma associação, o conector não os descobriu corretamente.
* Conector do Lotus Domino:
  * Uma exportação do atributo fullName para um banco de dados de entrada de email não funcionou.
  * Uma exportação que adicionava e removia um membro de um grupo exportou apenas os membros adicionados.
  * Se um Documento do Notes for inválido (o atributo isValid for definido como false), haverá uma falha do Conector.

## <a name="older-releases"></a>Versões mais antigas
Antes de março de 2016, os Conectores foram liberados como tópicos de suporte.

**LDAP genérico**

* [KB3078617](https://support.microsoft.com/kb/3078617) – 1.0.0597, setembro de 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) – 1.0.0549, março de 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) – 1.0.0534, janeiro de 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) – 1.0.0419, setembro de 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) – 4.3.1082, março de 2014

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) – 1.0.0419, setembro de 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) – 1.0.0419, setembro de 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) – 1.0.0597, setembro de 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) – 1.0.0549, março de 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) – 5.3.0712, agosto de 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) – 5.3.1003, fevereiro de 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) – 5.3.0721, outubro de 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) – 5.3.0534, agosto de 2013

## <a name="troubleshooting"></a>Solução de problemas 

> [!NOTE]
> Ao atualizar o Microsoft Identity Manager ou o AADConnect usando um dos conectores ECMA2. 

É necessário atualizar a definição do conector ao fazer atualização para corresponder, caso contrário, você receberá o seguinte erro no início do log de eventos do aplicativo relatando o aviso ID 6947: “A versão do assembly na configuração do AAD Connector (“X.X.XXX.X”) é anterior à versão real (“X.X.XXX.X”) de ‘C:\Arquivos de Programas\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

Para atualizar a definição:
* Abra as Propriedades da instância do Conector
* Clique na guia Conexão / Conectar a
  * Insira a senha para a conta do Conector
* Clique em cada guia de propriedade, uma por vez
  * Se esse tipo de Conector tiver uma guia Partições com um botão Atualizar, clique nele enquanto estiver na guia
* Depois que todas as guias de propriedade tenham forem acessadas, clique no botão OK para salvar as alterações.

## <a name="other-connectors"></a>Outros conectores

Além dos conectores listados acima, os conectores para SharePoint e um conector herdado para Windows Azure Active Directory, também foram distribuídos separadamente do MIM.

### <a name="sharepoint-user-profile"></a>Perfil de usuário do SharePoint

O conector do Forefront Identity Manager para o armazenamento de perfil de usuário do SharePoint ajuda a sincronizar informações de identidade para o repositório de perfis de usuário no SharePoint 2013 e no SharePoint 2016.   4.3.2430.0 de versão deste conector, publicado 12/19/2016 pode ser baixado do [centro de download da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=41164).

Mais informações sobre esse conector podem ser encontradas no [pacote cumulativo de atualizações do hotfix](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man) e instruções sobre como [usar uma solução mim de exemplo no SharePoint Server 2016](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### <a name="forefront-identity-manager-connector-for-windows-azure-active-directory-legacy-connector"></a>Conector do Forefront Identity Manager para Windows Azure Active Directory (conector herdado)

O Azure AD Connector para FIM foi uma tecnologia inicial para sincronizar informações de identidade para Azure Active Directory. O Azure AD Connector para FIM, versão 1.0.6635.0069 de 19 de fevereiro de 2014, está no congelamento do recurso. [Ele](https://www.microsoft.com/en-us/download/details.aspx?id=41166) não recebe nenhuma atualização, mas ainda tem suporte. A solução de usar o FIM e o Azure AD Connector foi substituída pelo [Azure ad Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect). A Microsoft recomenda que você não inicie uma nova implantação usando esse conector.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre a documentação de referência do [conector LDAP genérico](microsoft-identity-manager-2016-connector-genericldap.md) .
Saiba mais sobre a documentação genérica de referência do[conector do SQL](microsoft-identity-manager-2016-connector-genericsql.md) .
Saiba mais sobre a documentação de referência do [conector de serviços Web](microsoft-identity-manager-2016-ma-ws.md) .
Saiba mais sobre a documentação de referência do [conector do PowerShell](microsoft-identity-manager-2016-connector-powershell.md) .
Saiba mais sobre a documentação de referência do [conector do Lotus Domino](microsoft-identity-manager-2016-connector-domino.md) .
