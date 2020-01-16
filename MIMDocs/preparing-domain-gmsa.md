---
title: Configurar um gMSAs para o Microsoft Identity Manager 2016 | Microsoft Docs
description: Configurar Contas de Serviço Gerenciado de Grupo em um domínio para o Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: a74f4074d9a0cf8378fd4972b7f51f723bd2f1c6
ms.sourcegitcommit: 80cdfd782cc6e2a4c4698decd54342f0e1460f5f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75756251"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Configurar um domínio para o cenário de gMSA (Contas de Serviço Gerenciado de grupo)

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)

> [!IMPORTANT]
> Este artigo se aplica somente ao MIM 2016 SP2.

O MIM (Microsoft Identity Manager) trabalha com o domínio do AD (Active Directory). Você já deve ter o AD instalado e verificar se tem um controlador de domínio em seu ambiente para um domínio que possa administrar.  Este artigo descreve como configurar Contas de Serviço Gerenciado de Grupo naquele domínio para uso pelo MIM.

## <a name="overview"></a>Visão geral

As Contas de Serviço Gerenciado de Grupo eliminam a necessidade de alterar periodicamente as senhas das contas de serviço. Com o lançamento do MIM 2016 SP2, os seguintes componentes do MIM podem ter contas gMSA configuradas para serem usadas durante o processo de instalação:

-   Serviço de Sincronização do MIM (FIMSynchronizationService)
-   Serviço do MIM (FIMService)
-   Pool de aplicativos do site da Web para Registro de Senha do MIM
-   Pool de aplicativos do site da Web para Redefinição de Senha do MIM
-   Pool de aplicativos do site da Web para API REST do PAM
-   Serviço de Monitoramento do PAM (PamMonitoringService)
-   Serviço de componente do PAM (PrivilegeManagementComponentService)

Os seguintes componentes do MIM não oferecem suporte à execução como contas gMSA:

-   Portal do MIM. Isso ocorre porque o Portal do MIM faz parte do ambiente do SharePoint. Em vez disso, você pode implantar o SharePoint no modo farm e [Configurar a alteração automática de senha no SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Todos os Agentes de Gerenciamento
-   Gerenciamento de Certificados da Microsoft
-   BHOLD


Mais informações sobre gMSA podem ser encontradas nos seguintes artigos:
-   [Visão geral das contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Criar a Chave Raiz dos KDS (Serviços de Distribuição de Chaves)](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Crie contas de usuários e grupos

Todos os componentes da implantação do MIM precisam de suas próprias identidades no domínio. Isso inclui os componentes do MIM, como Serviço e Sincronização, bem como o SharePoint e o SQL.


> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio – **dc**
> - Nome do domínio - **contoso**
> - Nome do Servidor de Serviços do MIM – **mimservice**
> - Nome do Servidor de Sincronização do MIM – **mimsync**
> - Nome do SQL Server – **sql**
> - Senha – <strong>Pass@word1</strong>

1. Entre no controlador de domínio como o Administrator do domínio com a senha (*e. g. Contoso\Administrador*).

2. Crie as seguintes contas de usuário para serviços do MIM. Inicie o PowerShell e digite o script do PowerShell a seguir para criar novos usuários de domínio do AD (nem todas as contas são obrigatórias; embora o script seja fornecido apenas para fins informativos, é uma melhor prática usar uma conta *MIMAdmin* dedicada para o processo de instalação do MIM e do SharePoint).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Crie grupos de segurança para todos os grupos.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Adicione os SPNs (Nomes da Entidade de Serviço) para habilitar a autenticação Kerberos para contas de serviço

    ```PowerShell
    Set-ADServiceAccount -Identity svcMIMAppPool -ServicePrincipalNames @{Add="http/mim.contoso.com"}
    ```

5.  Não se esqueça de registrar os registros 'A' de DNS a seguir para a resolução apropriada dos nomes (supondo que os sites da Web do Serviço do MIM, do Portal do MIM, de Redefinição de Senha e de Registro de Senha sejam hospedados no mesmo computador)

    - mim.contoso.com – aponta para o Serviço do MIM e o endereço IP físico do servidor do portal
    - passwordreset.contoso.com – aponta para o Serviço do MIM e o endereço IP físico do servidor do portal
    - passwordregistration.contoso.com – aponta para o Serviço do MIM e o endereço IP físico do servidor do portal

## <a name="create-key-distribution-service-root-key"></a>Criar a Chave Raiz do Serviço de Distribuição de Chaves

Verifique se você está conectado ao controlador de domínio como administrador para preparar o serviço de distribuição de chave de grupo.

Se já houver uma chave raiz para o domínio (use o **Get-KdsRootKey** para verificar) e, em seguida, passe para a próxima seção.

6.  Crie a Chave Raiz dos KDS (Serviços de Distribuição de Chaves) (apenas uma vez por domínio), se necessário. A Chave Raiz é usada pelo serviço KDS em controladores de domínio (juntamente com outras informações) para gerar senhas. Como um administrador de domínio, digite o seguinte comando do PowerShell:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *–EffectiveImmediately* pode exigir um atraso de até \~10 horas, pois será necessário replicar para todos os controladores de domínio. Esse atraso foi de aproximadamente uma hora para os dois controladores de domínio.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >No laboratório ou no ambiente de teste, você pode evitar o atraso de replicação de 10 horas executando o seguinte comando substituto:
    ><br/>
    >Add-KDSRootKey -EffectiveTime ((Get-Date).AddHours(-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>Criar conta de serviço de sincronização, grupo e entidade de serviço do MIM
-----------------------

Verifique se todas as contas de computador nos computadores em que o software do MIM deverá ser instalado já estão associadas ao domínio.  Em seguida, execute essas etapas no PowerShell como um administrador de domínio.

7.  Crie um grupo chamado *MIMSync_Servers* e adicione todos os servidores de sincronização do MIM nesse grupo.
    Digite conforme a seguir para criar um novo grupo do AD para servidores de sincronização do MIM. Em seguida, adicione nesse grupo as contas de computador do Active Directory do servidor de sincronização do MIM – por exemplo, *contoso\MIMSync$* .

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  Crie uma gMSA para o Serviço de Sincronização do MIM. Digite os comandos do PowerShell a seguir.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Verifique os detalhes da gSMA criada executando o comando do PowerShell *Get-ADServiceAccount*:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Se você planeja executar o Serviço de Notificação de Alteração de Senha, será necessário registrar o Nome da Entidade de Serviço executando este comando do PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Reinicialize o servidor de sincronização do MIM para atualizar o token Kerberos associado ao servidor, uma vez que a associação ao grupo "MIMSync_Server" foi alterada.

## <a name="create-mim-service-management-agent-service-account"></a>Criar conta de serviço do Agente de Gerenciamento de Serviços do MIM

11. Normalmente, ao instalar o serviço do MIM, você criará uma nova conta para o Agente de Gerenciamento de Serviços do MIM (conta de MA do MIM).  Com a gMSA, há duas opções disponíveis:

- Usar a conta de serviço gerenciado de grupo do Serviço de Sincronização do MIM e não criar uma conta separada

    Você pode pular a criação da conta de serviço do Agente de Gerenciamento de Serviços do MIM. Nesse caso, use o nome da gMSA do Serviço de Sincronização do MIM – por exemplo, *contoso\MIMSyncGMSAsvc$* – em vez da conta de MA do MIM ao instalar o Serviço do MIM. Posteriormente, na configuração do Agente de Gerenciamento de Serviços do MIM, habilite a opção *'Usar a conta do MIMSync'* .

    Não habilite a opção 'Negar logon da rede' para a gMSA do Serviço de Sincronização do MIM, uma vez que a conta de MA do MIM requer a autorização 'Permitir logon na rede'.

- Usar uma conta de serviço normal para a conta de serviço do Agente de Gerenciamento de Serviços do MIM

    Inicie o PowerShell como um administrador de domínio e digite o seguinte para criar um novo usuário de domínio do AD:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Não habilite a opção 'Negar logon da rede' para a conta de MA do MIM, uma vez que ela requer a autorização 'Permitir logon na rede'.

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>Criar contas de serviço, grupos e a entidade de serviço do MIM

Continue usando o PowerShell como um administrador de domínio.
   
12. Crie um grupo chamado *MIMService_Servers* e adicione todos os servidores de Serviço do MIM nesse grupo.  Digite o comando do PowerShell a seguir para criar um novo grupo do AD para servidores de Serviço do MIM e adicione nesse grupo a conta de computador do Active Directory do servidor de Serviço do MIM – por exemplo, *contoso\MIMPortal$* .

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  Crie a gMSA do Serviço do MIM.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Registre o nome da entidade de serviço e habilite a delegação Kerberos executando o seguinte comando do PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  Para cenários de SSPR, você precisa que a Conta de Serviço do MIM seja capaz de se comunicar com o Serviço de Sincronização do MIM. Portanto, a Conta de Serviço do MIM deve ser membro do MIMSyncAdministrators ou dos grupos de Redefinição de Senha de Sincronização e Procurar:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Reinicialize o servidor de Serviço do MIM para atualizar o token Kerberos associado ao servidor, uma vez que a associação ao grupo "MIMService_Servers" foi alterada.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Criar outras contas e grupos do MIM, se necessário

Se você estiver configurando o SSPR do MIM, então, seguindo as mesmas diretrizes descritas acima para o Serviço de Sincronização do MIM e o Serviço do MIM, você poderá criar outras gMSA para:

- Pool de aplicativos do site da Web para Redefinição de Senha do MIM
- Pool de aplicativos do site da Web para Registro de Senha do MIM

Se você estiver configurando o PAM do MIM, então, seguindo as mesmas diretrizes descritas acima para o Serviço de Sincronização do MIM e o Serviço do MIM, você poderá criar outras gMSA para:

- Pool de aplicativos do site da Web para API REST do PAM do MIM
- Serviço de componente do PAM do MIM
- Serviço de monitoramento do PAM do MIM

## <a name="specifying-a-gmsa-when-installing-mim"></a>Especificação de uma gMSA ao instalar o MIM

Como regra geral, na maioria dos casos, ao usar um instalador do MIM, para especificar que você deseja usar uma gMSA em vez de uma conta normal, acrescente um caractere de cifrão ao nome da gMSA – por exemplo, **contoso\MIMSyncGMSAsvc$** – e deixe o campo de senha vazio. Uma exceção é a ferramenta *miisactivate.exe*, que aceita o nome da gMSA sem o cifrão.
<br/>

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)
