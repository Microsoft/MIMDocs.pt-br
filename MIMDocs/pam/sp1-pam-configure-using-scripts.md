---
title: Configurar o PAM usando scripts
description: "Prepare o domínio CORP com identidades novas ou existentes para serem gerenciadas pelo Privileged Identity Manager usando scripts"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 3aca2fb513280f118e760bdbc2ba471151c41b17


---

# <a name="configure-pam-using-scripts"></a>Configurar o PAM usando scripts

Se você optar por instalar o SQL e o SharePoint em servidores separados, eles deverão ser configurados usando as instruções a seguir. Se os componentes de PAM, SharePoint e SQL estiverem instalados no mesmo computador, as etapas a seguir deverão ser executadas no computador.

As etapas a seguir pressupõem que um Domínio PRIV já esteja configurado. Para obter instruções sobre como configurar um domínio PRIV, exiba o adendo ao final do documento.

etapas:

1. Descompacte o arquivo compactado "PAMDeploymentScripts.zip" na pasta %SYSTEMDRIVE%\PAM em todos os computadores.
2. Em qualquer um dos computadores, abra o arquivo **PAMDeploymentConfig.xml** e atualize os detalhes usando o gráfico abaixo ou orientação dentro do próprio arquivo XML. Se as florestas CORP e PRIV já estiverem configuradas, você precisará atualizar **DNSName** e **NetbiosName.**
3. Na seção Funções, atualize a **conta de serviço**, **detalhes de máquina** e o **local dos binários de instalação** para funções SQL, SharePoint e MIM.
    1. O local de binários do MIM deve apontar para o diretório que contém a pasta "Serviço e Portal". O local de binários do Cliente deve apontar para o diretório que contém "Add-ins and Extensions.msi".

4. Se este for um ambiente PRIVOnly, a marca PRIVOnly deve ser definida como True.
    1. Para ambientes PRIVOnly, atualize o **DNSName** e **NetbiosName** do Domínio PRIV para corresponder ao domínio CORP. Verifique se os sufixos de máquina estão corretos para as máquinas nas quais o MIM, o SharePoint e o SQL serão instalados, pois o arquivo de modelo padrão pressupõem uma configuração de CORP e PRIV.
    2. Clique aqui para obter mais detalhes sobre os ambientes PRIVOnly.

5. Copie o mesmo PAMDeploymentConfig.xml para a pasta %SYSTEMDRIVE%\PAM em todas as máquinas, CORPDC, PRIVDC, PAM Server, SQL Server e os servidores do SharePoint.


## <a name="deployment-worksheet"></a>Planilha de implantação

Antes de continuar, atualize o PAMDeploymentConfig.xml e coloque a cópia atualizada em todos os computadores.

### <a name="setup"></a>Setup

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


>[!div class="step-by-step"]
[Iniciar »](sp1-step1-configuring-priv-domain.md)



<!--HONumber=Nov16_HO2-->


