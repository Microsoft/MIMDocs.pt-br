---
title: Guia de fluxo de trabalho do conector de serviço Web para SOAP | Microsoft Docs
description: Este artigo descreve como criar um novo projeto para sua fonte de dados SOAP usando a ferramenta de configuração do serviço Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 455988722b3aeb9e29b00696342e1800e9ad82c7
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835964"
---
# <a name="web-service-connector-workflow-guide-for-soap"></a>Guia de fluxo de trabalho do conector de serviço Web para SOAP

Este artigo descreve como criar um novo projeto para sua fonte de dados na ferramenta de configuração do serviço Web. Siga estas etapas para criar um projeto.

1.  Abra a ferramenta de configuração do serviço Web. Ele abre um projeto em branco.

    ![Ferramenta de configuração do serviço Web](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-01.png)

2.  Selecione **projeto SOAP** e, em seguida, selecione **Adicionar**.

    ![Projeto SOAP](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-02.png)

3.  Na página seguinte, forneça as seguintes informações e, em seguida, selecione **Avançar**:

    - O novo nome do serviço Web
    - Endereço (caminho WSDL) para recuperar os serviços expostos, os pontos de extremidade e as operações
    - Namespace
    - Modo de segurança (tipo de autenticação)
  
4.  Neste exemplo, a página **credenciais** é mostrada com os requisitos para o modo de segurança *básica* (o modo que foi selecionado na etapa anterior). Se "nenhum" tiver sido especificado para o modo de segurança, uma página de credenciais não será exibida. Selecione **Avançar**.

    ![Tela de serviço SOAP com nome de usuário e senha](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service.png)

5.  O caminho WSDL está sendo acessado para recuperar as informações de serviço e a lista de funções expostas é exibida. Se o caminho WSDL inserido estiver incorreto, a ferramenta de configuração falhará ao recuperar as informações do serviço e lançará um erro.

    ![tela de progresso do download do serviço Web](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-progress.png)

6.  Depois que a descoberta é executada, ela lista o ponto de extremidade e as operações que são descobertas. Selecione **Concluir**.

    ![Pontos de extremidade de serviço SOAP e operações descobertos](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service-endpoints.png)

7.  A compilação é executada. A compilação é um processo de compilação do assembly de contrato de dados, que pode ser uma operação demorada. O usuário é informado sobre quaisquer erros de compilação. Depois que a descoberta for executada, a ferramenta exibirá a seguinte página:

    ![Descoberta de SOAP](media/microsoft-identity-manager-2016-ma-ws-soap/soap-discovery.png)

8.  Expandindo o **projeto SOAP** e selecionando ponto de extremidade exposto fornecido abaixo da tela. Esta tela lista as operações declaradas no ponto de extremidade.

    ![Operações declaradas no ponto de extremidade](media/microsoft-identity-manager-2016-ma-ws-soap/basic-http-binding.png)

9.  A expansão do ponto de extremidade exibe a lista de operações. Uma operação é uma função declarada por um ponto de extremidade. Cada operação resolve um tipo de tarefa que pode ser executada dentro do serviço. Esta tela lista os argumentos declarados para a operação. Esses argumentos são definidos quando a operação é usada na configuração dos fluxos de trabalho.

    ![Pontos de extremidade expandidos](media/microsoft-identity-manager-2016-ma-ws-soap/get-employee-byid.png)

10. A próxima etapa é definir o esquema de espaço do conector, que é obtido criando-se o tipo de objeto e definindo seus tipos de objeto. Selecione **tipos de objeto** e, em seguida, selecione **Adicionar**. Na nova janela, adicione um novo tipo de objeto e forneça um nome. Selecione **OK**.

    ![Definindo o tipo de objeto](media/microsoft-identity-manager-2016-ma-ws-soap/object-types.png)

11. A adição de um tipo de objeto fornece a tela abaixo.

    ![exibindo tipo de objeto recém-criado](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-employee.png)

12. O painel direito correspondente ao tipo de objeto permite que você mantenha os atributos e suas propriedades para o tipo de objeto selecionado. Selecione **Adicionar**. Uma nova janela é aberta para adicionar atributos:

    ![Atributo e tipo de dados](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-firstname.png)

    ![Atributo e tipo de dados com a opção de âncora selecionada](media/microsoft-identity-manager-2016-ma-ws-soap/employeeid-string.png)

13. A tela a seguir aparece depois de adicionar todos os atributos necessários:

    ![Tipo de objeto com informações de atributo](media/microsoft-identity-manager-2016-ma-ws-soap/soap-project.png)

14. Os atributos e o tipo de objeto, uma vez criados, fornecem fluxos de trabalho em branco que atendem às operações executadas no MIM (Microsoft Identity Manager 2016).

    ![Tipos de objeto mostra as operações que o funcionário pode executar](media/microsoft-identity-manager-2016-ma-ws-soap/object-types-operations.png)


## <a name="configure-workflows-in-the-web-service-configuration-tool"></a>Configurar fluxos de trabalho na ferramenta de configuração do serviço Web

A próxima etapa é configurar os fluxos de trabalho para o tipo de objeto. Os arquivos de fluxo de trabalho são uma série de atividades que são usadas pelo conector de serviços da Web em tempo de execução. Os fluxos de trabalho são usados para implementar a operação apropriada do MIM. A ferramenta de configuração do serviço Web ajuda a criar quatro fluxos de trabalho diferentes:

- Importar: importe dados de uma fonte de dados para os dois tipos de fluxos de trabalho a seguir:

    - Importação completa: uma importação completa que pode ser configurada.
    - Importação Delta: não há suporte para a ferramenta de configuração do serviço Web.

- Exportar: exportar dados do MIM para uma fonte de dados conectada. As três ações a seguir têm suporte para a operação. Você pode configurar essas ações com base em seus requisitos.

    - Adicionar
    - Excluir
    - Substitua

- Senha: executar o gerenciamento de senha para o usuário (tipo de objeto). Há duas ações disponíveis para esta operação:

    - Definir senha
    - Alterar senha

- Testar conexão: Configure um fluxo de trabalho para verificar se a conexão com o servidor de fonte de dados foi estabelecida com êxito.

>[!NOTE]
>Você pode configurar esses fluxos de trabalho para seu projeto ou baixar o projeto padrão do [centro de download da Microsoft](https://www.microsoft.com/download/details.aspx?id=29944).


### <a name="workflow-designer"></a>Designer de Fluxo de Trabalho
O Designer de Fluxo de Trabalho abre a área de trabalho para configurar o fluxo de trabalhos de acordo com o requisito. Para cada tipo de objeto (New/existing), a ferramenta de configuração fornece os nós para fluxos de trabalho com suporte da ferramenta. 

![Designer de Fluxo de Trabalho](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-workflow.png)

O Designer de Fluxo de Trabalho é composto pelos seguintes elementos da interface do usuário:

   - **Nós no painel esquerdo**: eles ajudam a selecionar qual deseja criar o fluxo de trabalho.

   - **Designer de fluxo de trabalho central**: aqui, você pode descartar as atividades para configurar os fluxos de trabalho. Para realizar várias operações do MIM (exportação, importação e gerenciamento de senhas), você pode usar as atividades de fluxo de trabalho padrão e personalizadas do .NET Workflow Framework 4. A ferramenta de configuração do serviço Web usa atividades de fluxo de trabalho padrão e personalizadas. Para obter mais informações sobre as atividades padrão, consulte [usando os designers de atividade](https://msdn.microsoft.com/library/ee829528.aspx).

      - No Designer de Fluxo de Trabalho central, um círculo vermelho com ponto de exclamação ao lado de qualquer atividade indica que a operação foi descartada e não está definida corretamente e completamente. Passe o mouse sobre o círculo vermelho para descobrir o erro exato. Depois que a atividade é definida corretamente, o círculo vermelho muda para a marca de informações amarela.
      
      - No Designer de Fluxo de Trabalho central, uma marca de informações de triângulo amarelo ao lado de qualquer atividade indica que a atividade está definida, mas há mais que você pode fazer para concluir a atividade. Passe o mouse sobre o triângulo amarelo para ver mais informações.

   - **Caixa de ferramentas**: empacota todas as ferramentas, incluindo atividades personalizadas e do sistema, e instruções predefinidas para criar o fluxo de trabalho. Para saber mais, confira [Caixa de Ferramentas](https://msdn.microsoft.com/library/aa480213.aspx).
   
   - **Seções da caixa de ferramentas**: a caixa de ferramentas tem as seguintes seções e categorias:
   
      - **Descrição**: o cabeçalho da caixa de ferramentas. Uma guia acessa a caixa de ferramentas e as propriedades da atividade de fluxo de trabalho selecionada. 

      - **Importar fluxo de trabalho**: atividades personalizadas para configurar fluxos de trabalho de importação.
      
      - **Exportar fluxo de trabalho**: atividades personalizadas para configurar fluxos de trabalho de exportação.
      
      - **Comum**: atividades personalizadas para configurar qualquer fluxo de trabalho.
      
      - **Depuração**: atividades de fluxo de trabalho do sistema para depuração definidas no fluxo de trabalho 4. Essas atividades permitem o acompanhamento de problemas de um fluxo de trabalho.
      
      - **Instruções**: atividades de fluxo de trabalho do sistema definidas no fluxo de trabalho 4. Para obter mais informações, consulte [usando os designers de atividade](https://msdn.microsoft.com/library/ee829528.aspx).            

   - **Propriedades**: a guia Propriedades exibe as propriedades de uma atividade de fluxo de trabalho específica que é descartada na área do designer e selecionada. A figura à esquerda mostra as propriedades da atividade **atribuir** . Para cada atividade, as propriedades são diferentes e são usadas ao configurar o fluxo de trabalho personalizado. Esta guia permite que você defina os atributos da ferramenta selecionada que foi descartada no designer de fluxo de trabalho central. Para obter mais informações, consulte [Propriedades](https://msdn.microsoft.com/library/ee342461.aspx).

   - **Barra de tarefas:** A barra de tarefas inclui três elementos: **variáveis**, **argumentos** e **importações**. Esses elementos são usados junto com as atividades de fluxo de trabalho. Para obter mais informações, consulte [a introdução do desenvolvedor ao Windows Workflow Foundation (WF) no .NET 4](https://msdn.microsoft.com/library/ee342461.aspx).


<h2 id="full-import-workflows">Configurar um fluxo de trabalho de importação completa na ferramenta de configuração do serviço Web</h2>
As etapas a seguir mostram como configurar fluxos de trabalho de importação completos para SOAP usando a ferramenta de configuração do serviço Web.

>[!WARNING]
>Este exemplo cria apenas um fluxo de trabalho. As modificações no fluxo de trabalho, como para usar a lógica personalizada na API, podem ser necessárias.

1. Selecione o fluxo de trabalho de importação completa a ser configurado. Os **argumentos** e as **importações** já estão definidos e são específicos para as atividades. Consulte as telas a seguir para obter mais informações.

   ![Argumentos de fluxo de trabalho de importação completa](media/microsoft-identity-manager-2016-ma-ws-soap/arguments.png)
 
   ![Namespaces importados](media/microsoft-identity-manager-2016-ma-ws-soap/imports.png)

   Após a reconfiguração das chamadas, altere os nomes dos atributos que mudam, adicione ou altere o namespace para variáveis que se referem à estrutura de retorno dos tipos de API e de objeto que se referem ao namespace antigo. A caixa de ferramentas no painel direito contém todas as atividades personalizadas específicas do fluxo de trabalho que você precisa para a configuração. Atribua os valores às variáveis que você pretende usar para sua lógica. Vá para a seção inferior do designer de fluxo de trabalho central e declare as variáveis. As variáveis são declaradas na próxima etapa.

2. Adicione uma atividade de sequência. Arraste o designer de atividade de **sequência** da **caixa de ferramentas** e solte-o na superfície de designer de fluxo de trabalho do Windows. Consulte as telas a seguir. A atividade [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) contém uma coleção ordenada de atividades filho que ela executa na ordem.
   
    ![Atividade de sequência](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-sequence.png)

3. Para adicionar uma variável, localize **criar variável**. Digite _wsResponse_ para o **nome**, selecione a lista suspensa **tipo de variável** e, em seguida, selecione **procurar tipos**. Uma caixa de diálogo é exibida. Selecione a  >    >  **resposta** padrão gerada. Mantenha o **escopo** e os valores **padrão** desmarcados. Como alternativa, defina esses valores usando a exibição **Propriedades** .

   ![Resposta padrão](media/microsoft-identity-manager-2016-ma-ws-soap/default-response.png)

   ![Propriedades de importação completa](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-properties.png)

4. Agora, adicione todas as outras variáveis e abaixo está a tela final.

   ![Variáveis de importação completas](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-variables.png)

5. Arraste mais um designer de atividade de **sequência** da **caixa de ferramentas** dentro da atividade de sequência já adicionada.

6. Arraste um **WebServiceCallActivity** apresentado em **comum.** Essa atividade é usada para invocar a operação de serviço Web disponível após a descoberta. Essa é uma atividade personalizada e é comum em cenários de operação diferentes. 

    ![Operação de nome de serviço](media/microsoft-identity-manager-2016-ma-ws-soap/service-name-operation.png)

   Para usar a operação de serviço Web, defina as seguintes propriedades:
   
      - **Nome do serviço**: Insira um nome para o serviço Web.
      - **Nome do ponto de extremidade**: especifique um nome de ponto de extremidade para o serviço selecionado.
      - **Nome da operação**: Especifique a respectiva operação para o serviço.
      - **Argumento**: selecione **argumentos**. Na próxima caixa de diálogo, atribua os valores de argumento, conforme mostrado na figura a seguir:
      
         ![Atribuir argumentos](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

         >[!IMPORTANT]
         >Não altere o **nome**, a **direção** ou o **tipo** de um argumento usando esta caixa de diálogo. Se qualquer um desses valores for alterado, a atividade se tornará inválida. Defina apenas o **valor** para o argumento. Conforme mostrado nesta figura, o valor *wsResponse* é definido.

7. Adicione uma atividade **foreach** logo abaixo de **WebServiceCallActivity.** Essa atividade é usada para iterar em todos os atributos (âncoras e não-âncoras) do tipo de objeto. Ao arrastar essa atividade para sua superfície de Designer de Fluxo de Trabalho, ela enumera automaticamente todos os nomes de atributo para o objeto. Defina os valores necessários de acordo com a tela a seguir:

   ![Atividade de chamada de serviço Web](media/microsoft-identity-manager-2016-ma-ws-soap/webservicecallactivity.png)

8. Arraste uma atividade **CreateCSEntryChangeScope** no corpo **foreach** . Essa atividade é usada para criar uma instância do objeto CSEntryChange no domínio do fluxo de trabalho para cada registro respectivo ao recuperar dados da fonte de dados de destino. Arrastar essa atividade fornece a tela abaixo. As atividades **ancoraattribute** são herdadas automaticamente.

    ![Criar atividade de CS entrada de escopo de alteração](media/microsoft-identity-manager-2016-ma-ws-soap/createcsentrychangescope.png)

9.  Defina o valor da expressão DN como `‘string.Concat ("Employee",item.EmployeeID)’` . Defina **Ancorable** para _EmployeeID_ como **' Convert. ToString (item. EmployeeID) '**. Defina **Objecttypename** como _funcionário_. Depois de fazer essas modificações, você verá a tela a seguir:

    ![Obter a ID do funcionário](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

    >[!NOTE]
    >Valores de âncora e nomes de objeto variam de acordo com o serviço Web exposto. A figura mostra um exemplo.

10. Arraste uma atividade **CreateAttributeChange** abaixo da atividade **ancoraattribute** . O número de atividades a serem arrastadas é igual ao número de atributos que não são de âncora. Consulte a figura a seguir para obter referência.

    ![Criar âncora](media/microsoft-identity-manager-2016-ma-ws-soap/create-anchor.png)

11. Arraste **CreateValueChangeActivity** na atividade **CreateAttributeChange** e defina o valor do atributo de acordo com a tela abaixo.

    ![Alterar um atributo](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change.png)

    >[!NOTE]
    >Para usar essa atividade, escolha e atribua os respectivos campos da lista suspensa e atribua os valores. Para atributos com vários valores, descarte várias atividades **CreateValueChangeActivity** dentro de uma atividade **CreateAttributeChangeActivity** .

12. Para adicionar condições para um atributo, adicione uma atividade **If** , conforme mostrado na figura a seguir:

    ![Atividade If](media/microsoft-identity-manager-2016-ma-ws-soap/if.png)

13. Por fim, adicione uma atividade **assign** e defina a expressão, conforme mostrado na figura a seguir:

    ![Atribuir atividade e definir a expressão](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change-email.png)

14. Salve este projeto no local `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .
    
    Os projetos padrão devem ser baixados e salvos no local `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` no sistema de destino. Os projetos são então visíveis no assistente do conector de serviço da Web.
    
    Ao executar o arquivo executável, você será solicitado a especificar o local para a instalação. Insira o local para salvar.
    
    >[!IMPORTANT]
    >O arquivo de projeto pode ser salvo e aberto em qualquer local (com os privilégios de acesso apropriados de seu executor). Somente arquivos de projeto que são salvos na `Synchronization Service\Extension` pasta podem ser selecionados no assistente de conector de serviço da Web que é acessado por meio da interface do usuário de sincronização do mim.
    
    O usuário que está executando a ferramenta de configuração do serviço Web requer os seguintes privilégios:
    
       - Controle total para a pasta de extensão do serviço de sincronização.
       - Acesso de leitura à chave do registro `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` por meio da qual o caminho da pasta de extensão está localizado.


## <a name="configure-export-workflows-in-the-web-service-configuration-tool"></a>Configurar fluxos de trabalho de exportação na ferramenta de configuração do serviço Web
As seções a seguir mostram como exportar seus fluxos de trabalho usando a ferramenta de configuração do serviço Web.

<h3 id="attribute-change-anchor">Adicionar fluxos de trabalho</h3>
Adicione fluxos de trabalho de exportação seguindo estas etapas na ferramenta de configuração do serviço Web.

1. Selecione o fluxo de trabalho de exportação a ser configurado. Em **Exportar**, selecione **Adicionar**. Os **argumentos** e as **importações** já estão definidos e são específicos para as atividades. Consulte as telas a seguir para referência.

    ![Adicionar](media/microsoft-identity-manager-2016-ma-ws-soap/add.png)

2. Adicione uma atividade de **sequência** . Arraste o designer de atividade de **sequência** da **caixa de ferramentas** e solte-o na superfície de designer de fluxo de trabalho do Windows. A atividade [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) contém uma coleção ordenada de atividades filho que ela executa na ordem. Selecione **criar variável**. Atribua os valores às variáveis que você pretende usar para sua lógica.

    ![Exportação](media/microsoft-identity-manager-2016-ma-ws-soap/export-add.png)

    >[!NOTE]
    >As etapas para adicionar uma variável são descritas na seção para criar <a href="#full-import-workflows">fluxos de trabalho de importação completa</a>.

3. Arraste uma atividade **foreach** dentro de uma atividade de **sequência** já adicionada para iterar os valores de atributo de âncora.

4. Selecione **Propriedades** e defina os **valores** de acordo com a tela abaixo. Aqui, **objectToExport** é argumento.

    ![Definir as propriedades para a atividade ForEach](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-sequence.png)

5. Definir **DisplayName** como **ForEach \<AnchorAttribute\>**

   ![Definir o nome de exibição](media/microsoft-identity-manager-2016-ma-ws-soap/add-sequence.png)

6. Defina **TypeArgument** como `Microsoft.MetadirectoryServices.AnchorAttribute` .

   ![Definir o argumento de tipo](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor.png)

7. Adicione uma atividade de **comutador** no corpo **foreach** do **ancoraattribute**.

   ![Adicionar uma atividade de alternância](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-types.png)

8. Adicione uma expressão de acordo com a tela abaixo.

   ![Adicionar uma expressão](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Selecione **Adicionar um novo caso** e insira um valor para o **EmployeeID**. Arraste uma atividade **Sequence** e, dentro dela, adicione uma atividade **assign** .

    ![Adicionar um novo caso e atribuí-lo à sequência](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-employeeid.png)

10. Atribua as propriedades **para** e **valor** para a atividade **atribuir** .

    ![Atribuir as propriedades para e valor](media/microsoft-identity-manager-2016-ma-ws-soap/anchor-attribute-name.png)

11. A atividade **foreach** é usada para valores de âncora. Adicione outra atividade **foreach** para atribuir valores não âncora. Neste exemplo, a âncora **AttributeChange** é usada.

    ![Adicionar outra atividade ForEach com a âncora AttributeChange](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-change.png)

12. Adicione uma atividade **switch** no corpo **foreach** da âncora **AttributeChange** .   

    ![Adicionar atividade de comutador para a âncora AttributeChange](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-name-wrapper.png)

13. Adicione uma expressão de acordo com a tela abaixo.

    ![Adicionar uma expressão para a atividade Switch](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-expression.png)

14. Selecione **Adicionar um novo caso** e insira um valor para o **nome**. Arraste uma atividade **Sequence** e, dentro dela, adicione uma atividade **assign** . Atribua as propriedades **para** e **valor** para a atividade **atribuir** .

    ![Adicionar um novo caso à sequência](media/microsoft-identity-manager-2016-ma-ws-soap/switch-firstname.png)

15. Adicione valores para os atributos necessários, como **LastName**, **email** e assim por diante. 

    ![Adicionar valores para atributos necessários](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

16. Em **comum**, arraste um **WebServiceCallActivity** e defina **valores** para seus **argumentos**.

    ![Adicionar uma atividade de chamada de serviço Web e definir os valores](media/microsoft-identity-manager-2016-ma-ws-soap/add-employee-attribute.png)

    >[!IMPORTANT]
    >Não altere o **nome**, a **direção** ou o **tipo** de um argumento usando esta caixa de diálogo. Se qualquer um desses valores for alterado, a atividade se tornará inválida. Defina apenas o **valor** para o argumento. Conforme mostrado nesta figura, o valor *wsResponse* é definido.

17.  Por fim, adicione uma atividade **If** para verificar as respostas retornadas da operação do serviço Web.

A criação do fluxo de trabalho de exportação com a operação de **adição** foi concluída:

![Fluxo de trabalho de exportação concluído](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

Salve este projeto no local `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="delete-workflows"></a>Excluir fluxos de trabalho
Exclua os fluxos de trabalho de exportação seguindo estas etapas na ferramenta de configuração do serviço Web.

1. Selecione o fluxo de trabalho de exportação a ser configurado. Em **Exportar**, selecione **excluir**. Os **argumentos** e as **importações** já estão definidos e são específicos para as atividades. Consulte as telas a seguir para referência.

   ![Exportar fluxos de trabalho de exclusão](media/microsoft-identity-manager-2016-ma-ws-soap/export-delete.png)

2. Adicione uma atividade de **sequência** . Selecione **criar variável**. Atribua os valores às variáveis que você pretende usar para sua lógica.

   ![Adicionar uma atividade de sequência](media/microsoft-identity-manager-2016-ma-ws-soap/sequence-variables.png)

   >[!NOTE]
   >As etapas para adicionar uma variável são descritas na seção para criar <a href="#full-import-workflows">fluxos de trabalho de importação completa</a>.

3. Arraste uma atividade **foreach** dentro de uma atividade de **sequência** já adicionada para iterar os valores de atributo de âncora.

4. Selecione **Propriedades** e defina os **valores** por tela abaixo. Aqui, **objectToExport** é argumento.

   ![Definir as propriedades para a atividade ForEach](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-object-to-export.png)

5. Defina o **DisplayName** como `ForEach\<AnchorAttribute\>` :

   ![Definir o nome de exibição](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor-type.png)

6. Defina o **TypeArgument** como `Microsoft.MetadirectoryServices.AnchorAttribute` : 

   ![Definir o argumento de tipo](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-type-argument.png)

7. Adicione uma atividade de **comutador** no corpo **foreach** do **ancoraattribute**.

   ![Adicionar uma atividade de alternância](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-type.png)

8. Adicione uma expressão de acordo com a tela abaixo.

   ![Adicionar uma expressão](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Selecione **Adicionar um novo caso** e insira um valor para o **EmployeeID**. Arraste uma atividade **Sequence** e, dentro dela, adicione uma atividade **assign** .

   ![Adicionar um novo caso e atribuí-lo à sequência](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-default.png)

10. Atribua as propriedades **para** e **valor** para a atividade **atribuir** .

    ![Atribuir as propriedades para e valor](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-attribute-flow.png)

11. Em **comum**, arraste um **WebServiceCallActivity** e defina **valores** para seus **argumentos**.

    ![Adicionar uma atividade de chamada de serviço Web e definir os valores](media/microsoft-identity-manager-2016-ma-ws-soap/delete-employee.png)

    >[!IMPORTANT]
    >Não altere o **nome**, a **direção** ou o **tipo** de um argumento usando esta caixa de diálogo. Se qualquer um desses valores for alterado, a atividade se tornará inválida. Defina apenas o **valor** para o argumento. Conforme mostrado nesta figura, o valor *EmployeeID* é definido.

12. Finalmente, adicione uma atividade **If** para verificar as respostas retornadas da operação do serviço Web.

A exclusão do fluxo de trabalho de exportação com a operação de **exclusão** foi concluída:

![Fluxo de trabalho de exportação excluído](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

<!-- Image of completed Delete operation is missing from document -->

Salve este projeto no local `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="replace-workflows"></a>Substituir fluxos de trabalho
Substitua fluxos de trabalho de exportação seguindo estas etapas na ferramenta de configuração do serviço Web.

1. Selecione o fluxo de trabalho de exportação a ser configurado. Em **Exportar**, selecione **substituir**. Os **argumentos** e as **importações** já estão definidos e são específicos para as atividades. Veja a tela abaixo para referência.

   ![Substituir um fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws-soap/replace.png)

2. Adicione uma atividade de **sequência** .

3. Arraste uma atividade **foreach** para o **\<AnchorAttribute> .**

4. Adicione outra **atividade \<AttributeChange> foreach** para atribuir valores não âncora.

5. Por fim, a tela é parecida com a figura a seguir. As instruções para configurar essa atividade são fornecidas na seção para <a href="#attribute-change-anchor">Adicionar fluxos de trabalho de exportação</a>.

   ![ForEach com uma atividade Switch e um atributo Anchor](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

6. Em **comum**, arraste um **WebServiceCallActivity** e defina **valores** para seus **argumentos**.

   ![Adicionar uma atividade de chamada de serviço Web e definir os valores](media/microsoft-identity-manager-2016-ma-ws-soap/wsresponse.png)

   >[!IMPORTANT]
   >Não altere o **nome**, a **direção** ou o **tipo** de um argumento usando esta caixa de diálogo. Se qualquer um desses valores for alterado, a atividade se tornará inválida. Defina apenas o **valor** para o argumento. Como mostrado nesta figura, o valor *Employee* é definido.

7. Por fim, adicione uma atividade **If** para verificar as respostas retornadas da operação do serviço Web.

A substituição do fluxo de trabalho de exportação com a operação de **substituição** foi concluída:

![Substituir fluxo de trabalho de exportação](media/microsoft-identity-manager-2016-ma-ws-soap/operation-name-update-employee.png)

Salve este projeto no local `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


## <a name="debug-activities"></a>Atividades de depuração
As seguintes atividades personalizadas estão disponíveis para ajudar a depurar o modelo de fluxo de trabalho.

### <a name="log-activity"></a>Atividade de log
A atividade **log** é usada para gravar mensagens de texto no arquivo de log. Para obter mais informações, consulte [Logging](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx).

>[!NOTE]
>Se você não puder depurar facilmente seu fluxo de trabalho, tente depurar o fluxo de trabalho no ambiente de produção.  

Para usar a atividade **log** , defina as propriedades a seguir. As propriedades são visíveis quando você seleciona a atividade em Designer de Fluxo de Trabalho e exibe as **Propriedades** da atividade.
<!-- Properties missing from document -->
<!-- Image of Log activity GUI missing from document -->

### <a name="writeline-activity"></a>Atividade WriteLine
A atividade **WriteLine** é usada para gravar mensagens de texto no gravador de um provedor. Se nenhum gravador estiver disponível, a atividade [WriteLine](http://127.0.0.1:47873/help/1-7016/ms.help?method=page&id=T%3ASYSTEM.ACTIVITIES.STATEMENTS.WRITELINE&product=VS&productVersion=100&topicVersion=100&locale=EN-US&topicLocale=EN-US&embedded=true) gravará o texto na janela do console.

<!-- Image of WriteLine activity GUI missing from document -->

Na caixa de texto, escreva a mensagem que você deseja que fique visível no destino do gravador.

>[!IMPORTANT]
>A janela do console não pode ser usada para esta atividade. Use outro gravador de saída da janela para esta tarefa.

Para usar a atividade **WriteLine** , defina as propriedades a seguir. As propriedades são visíveis quando você seleciona a atividade em Designer de Fluxo de Trabalho e exibe as **Propriedades** da atividade.

- **Nível de log**: especifica a quantidade de conteúdo a ser gravada no valor de log. Os valores possíveis são:

    - Alta: grave a mensagem **LogText** no arquivo de log se a severidade do log estiver definida como alta.
    - Verbose: grave a mensagem **LogText** no arquivo de log se a severidade do log estiver definida como Verbose.
    - Desabilitado: não grava no arquivo de log.
- **LogText**: especifica o conteúdo de texto a ser gravado no log.
- **Marca**: adiciona uma marca ao texto para identificar o tipo de conteúdo que está sendo gravado no log. Os valores possíveis são: Error, Trace ou Warning.

<!-- log severity is not defined in this document -->


## <a name="next-steps"></a>Próximas etapas

- [Visão geral do conector de serviço Web genérico](microsoft-identity-manager-2016-ma-ws.md)
- [Instalar a ferramenta de configuração do serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implantação de SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração de MA do serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
