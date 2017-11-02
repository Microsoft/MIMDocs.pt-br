---
title: "Configurar um domínio para o Microsoft Identity Manager 2016 | Microsoft Docs"
description: "Crie um controlador de domínio do Active Directory antes de instalar o MIM 2016"
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/26/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 816e816111b27d1cc7dd4f7da2c5a810e7aa22fd
ms.sourcegitcommit: 9e854a39128a5f81cdbb1379e1fa95ef3a88cdd2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2017
---
# <a name="set-up-a-domain"></a>Configure um nome de domínio

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

O MIM (Microsoft Identity Manager) trabalha com o domínio do AD (Active Directory). Você já deve ter o AD instalado e verificar se tem um controlador de domínio em seu ambiente para um domínio que possa administrar.

Este artigo explica as etapas para preparar seu domínio para trabalhar com o MIM.

## <a name="create-user-accounts-and-groups"></a>Crie contas de usuários e grupos

Todos os componentes da implantação do MIM precisam de suas próprias identidades no domínio. Isso inclui os componentes do MIM, como Serviço e Sincronização, bem como o SharePoint e o SQL.

> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **mimservername**
> - Nome do domínio - **contoso**
> - Senha – **Pass@word1**

1. Entre no controlador de domínio como o Administrator do domínio com a senha (*e. g. Contoso\Administrador*).

2. Crie as seguintes contas de usuário para serviços do MIM. Inicie o PowerShell e digite o script a seguir para atualizar o domínio.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Crie grupos de segurança para todos os grupos.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  Adicione os SPNs (Nomes da Entidade de Serviço) para habilitar a autenticação Kerberos para contas de serviço

    ```CMD
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S FIMService/mimservername.contoso.local Contoso\MIMService    
    ```

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)
