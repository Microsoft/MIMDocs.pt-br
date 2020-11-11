---
title: Registro dinâmico do serviço MIM | Microsoft Docs
description: Habilitar o registro de log dinâmico do serviço MIM sem a necessidade de reiniciar o serviço de gerenciamento
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 6cf0914b196673bb2e99d6d679fad46833c58b00
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492252"
---
# <a name="mim-2016-sp1-4414360--service-dynamic-logging"></a>Log dinâmico do serviço do MIM 2016 SP1 (4.4.1436.0)

Em 4.4.1436.0 Apresentamos uma nova capacidade de registro de log. Isso permite aos engenheiros de suporte e de administrador ativarem o registro de log sem a necessidade de reiniciar o serviço de gerenciamento.

Depois de instalado, você verá a seguinte nova linha no Microsoft.ResourceManagement.Service.exe.config chamado

*   Linha 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Linha 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Linha 266 ``</system.diagnostics> ``

![Seções realçadas mostrando as novas entradas de registro de log dinâmico](media/mim-service-dynamic-logging/screen01.png)

Os níveis de registro de log dinâmico podem ser encontrados [aqui](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Crítico = o serviço de nível padrão gravará somente os eventos críticos
- Atualizar a linha 8 (dynamicLogging mode="true" loggingLevel="Critical") com o valor de registro de log preferencial

Configuração de registro de log dinâmico localizado na linha 266: Microsoft.ResourceManagement.Service.exe.config

![As seções destacadas mostram linhas com as diversas áreas de registro de log disponíveis](media/mim-service-dynamic-logging/screen02.png)

Por padrão o local de registro de log estará em **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, a conta de serviço do FIM precisará gravar a permissão nesse local para gerar o log dinâmico.

![O local da pasta dos logs](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  No caso de erros inesperados (erros de sintaxe no arquivo de configuração Microsoft.ResourceManagement.Service.exe.config ou outros erros), a mensagem de erro correspondente será gravada no arquivo Microsoft.ResourceManagement.Service.exe_Emergency.log no seguinte caminho %TMP%, %TEMP% ou %USERPROFILE% (primeiro existente).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Para exibir o rastreamento, você pode usar a [ferramenta Visualizador de rastreamento de serviço](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Captura de tela do visualizador de rastreamento de serviço](media/mim-service-dynamic-logging/screen04.png)

## <a name="updates-build-45xx-or-greater"></a>Atualizações: Build 4.5.x.x ou posterior

No build 4.5.x.x, revisamos o recurso de registro em log para especificar o nível de log padrão como **"Aviso"** . O serviço grava mensagens em dois arquivos (os índices "00" e "01" são adicionados antes da extensão). Os arquivos estão localizados no diretório "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service". Quando o arquivo excede o tamanho máximo, o serviço inicia a gravação em outro arquivo. Se outro arquivo existir, ele será substituído. O tamanho máximo de arquivo padrão é de 1 GB. Para alterar o tamanho máximo padrão, é necessário adicionar o parâmetro **"maxOutputFileSizeKB"** com valor de tamanho máximo em KB no ouvinte (confira o exemplo a seguir) e reinicie o Serviço do MIM. Quando o serviço é iniciado, ele acrescenta os logs ao arquivo mais recente (se o limite de espaço for excedido, ele substituirá o arquivo mais antigo). 

> [!NOTE] 
> Como o tamanho do arquivo de verificação de serviço antes da mensagem é gravado, o tamanho do arquivo pode ser maior do que o tamanho máximo do tamanho de uma mensagem. Por padrão, o tamanho dos logs pode ser de aproximadamente 6 GB (três > ouvintes com dois arquivos para o tamanho de um GB).

> [!NOTE] 
> A conta de serviço deve ter permissões para gravar no diretório >"C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Service" >. Caso a conta de serviço não tenha tais direitos, os > arquivos não serão criados.

Exemplo de como definir o tamanho máximo de 200 MB (200 * 1024 KB) para arquivos svclog e 100 MB * (100 * 1024 KB) para arquivos txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
