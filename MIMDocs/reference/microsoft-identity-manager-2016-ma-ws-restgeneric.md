---
title: Guia de fluxo de trabalho do conector de serviço Web para a API REST | Microsoft Docs
description: Este artigo aborda como implantar um exemplo de API REST.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9b1a65d6604f434d3619ad7964caa8ce202092a0
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835947"
---
# <a name="web-service-connector-workflow-guide-for-a-rest-api-sample"></a>Guia de fluxo de trabalho do conector de serviço Web para exemplo de API REST

Este artigo aborda a implantação de uma API REST de exemplo para percorrer a ferramenta de configuração do serviço Web com uma fonte de dados da Web da API REST.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos são necessários para usar o exemplo:

- A ferramenta de configuração do serviço Web do está instalada.
- O serviço de exemplo de fonte de dados REST está implantado. Baixe e instale o exemplo de (veja aqui).

<!-- No link provided for "see here" -->
<!-- Should Note go with bullet point #2 -->

>[!NOTE]
>Os dados JSON devem conter um único objeto com uma propriedade que contenha uma matriz.

<!-- Should JSON be exactly as-is or just a sample -->
<!-- Should JSON be part of Note content -->
```JSON
{

"EmployeeList":[

{"id":"1","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""},{"id":"2","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""}

]

}
```

## <a name="configure-rest-project-discovery-in-the-web-service-configuration-tool"></a>Configurar a descoberta de projetos REST na ferramenta de configuração do serviço Web
As etapas a seguir mostram como criar um novo projeto para sua fonte de dados na ferramenta de configuração do serviço Web.

1. Abra a ferramenta de configuração do serviço Web. Ele abre um projeto SOAP em branco.

   ![Ferramenta de configuração do serviço Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/web-service-configuration-tool.png)

2. Selecione **arquivo**  >  **novo**  >  **projeto REST**.

   ![Criar um novo projeto REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/new-project.png)
   <!-- Image shows SOAP project selected, not REST project -->

3. À esquerda, selecione **projeto REST** e, em seguida, selecione **Adicionar**.

   ![Selecione o projeto REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-project.png)

4. Na próxima página, forneça as seguintes informações:

   - O novo nome do serviço Web
   - Endereço (caminho da URL da API REST)
   - Namespace
   - Modo de segurança (tipo de autenticação)

   ![Serviço REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service.png)
    
   A tela a seguir mostra exemplos para esses valores:
    
   ![Valores de exemplo para o serviço REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/restsample.png)

   Defina o **modo de segurança** como _nenhum_. Defina o **endereço** para o servidor JSON de exemplo que está hospedado no Azure.

5. Selecione **OK**. O projeto REST listado na ferramenta de configuração de serviços da Web.

   ![Projeto REST na ferramenta de configuração de serviços Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-discovery.png)

6. A próxima etapa é definir a chamada à API REST e converter a chamada para as chamadas de Windows Communication Foundation (WCF).

   1. Expanda o **projeto REST** e selecione o serviço _RESTSAMPLE_ .

   2. Selecione **Adicionar**. Você será solicitado a adicionar dois valores:
   
      ![Inserir valores para o serviço REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service-highlights.png)
      
      1. Insira o **nome**. Esta etapa é rotulada como 3 na captura de tela.
      2. Insira o **endereço**. Esta etapa é rotulada como 4 na captura de tela.
      3. Selecione **OK**. Um recurso REST é adicionado à descrição para o serviço _RESTSAMPLE_ .

7. Na caixa **recursos** , selecione o recurso REST que você acabou de adicionar. Adicione o seguinte método:

   ![Adicionar um método REST ao recurso](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-method.png)
   <!-- How does this dialog appear, by selecting Edit? -->

8. Selecione o método REST. Observe que é possível criar vários métodos no mesmo recurso e definir as consultas passadas durante a execução.

9. Para o método GETALL, nenhuma consulta é necessária. Deixe os valores de parâmetro em branco. Ao exportar ou importar a API REST, você deve definir a resposta de/ou de solicitação de exemplo, dependendo da função. Copie e cole o retorno JSON ao navegar para este exemplo.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-samples.png)

10. Clique em **Salvar**. Salve o projeto no `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions` . 

>[!NOTE]
>Depois que o projeto é salvo, o arquivo WsConfig é gerado. O arquivo de configuração contém vários arquivos que são definidos anteriormente na visão geral do serviço Web.


## <a name="configure-object-types-in-the-web-service-configuration-tool"></a>Configurar tipos de objeto na ferramenta de configuração do serviço Web
As etapas a seguir mostram como configurar tipos de objeto para sua fonte de dados na ferramenta de configuração do serviço Web.

1. A próxima etapa é definir o esquema de espaço do conector. Isso é feito criando-se o tipo de objeto e definindo seus tipos de objeto. Clique em **tipos de objeto** no painel esquerdo e clique no botão **Adicionar** . Isso é aberto abaixo da tela. Adicione um novo tipo de objeto e forneça um nome. Clique no botão **OK**.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-types.png)

2. A adição de um tipo de objeto fornece a tela abaixo.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-type-employee.png)

3. O painel direito correspondente ao tipo de objeto permite que você mantenha os atributos e suas propriedades para o tipo de objeto selecionado. Clicar no botão Adicionar fornecerá a tela abaixo, em que um pode adicionar atributos.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employeeid-string.png)

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-id-string.png)

4. A tela abaixo aparece depois de adicionar todos os atributos necessários.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-object-type-02.png)

5. Tipos de objeto e atributos depois de criados, fornecem fluxos de trabalho em branco que atendem às operações executadas no MIM (Microsoft Identity Manager).


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

![Designer de Fluxo de Trabalho](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-configuration-workflow.png)

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



## <a name="configure-a-full-import-workflow-in-the-web-service-configuration-tool"></a>Configurar um fluxo de trabalho de importação completa na ferramenta de configuração do serviço Web
As etapas a seguir mostram como configurar fluxos de trabalho de importação completos para a API REST usando a ferramenta de configuração do serviço Web.

>[!WARNING]
>Este exemplo cria apenas um fluxo de trabalho. As modificações no fluxo de trabalho, como para usar a lógica personalizada na API, podem ser necessárias.

1. Selecione o fluxo de trabalho de importação completa a ser configurado. Os **argumentos** e as **importações** já estão definidos e são específicos para as atividades. Consulte as telas a seguir para obter mais informações.

   ![Argumentos de fluxo de trabalho de importação completa](media/microsoft-identity-manager-2016-ma-ws-restgeneric/arguments.png)

   ![Namespaces importados](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imported-name-spaces.png)

   Após a reconfiguração das chamadas, você precisa alterar os nomes dos atributos que alteram ou adicionam o namespace a variáveis que se referem à estrutura de retorno dos tipos de API e de objeto que se referem ao namespace antigo. A caixa de ferramentas no painel direito contém todas as atividades personalizadas específicas do fluxo de trabalho que você precisa para a configuração. Atribua os valores às variáveis que você pretende usar para sua lógica. Vá para a seção inferior do designer de fluxo de trabalho central e declare as variáveis. As variáveis são declaradas na próxima etapa.

2. Adicione uma atividade de sequência. Arraste o designer de atividade de **sequência** da **caixa de ferramentas** e solte-o na superfície de designer de fluxo de trabalho do Windows. Consulte as telas a seguir. A atividade [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) contém uma coleção ordenada de atividades filho que ela executa na ordem.
   
    ![Atividade de sequência](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imports.png)

3. Para adicionar uma variável, localize **criar variável**. Digite _wsResponse_ para o **nome**, selecione a lista suspensa **tipo de variável** e, em seguida, selecione **procurar tipos**. Uma caixa de diálogo é exibida. Selecione   >    >  **resposta** GetAll gerada. Mantenha o **escopo** e os valores **padrão** desmarcados. Como alternativa, defina esses valores usando a exibição **Propriedades** .

   ![Resposta padrão](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list.png)

4. Arraste mais um designer de atividade de **sequência** da **caixa de ferramentas** dentro da atividade de sequência já adicionada.

5. Arraste um **WebServiceCallActivity** apresentado em **comum.** Essa atividade é usada para invocar a operação de serviço Web disponível após a descoberta. Essa é uma atividade personalizada e é comum em cenários de operação diferentes. 

    ![Operação de nome de serviço](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-operation-workflow.png)

   Para usar a operação de serviço Web, defina as seguintes propriedades:
   
      - **Nome do serviço**: Insira um nome para o serviço Web.
      - **Nome do ponto de extremidade**: especifique um nome de ponto de extremidade para o serviço selecionado.
      - **Nome da operação**: Especifique a respectiva operação para o serviço.
      - **Argumento**: selecione **argumentos**. Na próxima caixa de diálogo, atribua os valores de argumento, conforme mostrado na figura a seguir:
      
         ![Atribuir argumentos](media/microsoft-identity-manager-2016-ma-ws-restgeneric/get-all.png)

         >[!IMPORTANT]
         >Não altere o **nome**, a **direção** ou o **tipo** de um argumento usando esta caixa de diálogo. Se qualquer um desses valores for alterado, a atividade se tornará inválida. Defina apenas o **valor** para o argumento. Conforme mostrado nesta figura, o valor *wsResponse* é definido.

6. Adicione uma atividade **foreach** logo abaixo de **WebServiceCallActivity.** Essa atividade é usada para iterar em todos os atributos (âncoras e não-âncoras) do tipo de objeto. Ao arrastar essa atividade para sua superfície de Designer de Fluxo de Trabalho, ela enumera automaticamente todos os nomes de atributo para o objeto. Defina os valores necessários de acordo com a tela a seguir:

   ![Atividade de chamada de serviço Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach.png)

7. Em alguns casos, talvez seja necessário abrir o generated.dll que está dentro do arquivo WsConfig. Copie esse arquivo WsConfig e renomeie-o com a extensão. zip. Abra e extraia o generated.dll usando sua ferramenta de refletor .NET preferida.

   ![Arquivo de configuração](media/microsoft-identity-manager-2016-ma-ws-restgeneric/config-files.png)

8. Identifique o namespace público para a _employeeList_:

    ![Código da lista de funcionários](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list-code.png)

    Em seguida, adicione este retorno ao **foreach** do fluxo de trabalho:

    ![Adicionar lista de funcionários ao fluxo de trabalho ForEach](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach-employee-list.png)

9. Arraste uma atividade **CreateCSEntryChangeScope** no corpo **foreach** . Essa atividade é usada para criar uma instância do objeto CSEntryChange no domínio do fluxo de trabalho para cada registro respectivo ao recuperar dados da fonte de dados de destino. Arrastar essa atividade fornece a tela abaixo. As atividades **ancoraattribute** são herdadas automaticamente. Atualize o valor de **DN** para seu nome de domínio preferencial.

    ![Criar atividade de CS entrada de escopo de alteração](media/microsoft-identity-manager-2016-ma-ws-restgeneric/createcsentry.png)

    >[!NOTE]
    >Valores de âncora e nomes de objeto variam de acordo com o serviço Web exposto. A figura mostra um exemplo.

10. Arraste uma atividade **CreateAttributeChange** abaixo da atividade **ancoraattribute** . O número de atividades a serem arrastadas é igual ao número de atributos que não são de âncora. Consulte a figura a seguir para obter referência.

    ![Criar âncora](media/microsoft-identity-manager-2016-ma-ws-restgeneric/create-anchor-attribute.png)

    >[!NOTE]
    >Para usar essa atividade, escolha e atribua os respectivos campos da lista suspensa e atribua os valores. Para atributos com vários valores, descarte várias atividades **CreateValueChangeActivity** dentro de uma atividade **CreateAttributeChangeActivity** .

11. Salve este projeto no local `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` . Em seguida, configure o agente de gerenciamento conforme descrito em [configuração de ma de serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md).

    ![Salvar o projeto REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/sample-rest.png)
    
    Os projetos padrão devem ser baixados e salvos no local `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` no sistema de destino. Os projetos são então visíveis no assistente do conector de serviço da Web.
    
    Ao executar o arquivo executável, você será solicitado a especificar o local para a instalação. Insira o local para salvar.
    
    >[!IMPORTANT]
    >O arquivo de projeto pode ser salvo e aberto em qualquer local (com os privilégios de acesso apropriados de seu executor). Somente arquivos de projeto que são salvos na `Synchronization Service\Extension` pasta podem ser selecionados no assistente de conector de serviço da Web que é acessado por meio da interface do usuário de sincronização do mim.
    
    O usuário que está executando a ferramenta de configuração do serviço Web requer os seguintes privilégios:
    
       - Controle total para a pasta de extensão do serviço de sincronização.
       - Acesso de leitura à chave do registro `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` por meio da qual o caminho da pasta de extensão está localizado.


## <a name="next-steps"></a>Próximas etapas

- [Visão geral do conector de serviço Web genérico](microsoft-identity-manager-2016-ma-ws.md)
- [Instalar a ferramenta de configuração do serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implantação de SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração de MA do serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
