---
title: "Etapa 6 para implantar o PAM – mover grupo | Microsoft Docs"
description: Migre um grupo para a floresta PRIV para que ele possa ser gerenciado com o Privilege Access Management.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 9e70bdd3c586a16a9a55c91bd0e099a500950a64


---

# <a name="step-6-transition-a-group-to-privileged-access-management"></a>Etapa 6 – Faz a transição de um grupo para Gerenciamento de acesso privilegiado

>[!div class="step-by-step"]
[« Etapa 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Etapa 7 »](step-7-elevate-user-access.md)

A criação de conta privilegiada na floresta PRIV é feita usando cmdlets do PowerShell. Esses cmdlets executam as seguintes funções:

- Crie um novo grupo na floresta PRIV com o mesmo SID (Identificador de Segurança) de um grupo na floresta CORP.  
- Crie um objeto no banco de dados do Serviço MIM correspondente ao grupo na floresta PRIV.  
- Para cada conta de usuário, crie dois objetos no banco de dados do Serviço MIM, correspondendo ao usuário na floresta CORP e à nova conta de usuário na floresta PRIV.  
- Crie um objeto da Função do PAM no banco de dados do Serviço do MIM.  

Os cmdlets precisam ser executados uma vez para cada grupo e uma vez para cada membro de um grupo. Os cmdlets de migração não alteram nem modificam nenhum usuário ou grupo na floresta CORP: o administrador do PAM fará isso manualmente mais tarde.

1. Entre em PAMSRV, diretamente ou por meio de uma estação de trabalho PRIV, como *PRIV\MIMAdmin*.

2.  Inicie o PowerShell e digite os comandos a seguir.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Crie uma conta de usuário em PRIV correspondente a uma conta de usuário em uma floresta existente, para fins de demonstração.

    Digite os comandos a seguir no PowerShell.  Se você não usou o nome *Julia* para criar o usuário em contoso.local anteriormente, altere os parâmetros do comando, conforme apropriado. A senha 'Pass@word1' é apenas um exemplo e deve ser alterada para um valor de senha exclusivo.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Copie um grupo e seus membros, Julia, de CONTOSO para o domínio PRIV, para fins de demonstração.

    Execute os seguintes comandos, especificando a senha do administrador do domínio CORP (CONTOSO\Administrator), quando solicitado:

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Na referência, o comando **New-PAMGroup** usa os seguintes parâmetros:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Opcional) Em CORPDC, remova a conta de Julia do grupo **CONTOSO CorpAdmins**, se ainda estiver presente.  Isso é necessário apenas para fins de demonstração, a fim de ilustrar como as permissões podem ser associadas às contas criadas na floresta PRIV.

    1.  Entre em CORPDC como *CONTOSO\Administrator*.

    2.  Inicie o PowerShell, execute o seguinte comando e confirme a alteração.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Se você desejar demonstrar que os direitos de acesso entre florestas são efetivos para a conta de administrador do usuário, vá para a próxima etapa.

>[!div class="step-by-step"]
[« Etapa 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Etapa 7 »](step-7-elevate-user-access.md)



<!--HONumber=Nov16_HO2-->


