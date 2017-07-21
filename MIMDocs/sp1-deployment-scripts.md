---
title: "Scripts de implantação do MIM2016 SP1 PAM"
description: "Essa página faz parte da série de artigos sobre como configurar o Privileged Identity Manager usando scripts. Ela inclui uma lista das pressuposições sobre o ambiente."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Scripts de implantação do MIM2016 SP1 PAM

Neste service pack, introduzimos um conjunto de scripts de implantação para facilitar a implantação do PAM. Esses scripts estão disponíveis no centro de download. Antes de tentar usar os scripts, é importante que você se certifique de que as suposições a seguir se aplicam ao seu ambiente.

Considerações importantes:
1. O sistema operacional de todos os computadores deve ser pelo menos o Windows Server 2012 R2. Se você estiver experimentando o Windows Server 2016 Technical Preview 5, o controlador de domínio PRIV deverá ser instalado com a compilação TP5.
2. O DNS deve estar configurado para que a resolução de nomes entre seus controladores de domínio e servidores de componente seja automática.
3. Os binários de instalação devem estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) executando independentemente o CORPDC, PRIVDC e PAMSERVER.
5. Para a opção de validação, supõe-se que existe uma máquina de cliente dedicada para executar essa etapa.

>[!NOTE]
>Se você tiver algum problema com a execução de script, talvez seja necessário examinar os logs. Todos os logs de script foram salvos em % AppData%\MIMPAMInstall. Compacte a pasta em um arquivo Zip e a inclua, juntamente com detalhes da operação e do erro, em seu caso de suporte.

Pronto para começar a usar os scripts de implantação do PAM? Comece em [Configurar o PAM usando scripts](./pam/sp1-pam-configure-using-scripts.md).
