---
title: Converter serviços específicos do Microsoft Identity Manager em gMSA | Microsoft Docs
description: Este artigo apresenta os pré-requisitos e as etapas básicas para configurar uma gMSA (Conta de serviço gerenciado de grupo).
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 50e5da9c7e3ed7df8edb8dbc315708df5ac5250a
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492530"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Converter serviços específicos do Microsoft Identity Manager para usar Contas de serviço gerenciado de grupo

Este artigo é um guia para configurar os serviços de Microsoft Identity Manager com suporte para usar gMSA (Contas de serviço gerenciado de grupo). Depois de pré-configurar seu ambiente, o processo de conversão para gMSA é fácil.

## <a name="prerequisites"></a>Pré-requisitos

- Baixe e instale o seguinte hotfix necessário: [Microsoft Identity Manager 4.5.26.0 ou posterior](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Serviços com suporte:

    -   Serviço de Sincronização do Microsoft Identity Manager (FIMSynchronizationService)
    -   Serviço do Microsoft Identity Manager (FIMService)
    -   Registro de Senha do Microsoft Identity Manager
    -   Redefinição de Senha do Microsoft Identity Manager
    -   Serviço de Monitoramento do PAM (Privileged Access Management) (PamMonitoringService)
    -   Serviço de componente do PAM (PrivilegeManagementComponentService)

    Serviços sem suporte:

    -   Não há suporte para o portal do Microsoft Identity Manager. Ele faz parte do ambiente do SharePoint, e você precisará implantá-lo no modo de farm e [Configurar a alteração automática de senha no SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Todos os agentes de gerenciamento, exceto o agente de gerenciamento de serviço do Microsoft Identity Manager
    -   Gerenciamento de Certificados da Microsoft
    -   BHOLD

- Para obter informações em segundo plano e de referência geral sobre como configurar seu ambiente, confira: 

    -   [Visão geral das Contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Antes de começar, é necessário [criar a chave raiz dos Serviços de Distribuição de Chave](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) no controlador de domínio do Windows. Lembre-se das seguintes informações:  

    - As chaves raiz são usadas pelo serviço KDS (Serviços de Distribuição de Chave) para gerar senhas e outras informações em controladores de domínio.
    - Crie somente uma chave raiz por domínio, se necessário.  
    - Inclua `Add-KDSRootKey –EffectiveImmediately`. "– EffectiveImmediately" significa que pode levar até 10 horas para replicar a chave raiz em todos os controladores de domínio. Pode levar cerca de 1 hora para replicar para os dois controladores de domínio. 
    ![A cadeia de caracteres "– EffectiveImmediately"](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Ações a executar em cada controlador de Domínio do Active Directory

1.  Crie um grupo chamado *MIMSync_Servers* e adicione todos os servidores de sincronização a esse grupo.

    ![Criar um grupo de MIMSync_Servers](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Entre no Windows PowerShell como um *Administrador de Domínio* com uma conta que já ingressou no domínio e execute o seguinte comando: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![O comando no PowerShell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Exiba detalhes da gMSA para sincronização:  
     ![Detalhes da gMSA para sincronização](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Se você estiver executando o PCNS (Serviço de Notificação de Alteração de Senha), atualize a delegação executando o seguinte comando:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Ações a executar no servidor de Sincronização do Microsoft Identity Manager

1. No Synchronization Service Manager, faça o backup da chave de criptografia. Ela será solicitada com a instalação do modo de alteração. Faça o seguinte:

    a. No servidor em que o Synchronization Service Manager está instalado, procure a ferramenta de Gerenciamento da Chave do Serviço de Sincronização. O **conjunto de chaves de exportação** já está selecionado por padrão.

    b. Selecione **Avançar**. 
    
    c. No prompt, insira e verifique as informações de conta do serviço de Sincronização do Microsoft Identity Manager ou do FIM (Forefront Identity Manager):

    -   **Nome da Conta** : o nome da conta do Serviço de Sincronização usada durante a instalação inicial.  
    -   **Senha** : a senha da conta de Serviço de Sincronização.  
    -   **Domínio** : o domínio do qual a conta do Serviço de Sincronização faz parte.

    d. Selecione **Avançar**.

    Se você inserir as informações da conta com êxito, verá uma opção para alterar o destino ou o local do arquivo de exportação da chave de criptografia de backup. Por padrão, o local do arquivo de exportação é *C:\Windows\system32\miiskeys-1.bin*.

1. Instale o Microsoft Identity Manager 2016 SP1 ou o hotfix posterior, que pode ser encontrado no centro de serviços de licenciamento por volume ou no site de downloads do MSDN. Depois de concluir a instalação, salve o conjunto de chaves *miiskeys.bin*.

   ![A janela do progresso da instalação do Serviço de Sincronização do Microsoft Identity Manager](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Instale o [hotfix 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) ou posterior.

1. Depois de instalar o patch, interrompa o serviço de sincronização do FIM fazendo o seguinte:

   a. No Painel de Controle, selecione **Programas e Recursos** > **Microsoft Identity Manager**.  
   b. Na página **Serviço de Sincronização** , selecione **Alterar** > **Avançar**.  
   c. Na janela **Opções de Manutenção** , selecione **Configurar**.

   ![A janela Opções de Manutenção](media/dc98c011bec13a33b229a0e792b78404.png)

   d. Na janela **Configurar o Serviço de Sincronização do Microsoft Identity Manager** , desmarque o valor padrão na caixa **Conta de serviço** e digite **MIMSyncGMSA$** . Certifique-se de incluir o símbolo de cifrão ($), como mostrado na imagem a seguir. Deixe a caixa **Senha** em branco.

   ![A janela Configurar o Serviço de Sincronização do Microsoft Identity Manager](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Selecione **Avançar** > **Avançar** > **Instalar**.  
   f. Restaure o conjunto de chaves do arquivo *miiskeys.bin* que você salvou anteriormente.

   ![Opção para restaurar a configuração do conjunto de chaves](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![A lista de Agentes de Gerenciamento no Synchronization Service Manager](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Serviço do Microsoft Identity Manager

>[!IMPORTANT]
>Siga cuidadosamente as instruções desta seção ao converter as contas relacionadas ao serviço do Microsoft Identity Manager em contas gMSA.

1. Crie Contas gerenciadas de grupo para o Serviço do Microsoft Identity Manager, a API Rest do PAM, o Serviço de Monitoramento do PAM, o Serviço de Componente do PAM, o Portal de Registro do SSPR (redefinição de senha self-service) e o Portal de Redefinição do SSPR.

    -   Atualize a delegação de gMSA e o SPN (nome da entidade de serviço):

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Delegação:

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Delegação restrita:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Adicione uma conta do Serviço do Microsoft Identity Manager nos Grupos de Sincronização. Essa etapa é necessária para a SSPR.

    ![Janela Usuários e Computadores do Active Directory](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > Um problema conhecido no Windows Server 2012 R2 é que os serviços que usam uma conta gerenciada param de responder depois que o servidor é reiniciado porque o Serviço de Distribuição de Chave da Microsoft não é iniciado após a reinicialização do Windows. A solução alternativa para esse problema é executar o seguinte comando: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > O comando inicia o Serviço de Distribuição de Chave da Microsoft quando a rede está ativada (normalmente no começo do ciclo de inicialização).
    >
    > Confira uma discussão sobre um problema semelhante em [AD FS Windows 2012 R2: o adfssrv trava no modo de início](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS).

1.  Execute o MSI Elevado do Serviço do Microsoft Identity Manager e selecione **Alterar**.

1.  Na janela **Configurar conexão do servidor de email** , marque a caixa de seleção **Usar usuário diferente para o Exchange (para contas gerenciadas)** . Você tem a opção de usar a conta atual do Exchange ou a caixa de correio da nuvem.

    >[!NOTE]
    >Se você selecionar a opção **Usar o Exchange Online** , para habilitar o Serviço do Microsoft Identity Manager para processar respostas de aprovação do complemento do Outlook do Microsoft Identity Manager, defina o valor da chave do Registro **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** de *PollExchangeEnabled* como **1** após a instalação.
    
    ![Janela "Configurar a conexão do servidor de email"](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  Na janela **Configurar a conta de serviço do MIM** , na caixa **Nome da Conta de Serviço** , digite o nome. Certifique-se de incluir o símbolo de cifrão ($). Insira também uma senha na caixa **Senha da Conta de Email do Serviço**. A caixa **Senha da Conta de Serviço** deve estar indisponível.

    ![Janela "Configurar a conta de serviço do MIM"](media/db0d543df6e1b0174a47135617c23fcb.png)

    Como a função LogonUser não funciona em contas gerenciadas, a próxima página exibe o aviso "Verifique se a Conta de Serviço está segura em sua configuração atual".

    ![Janela Aviso de Segurança da Conta](media/d350bc13751b2d0a884620db072ed019.png)

1.  Na janela **Configurar API REST do Privileged Access Management** , na caixa **Nome da Conta do Pool de Aplicativos** , digite o nome da conta. Certifique-se de incluir o símbolo de cifrão ($). Deixe a caixa **Senha da Conta do Pool de Aplicativos** em branco.

    ![A janela Configurar API REST do Privileged Access Management](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  Na janela **Configurar o Serviço de Componente do PAM** , na caixa **Nome da Conta de Serviço** , digite o nome da conta. Certifique-se de incluir o símbolo de cifrão ($). Deixe a caixa **Senha da Conta de Serviço** em branco.

    ![A janela Configurar o Serviço de Componente do PAM](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![A janela Aviso de Segurança da Conta](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  Na janela **Configurar o Serviço de Monitoramento do Privileged Access Management** , na caixa **Nome da Conta de Serviço** , digite o nome da conta de serviço. Certifique-se de incluir o símbolo de cifrão ($). Deixe a caixa **Senha da Conta de Serviço** em branco.

    ![A janela Configurar o Serviço de Monitoramento do Privileged Access Management](media/d1e824248edf12a77fc9ffb011475164.png)

1.  Na janela **Configurar o Portal de Registro de Senha do MIM** , na caixa **Nome da Conta** , digite o nome da conta. Certifique-se de incluir o símbolo de cifrão ($). Deixe a caixa **Senha** em branco.

    ![A janela Configurar o Portal de Registro de Senha do MIM](media/601e935cdfda298b61ae753a2a152996.png)

1.  Na janela **Configurar o Portal de Redefinição de Senha do MIM** , na caixa **Nome da Conta** , digite o nome da conta. Certifique-se de incluir o símbolo de cifrão ($). Deixe a caixa **Senha** em branco.

    ![A janela Configurar o Portal de Redefinição de Senha do MIM](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Conclua a instalação.

    > [!NOTE]
    > Durante a instalação, duas novas chaves são criadas no caminho do registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Service** para armazenar a senha criptografada do Exchange. Uma entrada é para o *ExchangeOnline* e a outra é para o *ExchangeOnPremise*. Para uma das entradas, o valor na coluna **Dados** deve estar vazio.

    > ![O Editor do Registro](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Para atualizar a senha em suas contas armazenadas sem precisar executar o modo de alteração, [baixe este script do PowerShell](microsoft-identity-manager-2016-gmsascript.md).

Para criptografar a senha do Exchange, o instalador cria um serviço adicional e o executa na conta gerenciada. As seguintes mensagens são adicionadas ao log de eventos do **Aplicativo** durante a instalação:

![A janela Visualizador de Eventos](media/95b315454705cd4d939b55ac5ad910f5.jpg)
