---
title: Requisitos de software do PAM | Microsoft Docs
description: Encontre os requisitos de hardware e software para uma implantação bem-sucedida do Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79f9eeabdd9ac9206c4232217c7cbcdf870c3a3a
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043980"
---
# <a name="hardware-and-software-requirements"></a>Requisitos de hardware e software

O Privileged Access Management não tem nenhum requisito de hardware além daqueles das plataformas de software subjacentes. Apenas certifique-se de ter memória ou espaço em disco suficiente e conectividade de rede.

> [!IMPORTANT]
> Este artigo fornece os requisitos mínimos para uma implantação básica. Seu objetivo não é demonstrar a alta disponibilidade, a escalabilidade ou o desempenho. Ele não representa uma topologia de implantação recomendada para empresas de grandes porte ou ambientes de produção.

## <a name="installing-from-software-packages"></a>Instalando por meio de pacotes de software

O software a seguir pode ser baixado do Centro de Avaliação TechNet ou MSDN:

- Microsoft Identity Manager 2016
  - Serviço e portal: contém o instalador para o serviço do MIM e o portal do MIM e para o cenário do PAM
  - Suplementos e extensões: contém o instalador para os cmdlets do PowerShell do solicitante

O software a seguir pode ser baixado do GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): contém o aplicativo Web de exemplo para a API REST

## <a name="required-software"></a>Software exigido

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 ou SQL Server 2014

## <a name="evaluation-software"></a>Software de avaliação

Se você não tiver licenças do Windows, SQL Server ou Windows Server, baixe versões de avaliação.

### <a name="technet-evaluation-center"></a>Centro de Avaliação TechNet

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Centro de Download da Microsoft

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 e seus pré-requisitos](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Requisitos de hardware

Para cada componente do PAM, consulte os requisitos do sistema referentes aos produtos de software.

Para CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) ou anterior

Para CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Para PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Para PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) ou [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
