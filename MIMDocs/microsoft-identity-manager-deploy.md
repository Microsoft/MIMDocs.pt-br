---
title: Etapas necessárias para implantar o Microsoft Identity Manager 2016 | Microsoft Docs
description: Obtenha a lista completa das etapas envolvidas na implantação do Microsoft Identity Manager 2016, da preparação do ambiente à configuração dos portais.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/17/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: ea9caef07c2496c5d2e040f5d88764938231220b
ms.sourcegitcommit: 80cdfd782cc6e2a4c4698decd54342f0e1460f5f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75756179"
---
# <a name="deploy-microsoft-identity-manager-2016-sp2"></a>Implantar o Microsoft Identity Manager 2016 SP2
Os artigos nesta seção fornecem instruções passo a passo para implantar o MIM (Microsoft Identity Manager) 2016 para cenários de autoatendimento de usuário final em um servidor novo que não tenha o FIM ou o MIM implantado anteriormente.

> [!NOTE]
> A topologia de implantação descrita nesta seção destina-se apenas à introdução e ao aprendizado sobre o MIM.  O [guia de planejamento de capacidade](capacity-planning-guide.md) fornece mais informações sobre topologias para implantações de produção.  Recomendamos revisar essa documentação antes de implantar o MIM para a escala de produção ou uso.

O cenário de gerenciamento de acesso privilegiado é implantado de maneira diferente de outros cenários do MIM, uma vez que requer um ambiente de floresta de bastião dedicado.  Se você quiser aprender mais sobre a implantação do MIM para o Privileged Identity Management, consulte [Configure o ambiente do MIM para o Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

O processo de implantação do MIM é muito semelhante ao processo de seu antecessor, o FIM 2010 R2. Se você quiser consultar a documentação do FIM, consulte o [Guia de implantação do Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Primeiro: preparar um domínio
O MIM funciona com o AD (Active Directory), portanto, siga estas etapas para configurar seu controlador de domínio do AD.
- [Configuração de domínio](preparing-domain.md) ou
- [Configuração de domínio para Contas de Serviço Gerenciado de Grupo](preparing-domain-gmsa.md)


## <a name="next-prepare-an-identity-management-servers"></a>Em seguida: preparar servidores de gerenciamento de identidade
Depois que o domínio está em vigor e configurado, prepare seu servidor de gerenciamento de identidade corporativa.

Saiba mais sobre plataformas com suporte em [Plataformas com suporte para o MIM 2016 ou posterior](microsoft-identity-manager-2016-supported-platforms.md)

 Isso inclui configurar:
- [Windows Server](prepare-server-ws2016.md)
- [SQL Server](prepare-server-sql2016.md)
- [SharePoint Server](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcional)

## <a name="finally-install-microsoft-identity-manager-2016-sp2-components"></a>Por fim: instalar os componentes do Microsoft Identity Manager 2016 SP2
Depois que você configurar o domínio e o servidor, estará pronto para instalar os componentes do MIM e configurá-los para sincronizar com o AD.
- [Serviço de Sincronização do MIM](install-mim-sync.md)
- [Serviço e Portal do MIM](install-mim-service-portal.md)
- [Sincronizar bancos de dados do Serviço do MIM e do Active Directory](install-mim-sync-ad-service.md)
