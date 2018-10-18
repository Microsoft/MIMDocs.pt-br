---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: O MIM inclui recursos de gerenciamento de acesso do FIM 2010 e ajuda a gerenciar usuários, credenciais, políticas e acesso dentro da organização.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: abbd661fa1bef13ad92b916f8485934390905bf4
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358305"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

O MIM (Microsoft Identity Manager) 2016 foi desenvolvido com base nos recursos de gerenciamento de identidades e acessos do [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Como seu antecessor, o MIM ajuda você a gerenciar usuários, credenciais, políticas e acessos na sua organização.  Além disso, o MIM 2016 adiciona uma experiência híbrida, recursos de gerenciamento com acesso privilegiado e suporte para novas plataformas.

Além da funcionalidade de gerenciamento de identidades existentes incluída no [FIM](https://technet.microsoft.com/library/jj133868). O MIM 2016 fornece novos recursos e melhorias, como:

- Privileged Identity Management
- Nova funcionalidade no gerenciamento de certificado
  - [Referência da API REST do Certificate Manager](./reference/certificate-management-rest-api-reference.md)
  - Suporte para topologias de várias florestas.
  - [Um aplicativo do Windows para cartão inteligente virtual](working-with-mim-certificate-manager.md)
  - Eventos atualizados e recursos de solução de problemas. 
- Os [cenários de autoatendimento](working-with-self-service-password-reset.md) agora incluem o desbloqueio de conta e o portão do MFA (autenticação multifator) do Azure para redefinição de senha.

## <a name="hybrid-experience"></a>Experiência híbrida

O Microsoft Identity Manager 2016 funciona com o [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) para fornecer a você controle sobre todo o ambiente. O relatório híbrido no Azure AD apresenta seus dados locais e da nuvem em um único local. Além disso, o [portal de Redefinição de Senha por Autoatendimento](working-with-self-service-password-reset.md) dá suporte à MFA (Autenticação Multifator) do Azure.

## <a name="privileged-identity-management"></a>Privileged Identity Management

O Privileged Identity Management controla e gerencia o acesso administrativo fornecendo acesso temporário baseado em tarefas a recursos confidenciais. Isso significa que você pode fornecer aos usuários apenas a permissão necessária, o que reduz as chances de um invasor cibernético conseguir acesso administrativo completo. Além disso, o Privileged Identity Management extrai e isola as contas administrativas de florestas do Active Directory existentes.

O MIM dá suporte a uma solução local do Privileged Identity Management para gerenciar Active Directory. Para começar, [Use o Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tópicos relacionados

- O Microsoft Identity Manager ainda está muito relacionado ao seu predecessor, o Forefront Identity Manager. Se você ainda usa o FIM ou deseja consultar a documentação adicional, veja o [Roteiro da documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações de topologia para implantação do MIM](topology-considerations.md). Este artigo apresenta várias topologias de implantação que você pode considerar.
- [Guia de planejamento de capacidade](capacity-planning-guide.md) Use este guia em ambientes de teste para entender o escopo apropriado da sua implantação.
