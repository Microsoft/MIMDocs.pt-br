---
title: Visão geral do ambiente do PAM | Microsoft Docs
description: Localize o número necessário e a configuração de máquinas virtuais para implantar o Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ca306e7521bbdc2200663889becd91b1ae952d5a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010652"
---
# <a name="mim-pam-test-lab-environment-overview"></a>Visão geral do ambiente de laboratório de teste do PAM do MIM

Para configurar um laboratório de teste do PAM do MIM, você pode instalar o software em máquinas virtuais.
O Privileged Access Management trabalha com VMs (máquinas virtuais) com unidades separadas conectadas entre si em uma rede compartilhada. Essas máquinas virtuais podem ser hospedadas pelo Windows 8.1, Windows Server 2012 R2 ou outras plataformas de sistema operacional.

![Servidores PAM: relações e plataformas com suporte – diagrama](media/pam-test-lab-architecture.png)

Será necessário um mínimo de três máquinas virtuais.  Se você ainda não tiver um domínio do AD para o PAM gerenciar, precisará de uma VM adicional para atuar como um controlador de domínio CORP.  Se você quiser configurar o software PRIV para alta disponibilidade, precisará de duas VMs adicionais.

As unidades em que serão armazenadas as imagens de disco de VM precisam de, pelo menos, 120 GB de espaço livre em disco.  Se você planeja implantar para obter alta disponibilidade, verifique se o subsistema do disco atende aos requisitos de armazenamento compartilhado do SQL.  O armazenamento compartilhado pode estar na forma de discos de cluster do Clustering de Failover do Windows Server, discos em uma rede SAN (Rede de Área de Armazenamento) ou em compartilhamentos de arquivos em um servidor SMB.

> [!IMPORTANT]
> O armazenamento deve ser dedicado ao ambiente de bastiões. O compartilhamento de armazenamento com outras cargas de trabalho fora do ambiente de bastiões não é recomendada, pois pode prejudicar a integridade do ambiente de bastiões.

## <a name="next-steps"></a>Próximas etapas

- [Privileged Access Management para Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) é uma visão geral do PAM e como ele funciona.
- [Noções básicas dos componentes do PAM](principles-of-operation.md) é uma visão geral dos vários componentes do PAM.
