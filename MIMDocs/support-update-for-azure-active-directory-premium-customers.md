---
title: Atualização de suporte para clientes do Azure AD Premium usando o Microsoft Identity Manager | Microsoft Docs
description: Este artigo descreve como os clientes do Azure AD Premium podem obter suporte após 21 de janeiro de 2021.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: dae61d97c9db5402d35e2757a1ce844d16236032
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035609"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Atualização de suporte para clientes do Azure AD Premium usando o Microsoft Identity Manager

Aplica-se a: Azure AD Premium, MIM (Microsoft Identity Manager)

Para clientes do Azure AD Premium, o suporte padrão está disponível de junho de 2020 em diante, continuando após 2021 de Janeiro, para componentes específicos do [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) ou service packs posteriores, que habilitam a integração do Azure AD. Isso é adicional ao suporte existente para o Microsoft Identity Manager já fornecido por meio da [política de ciclo de vida fixo](https://docs.microsoft.com//lifecycle/policies/fixed) e planos de [suporte para as empresas](https://support.microsoft.com/help/4341255).

Os componentes do MIM para os quais o suporte padrão está disponível incluem:
- Serviço de sincronização do MIM e PCNS (serviço de notificação de alteração de senha)
- Serviço e portal do MIM, suplementos e extensões, scripts de suporte de data warehouse e pacotes de idiomas
- Conectores do MIM

Esses componentes do MIM populam o Active Directory e, por extensão, o Azure AD por meio de Azure AD Connect, com os usuários e grupos provisionados de um sistema de RH local ou outro sistema de fontes de registro. Isso garante que os clientes que usam o Azure AD Premium com sistemas locais possam continuar a ter suporte durante a migração de seus cenários de gerenciamento de identidades de sistemas locais para o Azure AD. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Como abrir uma solicitação de suporte no portal do Azure

Como uma opção de suporte adicional para o Microsoft Identity Manager, os clientes do Azure AD Premium podem solicitar suporte para os componentes do Microsoft Identity Manager 2016 Service Pack 2 mencionados acima, um hotfix ou uma atualização posterior por meio do portal do Azure.

Um cliente pode criar uma solicitação de suporte do Azure usando as instruções em [Como criar uma solicitação de suporte do Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request):
1. selecione *Tipo de problema: Técnico*
1. alterne para mostrar *Todos os Serviços*
1. na lista de serviços em Azure Active Directory, selecione *Provisionamento e sincronização de usuários*
1. selecione *Tipo de problema: MIM (Microsoft Identity Manager)*
1. selecione *Subtipo de problema*: *Conectores*, *Serviço e Portal* ou *Mecanismo de sincronização*

![Criar uma solicitação de suporte do MIM](media/azure-active-directory-new-support-request.png)

Os componentes do MIM são listados como tipos de problema em *Provisionamento e sincronização de usuários do Azure Active Directory* no portal do Azure.

Para solicitações abertas por meio do portal do Azure, o suporte padrão está disponível para clientes do Azure AD Premium para os seguintes componentes do Microsoft Identity Manager 2016 Service Pack 2, ou um [hotfix ou uma atualização posterior](reference/version-history.md): serviço de sincronização, PCNS (serviço de notificação de alteração de senha), conectores, serviço e portal, suplementos e extensões, scripts de suporte de data warehouse e pacotes de idiomas.

## <a name="other-support-options"></a>Outras opções de suporte

O MIM 2016 SP2, build 4.6.34.0, foi lançado em outubro de 2019. Os clientes são altamente incentivados a permanecer em um service pack com suporte total para garantir que eles estejam na versão mais recente e mais segura de seus produtos. Para obter mais informações, confira a [política de ciclo de vida de service pack](https://support.microsoft.com/help/17138).

Para clientes que ainda usam um build mais antigo do MIM, clientes que não têm suporte nem assinatura do Azure para um pacote que inclui o Azure Active Directory Premium, ou para problemas com outros componentes do MIM não listados acima, o suporte continua disponível. A política de suporte é descrita em [Política de ciclo de vida fixa](https://docs.microsoft.com/lifecycle/policies/fixed) com as datas específicas em [ciclo de vida de suporte para o Microsoft Identity Manager 2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

Além do suporte do Azure, há várias outras opções de suporte que as organizações podem usar para obter suporte. Por exemplo, se você tiver o suporte profissional da Microsoft, poderá [criar uma solicitação de suporte](https://support.microsoft.com/supportforbusiness/productselection). Para selecionar o componente relevante do MIM:
1. selecione a família de produtos *Segurança*
1. selecione o produto *Identity Manager 2016*
