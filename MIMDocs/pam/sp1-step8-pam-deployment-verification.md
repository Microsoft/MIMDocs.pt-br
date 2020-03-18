---
title: Etapa 8 Verificação de implantação do PAM
description: A implantação do PAM com scripts inclui scripts de verificação que podem executar um cenário de PAM a fim de validar se a implantação do PAM está funcionando conforme o esperado.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 41c1ff575bafb4c892d0657234554387680b75f1
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043742"
---
# <a name="step-8-pam-deployment-verification"></a>Etapa 8 Verificação de implantação do Pam

> [!div class="step-by-step"]
> [«Etapa 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Adendo»](sp1-pam-deployment-addendum.md)

O pacote de implantação vem com scripts de verificação que podem executar um cenário de PAM a fim de validar se a implantação do PAM está funcionando conforme o esperado.
Para usar a Verificação de implantação, modifique a seção PAMDeploymentConfig.xml chamada <PamValidation/>.

>[!NOTE]
>A validação exige um domínio de computador cliente ingressado no Domínio CORP com os componentes do PAM do lado do cliente instalados. Veja o Adendo para obter os scripts sobre como instalar um cliente.

O nome do computador cliente deve ser atualizado na marca <PAMValidationClient/> do PAMDeploymentConfig.xml O restante dos dados no nó <PAMValidation/> deve ser editado somente se estiver em conflito com os usuários/grupos existentes, pois essa validação tentará criá-los.
Use as etapas a seguir para realizar a validação:

Etapa 1:

1. Faça logon em CORPDC como Administrador de Domínio CORP
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Isso criará os grupos e usuários necessários para a validação

Etapa 2:

1. Faça logon no PAM Server e no MIMAdmin
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Esta etapa migra os usuários e grupos para o ambiente do PAM

Etapa 3:

1. Faça logon no cliente CORP como administrador local
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Esta etapa solicitará a credencial do CORPAdmin. Quando fornecido, ele adicionará os usuários necessários aos grupos “Usuários de Área de Trabalho Remota” e “Usuários de Gerenciamento Remoto”.
No cliente CORP, use o seguinte comando para abrir o PowerShell como o usuário PRIV que você está validando. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Na janela do PowerShell, digite:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Isso mostrará o status da solicitação.
  Inicialmente, o usuário não terá acesso ao recurso. Depois que o usuário for adicionado de forma Just-In-Time à função, ele receberá o acesso. Após a expiração da duração da solicitação, o usuário deixará de ter acesso novamente.
  O script usa o padrão (11 minutos) para a expiração da solicitação.

> [!div class="step-by-step"]
> [«Etapa 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Adendo»](sp1-pam-deployment-addendum.md)
