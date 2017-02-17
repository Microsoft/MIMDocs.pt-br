---
title: "Etapas necessárias para implantar o Microsoft Identity Manager 2016 | Microsoft Docs"
description: "Obtenha a lista completa das etapas envolvidas na implantação do Microsoft Identity Manager 2016, da preparação do ambiente à configuração dos portais."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 804f957d5cf9f5fb09efa65944983b1d5013fff3


---

# <a name="deploy-mim-2016"></a>Implantação do MIM 2016
Os artigos nesta seção fornecem instruções passo a passo para implantar o MIM (Microsoft Identity Manager) 2016 para cenários de autoatendimento de usuário final em um servidor novo que não tenha o FIM ou o MIM implantado anteriormente.

> [!NOTE]
> A topologia de implantação descrita nesta seção destina-se apenas à introdução e ao aprendizado sobre o MIM.  O [guia de planejamento de capacidade](/microsoft-identity-manager/plan-design/capacity-planning-guide) fornece mais informações sobre topologias para implantações de produção.  Recomendamos revisar essa documentação antes de implantar o MIM para a escala de produção ou uso.

O cenário de gerenciamento de acesso privilegiado é implantado de maneira diferente de outros cenários do MIM, uma vez que requer um ambiente de floresta de bastião dedicado.  Se você quiser aprender mais sobre a implantação do MIM para o Privileged Identity Management, consulte [Configure o ambiente do MIM para o Privileged Access Management](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam).

O processo de implantação do MIM 2016 é muito semelhante ao processo de seu antecessor, o FIM 2010 R2. Se você quiser consultar a documentação do FIM, consulte o [Guia de implantação do Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Primeiro: Preparação de um domínio
O MIM funciona com o AD (Active Directory), portanto, siga estas etapas para configurar seu controlador de domínio do AD.
- [Configuração de domínio](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>Próximo: Configuração de um servidor de gerenciamento de identidade
Depois que o domínio está em vigor e configurado, prepare seu servidor de gerenciamento de identidade corporativa. Isso inclui configurar:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcional)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>Por último: Instalação do Microsoft Identity Manager 2016
Depois que você configurar o domínio e o servidor, estará pronto para instalar os componentes do MIM e configurá-los para sincronizar com o AD.
- [Serviço de Sincronização do MIM](install-mim-sync.md)
- [Serviço e Portal do MIM](install-mim-service-portal.md)
- [Sincronizar bancos de dados do Serviço do MIM e do Active Directory](install-mim-sync-ad-service.md)



<!--HONumber=Jan17_HO4-->


