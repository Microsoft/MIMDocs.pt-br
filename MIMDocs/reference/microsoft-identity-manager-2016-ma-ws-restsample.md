---
title: Exemplo de serviço de aplicativo de API REST do conector de serviço Web | Microsoft Docs
description: Guia ajudando você a implementar um servidor JSON REST de exemplo no Azure
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/28/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: deb743fcbe4bdd155c1b0c4a31e24af0e8da8a4e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757468"
---
# <a name="web-service-connector-rest-api-app-service-sample"></a>Exemplo de serviço de aplicativo de API REST do conector de serviço Web

Este guia de implantação ajudará você a implantar o servidor JSON REST de exemplo no Azure. Você pode usar este exemplo para ajudá-lo com a configuração e a compreensão do conector de serviço Web.

- O exemplo requer Microsoft Visual Studio 2017.
- Os pacotes do caminho do módulo nativo (NMP) no exemplo usam esse [servidor JSON](https://github.com/typicode/JSON-server) no github.
- Baixe o [código de exemplo](https://github.com/fimguy/SAMPLEREST) do GitHub e implante o código de exemplo para Azure app serviço.

## <a name="deploy-the-json-server-sample"></a>Implantar o exemplo de servidor JSON

1. Depois que o código for baixado, abra o arquivo de solução no [Visual Studio 2017](https://www.visualstudio.com/downloads/).

2. Implante a solução selecionando o projeto, clique com o botão direito do mouse e selecione **publicar** :

    ![Publicar a solução](media/microsoft-identity-manager-2016-ma-ws-restsample/publish-project.png)

3. Selecione o serviço de aplicativo a ser usado para a implantação:

    ![Selecione o serviço de aplicativo](media/microsoft-identity-manager-2016-ma-ws-restsample/app-service.png)

4. Selecione um grupo de recursos existente ou crie um novo grupo de recursos:

    ![Selecionar um grupo de recursos](media/microsoft-identity-manager-2016-ma-ws-restsample/resource-group.png)

5. Use os nomes existentes para o serviço de aplicativo e selecione **criar** :

    ![Criar o Serviço de Aplicativo](media/microsoft-identity-manager-2016-ma-ws-restsample/create.png)

    O serviço de aplicativo é criado.

6. Para publicar o serviço de aplicativo, selecione **publicar** :

    ![Publicar o serviço de aplicativo](media/microsoft-identity-manager-2016-ma-ws-restsample/publish.png)

7. Depois que o serviço de aplicativo é publicado, o exemplo da API REST e o site correspondente são iniciados no navegador padrão:

    ![Exemplo de API REST e site](media/microsoft-identity-manager-2016-ma-ws-restsample/sample-rest-api.png)

Agora você pode configurar sua implantação, conforme descrito no [Guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md).


## <a name="modify-the-sample"></a>Modificar o exemplo

Para modificar os dados JSON e o exemplo de API REST, faça as alterações na **db.JSno** arquivo e, em seguida, atualize a implantação:

![Atualizar o db.JSno arquivo](media/microsoft-identity-manager-2016-ma-ws-restsample/db-json.png)


## <a name="next-steps"></a>Próximas etapas

- [Visão geral do conector de serviço Web genérico](microsoft-identity-manager-2016-ma-ws.md)
- [Instalar a ferramenta de configuração do serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implantação de SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração de MA do serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
