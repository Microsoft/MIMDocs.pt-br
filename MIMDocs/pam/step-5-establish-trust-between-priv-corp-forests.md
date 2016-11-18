---
title: "Etapa 5 para implantar o PAM – link da floresta | Microsoft Docs"
description: "Estabeleça a confiança entre as florestas PRIV e CORP para que os usuários com privilégios em PRIV ainda possam acessar recursos em CORP."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 16208efe08c5a2c0f63ee121c64c45cad5a73909


---

# <a name="step-5-establish-trust-between-priv-and-corp-forests"></a>Etapa 5 – Estabelecer relação de confiança entre florestas PVI e CORP

>[!div class="step-by-step"]
[« Etapa 4](step-4-install-mim-components-on-pam-server.md)
[Etapa 6 »](step-6-transition-group-to-pam.md)


Para cada domínio CORP, como contoso.local, os controladores de domínio PRIV e CONTOSO precisam estar associados por uma relação de confiança. Isso permite que os usuários no domínio PRIV acessem os recursos no domínio CORP.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Conectar cada controlador de domínio a seu equivalente

Antes de estabelecer a relação de confiança, cada controlador de domínio deve ter a resolução de nome DNS configurada para seu equivalente, com base no endereço IP do outro servidor DNS/controlador de domínio.

1.  Se os controladores de domínio ou o servidor com o software MIM forem implantados como máquinas virtuais, verifique se não há nenhum outro servidor DNS que está fornecendo serviços de nomeação de domínios a esses computadores.
    - Se as máquinas virtuais tiverem várias interfaces de rede, incluindo interfaces de rede conectadas a redes públicas, talvez seja necessário desabilitar essas conexões temporariamente ou substituir as configurações de interface de rede do Windows. É importante garantir que um endereço de servidor DNS fornecido pelo DHCP não é usado por nenhuma máquina virtual.

2.  Verifique se cada controlador do domínio CORP existente pode encaminhar nomes para a floresta PRIV. Em cada controlador de domínio fora da floresta PRIV, como CORPDC, inicie o PowerShell e digite o seguinte comando:

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    Verifique se a saída indica um registro de servidor de nomes para o domínio PRIV com o endereço IP correto.

3.  Se o controlador de domínio não pôde encaminhar o domínio PRIV, use o **Gerenciador DNS** (localizado em **Iniciar** > **Ferramentas do Aplicativo** > **DNS**) para configurar o encaminhamento de nome DNS para o domínio PRIV ao endereço IP de PRIVDC. Se ele for um domínio superior (por exemplo, contoso.local), expanda os nós desse controlador de domínio e seu domínio, como **CORPDC** > **Zonas de Pesquisa Direta** > **contoso.local**, e garanta que uma chave chamada **priv** está presente como um tipo NS (Servidor de Nomes).

    ![estrutura de arquivos de chave privada – captura de tela](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Estabelecer relação de confiança em PAMSRV

Em PAMSRV, estabeleça uma relação de confiança unidirecional com CORPDC, para que o controlador de domínio CORP confie na floresta PRIV.

1. Entre em PAMSRV como administrador de domínio PRIV (PRIV\Administrator).

2.  Inicie o PowerShell.

3.  Digite os seguintes comandos do PowerShell para cada floresta existente. Insira as credenciais do administrador de domínio CORP (CONTOSO\Administrator), quando solicitado.

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Digite os seguintes comandos do PowerShell para cada domínio nas florestas existentes. Insira as credenciais do administrador de domínio CORP (CONTOSO\Administrator), quando solicitado.

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Conceder acesso de leitura de florestas ao Active Directory

Para cada floresta existente, habilite o acesso de leitura ao AD por administradores PRIV e o serviço de monitoramento.

1.  Entre no controlador de domínio da floresta CORP existente, (CORPDC), como administrador do domínio primário nessa floresta (Contoso\Administrator).  
2.  Inicie **Usuários e Computadores do Active Directory**.  
3.  Clique com o botão direito do mouse no domínio **contoso.local** e selecione **Delegar Controle**.  
4.  Na guia Usuários e Grupos Selecionados, clique em **Adicionar**.  
5.  Na janela Selecionar Usuários, Computadores ou Grupos, clique em **Locais** e altere o local para *priv.contoso.local*.  No nome do objeto, digite *Administradores de Domínio* e clique em **Verificar Nomes**. Quando um pop-up for exibido, insira o nome de usuário *priv\administrator* e a senha.  
6.  Após Administradores de Domínio, adicione “*; MIMMonitor*”. Depois que os nomes **Administradores de Domínio** e **MIMMonitor** forem sublinhados, clique em **OK** e em **Avançar**.  
7.  Na lista de tarefas comuns, selecione **Ler todas as informações do usuário**, clique em **Avançar** e em **Concluir**.  
8.  Feche Usuários e Computadores do Active Directory.

9.  Abra uma janela do PowerShell.  
10.  Use `netdom` para garantir que o histórico do SID está habilitado e a filtragem de SID desabilitada. Tipo:  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    A saída deve indicar **Habilitando histórico do SID para essa relação de confiança** ou **O histórico do SID já está habilitado para essa relação de confiança**.

    A saída também deve indicar que **A filtragem de SID não está habilitada para essa relação de confiança**. Veja [Disable SID filter quarantining](http://technet.microsoft.com/library/cc772816.aspx) (Desabilitar quarentena de filtro de SID) para obter mais informações.

## <a name="start-the-monitoring-and-component-services"></a>Iniciar os serviços de Monitoramento e Componente

1.  Entre em PAMSRV como administrador de domínio PRIV (PRIV\Administrator).

2.  Inicie o PowerShell.

3.  Digite os seguintes comandos do PowerShell.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

Na próxima etapa, você moverá um grupo para o PAM.

>[!div class="step-by-step"]
[« Etapa 4](step-4-install-mim-components-on-pam-server.md)
[Etapa 6 »](step-6-transition-group-to-pam.md)



<!--HONumber=Nov16_HO2-->


