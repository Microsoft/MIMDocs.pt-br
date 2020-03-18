---
title: Instalar o Serviço de Sincronização do Microsoft Identity Manager | Microsoft Docs
description: Comece a usar os componentes do MIM 2016 instalando e configurando o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 8e4371c8d3caac06f7200d8439b30b7aa978a336
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042450"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Instalação do MIM 2016: Serviço de Sincronização do MIM

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [Serviço MIM e Portal »](install-mim-service-portal.md)
 
> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome do domínio - **contoso**
> - Nome do Servidor do Serviço MIM - **corpservice**
> - Nome do Servidor de Sincronização do MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Senha – <strong>Pass@word1</strong>

Para instalar os componentes do Microsoft Identity Manager 2016, primeiramente configure o pacote de instalação.

1. Entre como *contoso\miminstall* no servidor que você está usando para o servidor de sincronização de gerenciamento de identidade **corpsync**.

2. Descompacte o pacote de instalação do MIM ou monte o DVD de imagem do MIM.  Caso não tenha este DVD, confira [Downloads e licenciamento do Microsoft Identity Manager](microsoft-identity-manager-licensing.md).

## <a name="install-mim-2016-sp1-synchronization-service"></a>Instalar o Serviço de Sincronização do MIM 2016 SP1

1. Na pasta de instalação descompactada do MIM, navegue até a pasta **Serviço de Sincronização** .

2. Execute o **Instalador do Serviço de Sincronização do MIM**. Siga as diretrizes do instalador e conclua a instalação.

3. Na tela de boas-vindas, clique em **Avançar**.

    ![Imagem de boas-vindas do assistente do instalador do MIM](media/install-mim-sync/MIM_Install1.png)

4. Leia os termos de licença e clique em **Avançar** para aceitá-los.

5. Na tela **Instalação Personalizada** clique em **Avançar**.

    ![Imagem de configuração personalizada](media/install-mim-sync/MIM_Install2.png)

6. Na tela de configuração do banco de dados de Serviço de sincronização, selecione:

   1.  O SQL Server está localizado em: **Um computador remoto** chamado **corpsql.contoso.com**.

   2.  A instância do SQL Server é: **A instância padrão**

   ![Imagem de conexão de banco de dados](media/install-mim-sync/MIM_Install3.png)

    3. *MIM 2016 SP2 e posterior*: Configurar o nome do Banco de dados do serviço de sincronização do MIM

7. Configure a conta de serviço de sincronização de acordo com a conta criada anteriormente:

   1. Conta de serviço: *MIMSync*

   2. Senha: <em>Pass@word1</em>

   3. Domínio da conta de serviço ou nome do computador local: *contoso*

    >[!NOTE]
    >MIM 2016 SP2 e posterior: para Contas de Serviço Gerenciado de Grupo, verifique se o caractere **$** está no final do Nome da Conta de Serviço, por exemplo, MIMSync$, e deixe o campo de senha vazio.

    ![Imagem de conta de serviço](media/install-mim-sync/MIM_Install4.png)

8. Fornece ao instalador do Serviço de sincronização do MIM os grupos de segurança relevantes:

   1. Administrador = *contoso\MIMSyncAdmins*

   2. Operador= *contoso\MIMSyncOperators*

   3. Ligação = *contoso\MIMSyncJoiners*

   4. Navegador do conector = *contoso\MIMSyncBrowse*

   5. Gerenciamento de senha do WMI = *contoso\MIMSyncPasswordReset*

   ![Imagem de grupos de segurança](media/install-mim-sync/MIM_Install5.png)

9. Na tela de configurações de segurança, marque **Habilitar regras de firewall para Comunicações RPC de entrada** e clique em **Avançar**.

10. Clique em **Instalar** para iniciar a instalação do Serviço de Sincronização do MIM.

    1. Um aviso sobre a conta de serviço de sincronização do MIM pode aparecer – clique em **OK**.

    2. O Serviço de sincronização do MIM será instalado.

    3. Um aviso sobre a criação de um backup da chave de criptografia é exibido. Clique em **OK** e selecione uma pasta para armazenar o backup da chave de criptografia.

    ![Imagem da chave de criptografia de backup de Sync do MIM](media/MIM-Install7.png)

    4. Quando o instalador concluir com sucesso a instalação, clique em **Concluir**.

    5. Você precisa sair e entrar para que as alterações de associação de grupo entrem em vigor. Clique em **Sim** Sair.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [Serviço MIM e Portal »](install-mim-service-portal.md)
