---
title: Atualização do FIM 2010 R2 e do MIM 2016 SP2 para Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: Saiba como atualizar os componentes do FIM 2010 R2 ou MIM 2016 SP2 e, em seguida, instale os componentes que são novos no MIM 2016.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bdf34be4841b1a911fdb61673e5a3855e66e7320
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "73568051"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>Atualização do MIM 2016 SP2 para Forefront Identity ou Microsoft Identity Manager

As organizações podem atualizar para o Microsoft Identity Manager 2016 SP2 de versões anteriores do Microsoft Identity Manager ou do Forefront Identity Manager.  Cada seção deste artigo aborda um caminho de atualização com suporte.

Há várias opções de atualização disponíveis. Se você já estiver executando o MIM 2016 e não precisar atualizar a plataforma subjacente (Windows Server, SQL, SharePoint, SCSM DW) ou executar os serviços do MIM usando contas de serviço gerenciado de grupo e não usar os Pacotes de Idioma do MIM, a opção mais fácil será a atualização in-loco/instalação do hotfix (.msp). Caso contrário, recomendamos a instalação completa.

> [!IMPORTANT]
> Marcar a seção atualizada [Pré-requisitos de software](prepare-server-ws2016.md#software-prerequisites) antes de instalar o MIM 2016 SP2

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Atualização do FIM 2010 R2 SP1 ou compilações posteriores do FIM

> [!NOTE]
> A versão mínima com suporte do Forefront Identity Manager que pode ser atualizada diretamente para o MIM 2016 SP2 é o FIM 2010 R2 SP1 (build 4.1.3419.0). Não há suporte para a atualização direta para o MIM 2016 SP2 a partir de versões anteriores do FIM. Se você estiver executando builds do FIM anteriores a 4.1.3419.0, precisará atualizar para o FIM 2010 R2 SP1 antes de atualizar para o MIM 2016 SP2.

1. **Opção 1: instalação completa usando os bancos de dados existentes**
    1. Faça uma cópia de backup dos bancos de dados FIMSynchronizationService e FIMService.
    1. Exporte todos os objetos RCDC do serviço FIM e as cadeias de caracteres de recurso RCDC nas quais fez alterações.
    1. Exporte as chaves de criptografia do Serviço de Sincronização.
    1. Faça backup do miisserver.exe.config, da pasta "Extensions" do Servidor de Sincronização, e do Microsoft.ResourceManagement.Service.exe.config, pois o instalador do MIM pode substituir as alterações personalizadas feitas nos arquivos de configuração.
    1. Desinstale **todos** os componentes FIM, incluindo o Pacotes de idiomas (para desinstalar primeiro).
    1. *Opcional:* Mova os bancos de dados do FIM para outro SQL Server. É recomendável criar aliases do SQL em servidores MIM e usar esses aliases em vez do nome real do SQL Server para facilitar a migração dos bancos de dados MIM no futuro.
    1. Instale o MIM 2016 SP2 da mídia de instalação .iso no mesmo ou em outro servidor seguindo o [guia de implantação do MIM](microsoft-identity-manager-deploy.md). Quando solicitado, escolha usar os bancos de dados existentes, forneça as chaves de criptografia do Serviço de Sincronização exportadas anteriormente.
    1. Examine os arquivos miisserver.exe.config e Microsoft.ResourceManagement. Service.exe.config para encontrar os redirecionamentos .net ausentes ou as seções personalizadas que você adicionou.
    1. Instale os Pacotes de idiomas do MIM 2016 SP2, se necessário.
    1. Envie novamente as personalizações para localizações, objetos RCDC e cadeias de caracteres de recurso RCDC, se necessário.
    1. Atualize os Complementos do FIM e clientes de Redefinição de Senha, forneça o novo nome do servidor do Serviço do MIM se o nome do servidor do Serviço do MIM for alterado.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Atualização de builds anteriores do MIM 2016
1. Faça uma cópia de backup dos bancos de dados FIMSynchronizationService e FIMService.
1. Exporte todas as localizações de Serviço do FIM personalizados feitas para objetos RCDC e cadeias de caracteres de recurso RCDC.
1. Exporte as chaves de criptografia do Serviço de Sincronização.
1. Faça backup do miisserver.exe.config, da pasta "Extensions" do Servidor de Sincronização, e do Microsoft.ResourceManagement.Service.exe.config, pois o instalador do MIM pode substituir as alterações personalizadas feitas nos arquivos de configuração.
1. Desinstale os Pacotes de idiomas do MIM se forem usados.
1. Interrompa os serviços do MIM.
1. **Opção 1: Atualização in-loco – instalação do hotfix**
    1. Aplicar o [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) do serviço de sincronização do MIM 2016 SP2
    1. Aplicar o [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) do serviço do MIM 2016 SP2
    1. Examine os arquivos miisserver.exe.config e Microsoft.ResourceManagement. Service.exe.config para encontrar os redirecionamentos .net ausentes ou as seções personalizadas que devem ser adicionadas.
    1. Instale os Pacotes de idiomas do MIM 2016 SP2, se necessário.
    1. Envie novamente as personalizações para localizações, objetos RCDC e cadeias de caracteres de recurso RCDC, se necessário.
    1. Atualize os Complementos do MIM 2016 e os clientes de Redefinição de Senha.
1. **Opção 2: instalação completa usando os bancos de dados existentes**
    1. Desinstale **todos** os componentes do MIM.
    1. *Opcional:* Mova os bancos de dados do FIM para outro SQL Server. É recomendável criar aliases do SQL em servidores MIM e usar esses aliases em vez do nome real do SQL Server para facilitar a migração dos bancos de dados MIM no futuro.
    1. Instale o MIM 2016 SP2 da mídia de instalação .iso no mesmo ou em outro servidor seguindo o [guia de implantação do MIM](microsoft-identity-manager-deploy.md). Quando solicitado, escolha usar os bancos de dados existentes, forneça as chaves de criptografia do Serviço de Sincronização exportadas anteriormente.
    1. Examine os arquivos miisserver.exe.config e Microsoft.ResourceManagement. Service.exe.config para encontrar os redirecionamentos .net ausentes ou as seções personalizadas que devem ser adicionadas.
    1. Instale os Pacotes de idiomas do MIM 2016 SP2, se necessário.
    1. Envie novamente as personalizações para localizações, objetos RCDC e cadeias de caracteres de recurso RCDC, se necessário.
    1. Atualize os Complementos do MIM 2016 e clientes de Redefinição de Senha, forneça o novo nome do servidor do Serviço do MIM se o nome do servidor do Serviço do MIM for alterado.

> [!NOTE]
> As atualizações dos Pacotes de idiomas após o MIM 2016 SP2 serão distribuídas como hotfixes (arquivos .msp), eliminando a necessidade de desinstalar/reinstalar os Pacotes de idiomas.

Informações mais detalhadas sobre os procedimentos de backup e atualização do bancos de dados podem ser encontradas no artigo [atualizar para o FIM 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29), aplicável a qualquer processo de atualização do FIM ou MIM.
