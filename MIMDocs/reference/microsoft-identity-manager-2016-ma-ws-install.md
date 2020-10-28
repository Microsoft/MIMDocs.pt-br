---
title: MIM instalar a ferramenta de configuração do serviço Web | Microsoft Docs
description: Este artigo aborda as etapas para instalar a ferramenta de configuração do serviço Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4b9d2463ea30839c2ea4e2a3427d057c925183e8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757239"
---
# <a name="install-the-web-service-configuration-tool"></a>Instalar a ferramenta de configuração do serviço Web

O conector do serviço Web e os projetos padrão estão disponíveis no [centro de download da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

O **MSI do conector de serviço da Web** expõe dois recursos:

- Tempo de execução do conector do serviço Web: instala o conector de núcleo, as dependências do conector e o conector do pacote.
- Ferramenta de configuração do serviço Web: instala a ferramenta de configuração do serviço Web.

![Opções do conector do assistente de instalação](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

A ferramenta de configuração do pode ser instalada sem que o serviço de sincronização esteja instalado. Isso permite a configuração em um computador separado.

## <a name="default-projects"></a>Projetos padrão

Projetos padrão adicionais são fornecidos com o conector de serviços da Web. Eles estão disponíveis como arquivos EXE auto-extraíveis. Você pode baixar o projeto do conector de serviço Web conforme apropriado para seu requisito.

Depois que a instalação for concluída, os diferentes componentes com seus binários serão instalados no local da pasta abaixo no seu sistema.

| Sumário | Location |
|---|---|
| Tempo de execução do conector do serviço Web           | % Arquivos de programas% \\ Microsoft Forefront Identity Management \\ 2010 \\ sincronizar &nbsp; \\ extensões de serviço |
| Projeto do conector de serviço Web           | % Arquivos de programas% \\ Microsoft Forefront Identity Management \\ 2010 \\ sincronizar &nbsp; \\ extensões de serviço |
| Conector empacotado                      | % Arquivos de programas% \\ Microsoft Forefront Identity Management \\ 2010 \\ sincronização &nbsp; serviço \\ UIShell \\ xmls \\ PackagedMAs |
| Ferramenta de configuração do serviço Web          | % Arquivos de programas% \\ Microsoft Forefront Identity Management 2010 serviço de \\ \\ sincronização &nbsp; \\ UIShell \\ configuração de &nbsp; serviço Web &nbsp; <br/>**Observação** : esse é o local de instalação padrão. Você pode alterar esse local durante a instalação. |
| Arquivo de projeto de serviço Web                | O usuário pode selecionar qualquer pasta de destino para a qual extrair esse arquivo, mas o arquivo de projeto extraído (. WsConfig) é visível para a interface do usuário da sincronização do FIM somente quando o arquivo de projeto é extraído para a pasta **extensões** do fim. O arquivo de projeto extraído é visível para a ferramenta de configuração do serviço Web em qualquer local. |


## <a name="additional-permissions"></a>Permissões adicionais

O arquivo de projeto pode ser salvo e aberto de qualquer local (com os privilégios de acesso apropriados de seu executor); no entanto, somente os arquivos de projeto que são salvos na `Synchronization Service\Extension` pasta podem ser selecionados no assistente de conector de serviço da Web acessado por meio da interface do usuário da sincronização do fim.

O usuário que executa a ferramenta de configuração do serviço Web requer os seguintes privilégios:

- Permissões de leitura/gravação para a pasta de extensão do serviço de sincronização.
- Acesso de leitura à chave do registro **HKLM \\ System \\ CurrentControlSet \\ Services \\ FIMSynchronizationService \\ Parameters** .
