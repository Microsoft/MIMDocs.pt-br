---
title: "Scripts de implantação do MIM2016 SP1 PAM"
description: "Essa página faz parte da série de artigos sobre como configurar o Privileged Identity Manager usando scripts. Ela inclui uma lista das pressuposições sobre o ambiente."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 12c60e12dc5662ff0313e21bb9180b3709969af6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# Scripts de implantação do MIM2016 SP1 PAM
<a id="mim2016-sp1-pam-deployment-scripts" class="xliff"></a>

Neste service pack, introduzimos um conjunto de scripts de implantação para facilitar a implantação do PAM. Esses scripts estão disponíveis no centro de download. Antes de tentar usar os scripts, é importante que você se certifique de que as suposições a seguir se aplicam ao seu ambiente.

Considerações importantes:
1. O sistema operacional de todos os computadores deve ser pelo menos o Windows Server 2012 R2. Se você estiver experimentando o Windows Server 2016 Technical Preview 5, o controlador de domínio PRIV deverá ser instalado com a compilação TP5.
2. O DNS deve estar configurado para que a resolução de nomes entre seus controladores de domínio e servidores de componente seja automática.
3. Os binários de instalação devem estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) executando independentemente o CORPDC, PRIVDC e PAMSERVER.
5. Para a opção de validação, supõe-se que existe uma máquina de cliente dedicada para executar essa etapa.

>[!NOTE]
>Se você tiver algum problema com a execução de script, talvez seja necessário examinar os logs. Todos os logs de script foram salvos em % AppData%\MIMPAMInstall. Compacte a pasta em um arquivo Zip e a envie por email para mim2016@microsoft.com, juntamente com detalhes da operação e do erro.

Pronto para começar a usar os scripts de implantação do PAM? Comece em [Configurar o PAM usando scripts](./pam/sp1-pam-configure-using-scripts.md).
