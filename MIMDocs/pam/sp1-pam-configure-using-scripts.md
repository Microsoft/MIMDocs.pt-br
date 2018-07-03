---
title: Configurar o PAM usando scripts
description: Esse artigo faz parte da série que aborda as configurações do PAM com scripts. Ele aborda as modificações do arquivo XML que será usado pelos scripts de implantação do PAM.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/20/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 741d722ce315b7265278997275d05981f44826e8
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289414"
---
# <a name="configure-pam-using-scripts"></a>Configurar o PAM usando scripts

Se optar por instalar o SQL e o SharePoint em servidores separados, você deve configurá-los usando as instruções a seguir. Se os componentes de PAM, SharePoint e SQL estiverem instalados no mesmo computador, as etapas a seguir deverão ser executadas no computador.

As etapas a seguir pressupõem que um Domínio PRIV já esteja configurado. Para obter instruções sobre como configurar um domínio PRIV, exiba o adendo ao final do documento.

etapas:

1. Baixar [Scripts de implantação do PAM](https://www.microsoft.com/download/details.aspx?id=53941)
2. Descompacte o arquivo compactado "PAMDeploymentScripts.zip" na pasta %SYSTEMDRIVE%\PAM em todos os computadores.
3. Em qualquer um dos computadores, abra o arquivo **PAMDeploymentConfig.xml** e atualize os detalhes usando o gráfico abaixo ou orientação dentro do próprio arquivo XML. Se as florestas CORP e PRIV já estiverem configuradas, você precisará atualizar **DNSName** e **NetbiosName.**
4. Na seção Funções, atualize a **conta de serviço**, **detalhes de máquina** e o **local dos binários de instalação** para funções SQL, SharePoint e MIM.
    1. O local de binários do MIM deve apontar para o diretório que contém a pasta "Serviço e Portal". O local de binários do Cliente deve apontar para o diretório que contém "Add-ins and Extensions.msi".

5. Se este for um ambiente PRIVOnly, a marca PRIVOnly deve ser definida como True.
    1. Para ambientes PRIVOnly, atualize o **DNSName** e **NetbiosName** do Domínio PRIV para corresponder ao domínio CORP. Verifique se os sufixos de máquina estão corretos para as máquinas nas quais o MIM, o SharePoint e o SQL serão instalados, pois o arquivo de modelo padrão pressupõem uma configuração de CORP e PRIV.
    2. Clique aqui para obter mais detalhes sobre os ambientes PRIVOnly.

6. Copie o mesmo PAMDeploymentConfig.xml para a pasta %SYSTEMDRIVE%\PAM em todas as máquinas, CORPDC, PRIVDC, PAM Server, SQL Server e os servidores do SharePoint.


## <a name="deployment-worksheet"></a>Planilha de implantação

Antes de continuar, atualize o PAMDeploymentConfig.xml e coloque a cópia atualizada em todos os computadores.

### <a name="setup"></a>Configuração

|Virtual   | Para executar como   |Comandos   |
|---|---|---|
|  PRIVDC |Administrador de Domínio PRIV   | .\PAMDeployment.ps1 Selecione a opção de menu 1 (Configuração de Floresta PRIV)   |
|   |   |  A etapa acima gera um SIDs.txt. Esse arquivo precisa ser copiado em $envDrive: PAM de CORPDC antes de executar a próxima etapa. |
| CORPDC  |Administrador de Domínio CORP   | .\PAMDeployment.ps1 Selecione a opção de menu 2 (Configuração de Floresta CORP)   |
| PAMServer (ou SQL Server)   |Administrador de Domínio CORP   |  .\PAMDeployment.ps1 Selecione a opção de menu 2 (Configuração de Floresta CORP)  |
|  PAMServer |  Administrador Local (Admin MIM após o ingresso no domínio) |  .\PAMDeployment.ps1 Selecione a opção de menu 4 (Configuração do SharePoint)  |
| PAMServer  | Administrador Local (Admin MIM após o ingresso no domínio)  | .\PAMDeployment.ps1 Selecione a opção de menu 5 (Configuração do PAM do MIM)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Selecione a opção de menu 6 (Configuração de confiança do PAM) .\PAMDeployment.ps1 Selecione a opção de menu 6 (Configuração de confiança do PAM) |

### <a name="validation"></a>Validação

|  Virtual | Para executar como   | Comandos   |
|---|---|---|
| CORPClient  | Usuário CORP (administrador local)  |   .\PAMDeployment.ps1 Selecione a opção de menu 7 (Configuração do Cliente do PAM do MIM)  |
| CORPDC  | Administrador de Domínio CORP   | Import-module .\PAMValidation.psm1; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1; Move-PAMValidationUsersToPAM  |
| CORPClient  | Usuário CORP (administrador local)   |   Import-module .\PAMValidation.psm1; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>Usuário \PRIV.pamRequestor e, no caso de PRIVOnly: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Iniciar »](sp1-step1-configuring-priv-domain.md)
