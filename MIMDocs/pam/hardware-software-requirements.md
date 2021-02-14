---
title: Requisitos de software do PAM | Microsoft Docs
description: Encontre os requisitos de hardware e software para uma implantação bem-sucedida do Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8d3c266c78df21c6ddb24f62618621c55820bd6f
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395445"
---
# <a name="hardware-and-software-requirements"></a>Requisitos de hardware e software

O Privileged Access Management não tem nenhum requisito de hardware além dos requisitos das plataformas de software subjacentes. Apenas certifique-se de ter memória ou espaço em disco suficiente e conectividade de rede.

> [!IMPORTANT]
> Este artigo fornece os requisitos mínimos para uma implantação básica em uma rede isolada. Não se destina a demonstrar o desempenho, a escalabilidade ou a alta disponibilidade e não representa uma topologia de implantação recomendada para empresas de grande porte ou ambientes de produção.  Se seu Active Directory fizer parte de um ambiente conectado à Internet, consulte em vez disso a orientação de [proteção de acesso privilegiado](/security/compass/overview) para obter mais informações sobre onde começar.

## <a name="installing-from-software-packages"></a>Instalando por meio de pacotes de software

O software a seguir pode ser baixado do Centro de Avaliação TechNet ou MSDN:

- Microsoft Identity Manager 2016
  - Serviço e portal: contém o instalador para o serviço do MIM e o portal do MIM e para o cenário do PAM
  - Suplementos e extensões: contém o instalador para os cmdlets do PowerShell do solicitante

O seguinte software opcional pode ser baixado do GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): contém o aplicativo Web de exemplo para a API REST

## <a name="required-software"></a>Software exigido

- Windows Server 2016
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 ou SQL Server 2014

## <a name="hardware-requirements"></a>Requisitos de hardware

Para cada componente do PAM, consulte os requisitos do sistema referentes aos produtos de software.

Para CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) ou anterior

Para CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Para PRIVDC:

- Windows Server 2016

Para PAMSRV:

- Windows Server 2016
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) ou [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
