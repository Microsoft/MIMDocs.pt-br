---
title: "Registro dinâmico do serviço MIM | Microsoft Docs"
description: "Habilitar o registro de log dinâmico do serviço MIM sem a necessidade de reiniciar o serviço de gerenciamento"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 90a0f144b7674bbfaf13138dfd926dbfc3c74f28
ms.openlocfilehash: ddd707210d5cd6b618709a477d40e7771d73cfa1
ms.lasthandoff: 03/27/2017



---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Registro de log dinâmico do serviço do SP1 MIM (4.4.1436.0)
Em 4.4.1436.0 Apresentamos uma nova capacidade de registro de log. Isso permite aos engenheiros de suporte e de administrador ativarem o registro de log sem a necessidade de reiniciar o serviço de gerenciamento.

Depois de instalado, você verá a seguinte nova linha no Microsoft.ResourceManagement.Service.exe.config chamado

*    Linha 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*    Linha 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*    Linha 266 ``</system.diagnostics> ``

![Seções realçadas mostrando as novas entradas de registro de log dinâmico](media/mim-service-dynamic-logging/screen01.png)

Os níveis de registro de log dinâmico podem ser encontrados [aqui](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Crítico = o serviço de nível padrão gravará somente os eventos críticos
- Atualizar a linha 8 (dynamicLogging mode="true" loggingLevel="Critical") com o valor de registro de log preferencial

Configuração de registro de log dinâmico localizado na linha 266: Microsoft.ResourceManagement.Service.exe.config

![As seções destacadas mostram linhas com as diversas áreas de registro de log disponíveis](media/mim-service-dynamic-logging/screen02.png)

Por padrão o local de registro de log estará em **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**, a conta de serviço do FIM precisará gravar a permissão nesse local para gerar o log dinâmico.

![O local da pasta dos logs](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 No caso de erros inesperados (erros de sintaxe no arquivo de configuração Microsoft.ResourceManagement.Service.exe.config ou outros erros), a mensagem de erro correspondente será gravada no arquivo Microsoft.ResourceManagement.Service.exe_Emergency.log no seguinte caminho %TMP%, %TEMP% ou %USERPROFILE% (primeiro existente).  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Para exibir o rastreamento, você pode usar a [ferramenta Visualizador de rastreamento de serviço](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Captura de tela do visualizador de rastreamento de serviço](media/mim-service-dynamic-logging/screen04.png)

