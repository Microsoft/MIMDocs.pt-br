---
title: Scripts de implantação do PAM do MIM
description: Esta página faz parte da série de artigos sobre como configurar Microsoft Identity Manager usando scripts. Ela inclui uma lista das pressuposições sobre o ambiente.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e7eea2c72df3ca9893acc5f6989afa9c488f384e
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010448"
---
# <a name="mim-pam-deployment-scripts"></a>Scripts de implantação do PAM do MIM

O MIM 2016 Service Pack 1 introduziu um conjunto de scripts de implantação para facilitar a implantação do PAM. Esses scripts estão disponíveis no centro de download. Antes de tentar usar os scripts, é importante verificar se você atende aos requisitos abaixo:

1. O sistema operacional de todos os servidores deve ser no mínimo o Windows Server 2012 R2.
2. O DNS deve estar configurado para habilitar a resolução de nomes entre seus controladores de domínio e servidores de componente.
3. Os binários de instalação devem estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) executando independentemente o CORPDC, PRIVDC e PAMSERVER.
5. Para a opção de validação, você precisa ter uma estação de trabalho dedicada.

>[!NOTE]
>Se você tiver algum problema com a execução de script, talvez seja necessário examinar os logs. Todos os logs de script foram salvos em % AppData%\MIMPAMInstall. Compacte a pasta em um arquivo Zip e a inclua, juntamente com detalhes da operação e do erro, em seu caso de suporte.

Pronto para começar a usar os scripts de implantação do PAM? Comece em [Configurar o PAM usando scripts](./pam/sp1-pam-configure-using-scripts.md).
