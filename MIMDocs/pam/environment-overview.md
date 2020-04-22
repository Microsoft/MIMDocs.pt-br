---
title: Visão geral do ambiente do PAM | Microsoft Docs
description: Localize o número necessário e a configuração de máquinas virtuais para implantar o Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c758bd924c6f7272a44567c2e9dc919215cdd273
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043997"
---
# <a name="environment-overview"></a>Visão geral do ambiente

O Privileged Access Management trabalha com VMs (máquinas virtuais) com unidades separadas conectadas entre si em uma rede compartilhada. Essas máquinas virtuais podem ser hospedadas pelo Windows 8.1, Windows Server 2012 R2 ou outras plataformas de sistema operacional.

![Servidores PAM: relações e plataformas com suporte – diagrama](media/pam-test-lab-architecture.png)

Será necessário um mínimo de três máquinas virtuais.  Se você ainda não tiver um domínio do AD para o PAM gerenciar, precisará de uma VM adicional para atuar como um controlador de domínio CORP.  Se você quiser configurar o software PRIV para alta disponibilidade, precisará de duas VMs adicionais.

As unidades em que serão armazenadas as imagens de disco de VM precisam de, pelo menos, 120 GB de espaço livre em disco.  Se você planeja implantar para obter alta disponibilidade, verifique se o subsistema do disco atende aos requisitos de armazenamento compartilhado do SQL.  O armazenamento compartilhado pode estar na forma de discos de cluster do Clustering de Failover do Windows Server, discos em uma rede SAN (Rede de Área de Armazenamento) ou em compartilhamentos de arquivos em um servidor SMB.

> [!IMPORTANT]
> O armazenamento deve ser dedicado ao ambiente de bastiões. O compartilhamento de armazenamento com outras cargas de trabalho fora do ambiente de bastiões não é recomendada, pois pode prejudicar a integridade do ambiente de bastiões.

## <a name="next-steps"></a>Próximas etapas

- [Privileged Access Management para Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) é uma visão geral do PAM e como ele funciona.
- [Noções básicas dos componentes do PAM](principles-of-operation.md) é uma visão geral dos vários componentes do PAM.
