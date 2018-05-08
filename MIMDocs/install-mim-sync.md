---
title: Instalar o Serviço de Sincronização do Microsoft Identity Manager | Microsoft Docs
description: Comece a usar os componentes do MIM 2016 instalando e configurando o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f5204d0396e2b485c9e8c210906703c467a67994
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/27/2018
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Instalação do MIM 2016: Serviço de Sincronização do MIM

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Serviço MIM e Portal »](install-mim-service-portal.md)

> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome do domínio - **contoso**
> - Nome do Servidor do Serviço MIM - **corpservice**
> - Nome do Servidor de Sincronização do MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Senha – **Pass@word1**

Para instalar os componentes do Microsoft Identity Manager 2016, primeiramente configure o pacote de instalação.

1. Entre como *contoso\miminstall* no servidor que você está usando para o servidor de sincronização de gerenciamento de identidade **corpsync**.

2. Descompacte o pacote de instalação do MIM ou monte o DVD de imagem do MIM.

## <a name="install-mim-2016-sp1-synchronization-service"></a>Instalar o Serviço de Sincronização do MIM 2016 SP1

1. Na pasta de instalação descompactada do MIM, navegue até a pasta **Serviço de Sincronização** .

2. Execute o **Instalador do Serviço de Sincronização do MIM**. Siga as diretrizes do instalador e conclua a instalação.

3. Na tela de boas-vindas, clique em **Avançar**.

    ![Imagem de boas-vindas do assistente do instalador do MIM](media/MIM-Install1.png)

4. Leia os termos de licença e clique em **Avançar** para aceitá-los.

5. Na tela **Instalação Personalizada** clique em **Avançar**.

    ![Imagem de configuração personalizada](media/MIM-Install2.png)

6.  Na tela de configuração do banco de dados de Serviço de sincronização, selecione:

    1.  O SQL Server está localizado em: **Este computador**.

    2.  A instância do SQL Server é: **A instância padrão**.

    ![Imagem de conexão de banco de dados](media/MIM-Install3.png)

7.  Configure a conta de serviço de sincronização de acordo com a conta criada anteriormente:

    1.  Conta de serviço: *MIMSync*

    2.  Senha: *Pass@word1*

    3.  Domínio da conta de serviço ou nome do computador local: *contoso*

    ![Imagem de conta de serviço](media/MIM-Install4.png)

8.  Fornece ao instalador do Serviço de sincronização do MIM os grupos de segurança relevantes:

    1. Administrador = *contoso\MIMSyncAdmins*

    2. Operador= *contoso\MIMSyncOperators*

    3. Ligação = *contoso\MIMSyncJoiners*

    4. Navegador do conector = *contoso\MIMSyncBrowse*

    5. Gerenciamento de senha do WMI = *contoso\MIMSyncPasswordReset*

    ![Imagem de grupos de segurança](media/MIM-Install5.png)

9. Na tela de configurações de segurança, marque **Habilitar regras de firewall para Comunicações RPC de entrada** e clique em **Avançar**.

10. Clique em **Instalar** para iniciar a instalação do Serviço de Sincronização do MIM.

    1. Um aviso sobre a conta de serviço de sincronização do MIM pode aparecer – clique em **OK**.

    2. O Serviço de sincronização do MIM será instalado.

    3. Um aviso sobre a criação de um backup da chave de criptografia é exibido. Clique em **OK** e selecione uma pasta para armazenar o backup da chave de criptografia.

        ![Imagem da chave de criptografia de backup de Sync do MIM](media/MIM-Install7.png)

    4. Quando o instalador concluir com sucesso a instalação, clique em **Concluir**.

    5. Você precisa sair e entrar para que as alterações de associação de grupo entrem em vigor. Clique em **Sim** Sair.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Serviço MIM e Portal »](install-mim-service-portal.md)
