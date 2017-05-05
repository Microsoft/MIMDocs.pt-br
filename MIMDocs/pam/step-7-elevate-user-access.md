---
title: "Etapa 7 para implantar o PAM – acesso do usuário | Microsoft Docs"
description: "Como etapa final, conceda um acesso de usuário privilegiado temporário para demonstrar que a implantação Privileged Access Management foi realizada com êxito."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.lasthandoff: 05/02/2017


---

# <a name="step-7--elevate-a-users-access"></a>Etapa 7 – elevar o acesso do usuário

>[!div class="step-by-step"]
[« Etapa 6 ](step-6-transition-group-to-pam.md)


Esta etapa demonstra que um usuário pode solicitar acesso a uma função pelo MIM.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Verifique se Julia não pode acessar o recurso com privilégios
Sem privilégios elevados, Julia não pode acessar o recurso privilegiado na floresta CORP.

1. Saia de CORPWKSTN para remover todas as conexões abertas em cache.
2. Entre em CORPWKSTN como *CONTOSO\Julia* e mude para a exibição **Área de trabalho**.
3. Abra um prompt de comando do DOS.
4. Digite o comando `dir \\corpwkstn\corpfs`. A mensagem de erro **Acesso negado** deverá ser exibida.
5. Deixe a janela do prompt de comando aberta.

## <a name="request-privileged-access-from-mim"></a>Solicite acesso privilegiado do MIM.
1. Em CORPWKSTN, ainda como CONTOSO\Julia, digite o comando a seguir.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Quando for solicitado, digite a senha da conta PRIV.Julia. Uma nova janela de prompt de comando será exibida.
3. Quando a janela do PowerShell for exibida, digite os seguintes comandos.

    > [!NOTE]
    > Depois de executar esses comandos, todas as etapas a seguir serão sensíveis ao tempo.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Depois de concluir, feche a janela do PowerShell.
5. Na janela de comando do DOS, digite o seguinte comando

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Digite a senha da conta PRIV.Julia. Uma nova janela de prompt de comando será exibida.

## <a name="validate-the-elevated-access"></a>Valide o acesso com privilégios elevados.
Na janela recém-aberta, digite os comandos a seguir.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Se o comando dir falhar com a mensagem de erro **Acesso negado**, verifique novamente a relação de confiança.

## <a name="activate-the-privileged-role"></a>Ativar a função com privilégios
Ative com a solicitação de acesso privilegiado por meio do portal de exemplo do PAM.

1. Em CORPWKSTN, verifique se você está conectado como CORP\Julia.
2. Digite o seguinte comando na janela de comando do DOS.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Quando for solicitado, digite a senha da conta PRIV.Julia. Uma nova janela do navegador da Web será exibida.
4. Navegue até http://pamsrv.priv.contoso.local:8090 e verifique se uma página da Web no portal de exemplo está visível.
5. No Internet Explorer, selecione **Ferramentas** > **Opções da Internet** e clique na guia **Segurança**.
6. Clique na **Zona de intranet local** > **Sites** > **Avançado** e adicione o site à zona.
7. Feche os diálogos **Opções da Internet** .
8. Na guia à esquerda, clique em **Ativar**. Selecione a **função do PAM** e clique em **Ativar**.

> [!Note]
> Neste ambiente, você também pode aprender a desenvolver aplicativos que usam a API REST do PAM, descrita na [Privileged Access Management REST API Reference](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference) (Referência da API REST do Privileged Access Management).

## <a name="summary"></a>Resumo
Depois de concluir as etapas neste passo a passo, você terá demonstrado um cenário do Privileged Access Management, no qual os privilégios do usuário são elevados por uma quantidade limitada de tempo, permitindo ao usuário acessar recursos protegidos com uma conta privilegiada separada. Assim que a sessão de elevação expirar, a conta privilegiada não pode mais acessar o recurso protegido. A decisão de quais grupos de segurança representam funções privilegiadas é coordenada pelo administrador do PAM. Depois que os direitos de acesso são migrados para o sistema do Privileged Access Management, o acesso antes possibilitado com a conta de usuário original é agora possibilitado apenas ao entrar com uma conta privilegiada especial e disponibilizado mediante solicitação. Como resultado, associações de grupo para grupos altamente privilegiados são efetivas por uma quantidade limitada de tempo.

>[!div class="step-by-step"]
[« Etapa 6 ](step-6-transition-group-to-pam.md)

