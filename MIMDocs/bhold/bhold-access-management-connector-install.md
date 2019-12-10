---
title: Instalação do módulo Conector de Gerenciamento de Acesso do BHOLD | Microsoft Docs
description: O módulo conector do BHOLD dá suporte à sincronização inicial e contínua de dados
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516055"
---
# <a name="access-management-connector-installation"></a>Instalação do Conector de Gerenciamento de Acesso

O módulo do Conector de Gerenciamento de Acesso do BHOLD Suite oferece suporte para sincronização inicial e contínua de dados no BHOLD. O Conector de Gerenciamento de Acesso funciona com o Serviço de Sincronização do MIM (Microsoft Identity Manager) para mover dados entre o banco de dados do BHOLD Core, o metaverso do FIM 2010 e repositórios de identidade e aplicativos de destino. Depois de instalar o módulo Conector de Gerenciamento de Acesso, você poderá criar agentes de gerenciamento do FIM que controlam o fluxo de dados entre o BHOLD e o MIM.

## <a name="access-management-connector-software-requirements"></a>Requisitos de software do Conector de Gerenciamento de Acesso

Antes de instalar o módulo Conector de Gerenciamento de Acesso, você deverá instalar o Microsoft .NET Framework 4. Para obter mais informações sobre o .NET Framework 4 e instruções de instalação, consulte a [home page do Microsoft .NET](http://www.microsoft.com/net).
Você deve instalar o Conector de Gerenciamento de Acesso em um computador executando o serviço de sincronização FIM do MIM.

## <a name="access-management-connector-setup"></a>Instalação do Conector de Gerenciamento de Acesso

Para instalar o módulo Conector de Gerenciamento de Acesso, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo Integração de FIM com o BHOLD:

- AccessManagementConnector.msi

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximas etapas

- [Instalação de Integração de FIM com o BHOLD](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) para habilitar o autoatendimento de funções do usuário final, você pode instalar o módulo Integração de FIM com o BHOLD
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)
