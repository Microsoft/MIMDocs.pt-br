---
title: Configurar o Windows Server 2016 ou 2019 para o MIM 2016 SP2 | Microsoft Docs
description: Conheça as etapas e os requisitos mínimos para preparar o Windows Server 2016 ou 2019 para funcionar com o MIM 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cf8261c4e6f6529fd82760206b62b689a75d0acb
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492405"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>Configurar um servidor de gerenciamento de identidade: Windows Server 2016 ou 2019

> [!div class="step-by-step"]
> [« Como preparar um domínio](preparing-domain.md)
> [SQL Server »](prepare-server-sql2016.md)
> 

> [!NOTE]
> O procedimento de instalação do Windows Server 2019 é igual ao do Windows Server 2016.


> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome do domínio - **contoso**
> - Nome do Servidor do Serviço MIM - **corpservice**
> - Nome do Servidor de Sincronização do MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Senha – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Adicione o Windows Server 2016 ao seu domínio

Comece com um computador com Windows Server 2016, com no mínimo de 8 a 12 GB de RAM. Ao instalar, especifique a edição "Windows Server 2016 Standard/Datacenter (servidor com GUI) x64".

1. Faça logon no novo computador como administrador.

2. Usando o painel de controle, dê ao computador um endereço IP estático em uma rede. Configure esse adaptador de rede para enviar consultas DNS para o endereço IP do controlador de domínio na etapa anterior e defina o nome do computador para **CORPSERVICE**.  Esta operação exigirá uma reinicialização do servidor.

3. Abra o painel de controle e ingresse o computador no domínio que você configurou na última etapa, *contoso.com*.  Esta operação inclui o fornecimento do nome de usuário e das credenciais de um administrador de domínio, tal como *Contoso\Administrador*.  Depois que a mensagem de boas-vindas for exibida, feche a caixa de diálogo e reinicie o servidor novamente.

4. Entre no computador *CORPSERVICE* como uma conta de domínio com administrador do computador local, por exemplo *Contoso\MIMINSTALL*.


5. Inicie uma janela do PowerShell como administrador e digite o seguinte comando para atualizar o computador com as configurações da política de grupo.

    ```
    gpupdate /force /target:computer
    ```

    Após não mais que minuto, será concluído com a mensagem "A atualização da política do computador foi concluída com sucesso".

6. Adicione as funções **Servidor Web (IIS)** e **Servidor de Aplicativos** , os recursos **.NET Framework** 3.5, 4.0 e 4.5 e o módulo **Active Directory** para o Windows PowerShell.

    ![Imagem de recursos do PowerShell](media/MIM-DeployWS2.png)

7. No PowerShell, digite os comandos a seguir. Observe que talvez seja necessário especificar um local diferente para os arquivos de origem para os recursos do **.NET Framework** 3.5. Normalmente, esses recursos não estão presentes no momento da instalação do Windows Server, mas estão disponíveis na pasta SxS (lado a lado) na pasta de origens do disco de instalação do sistema operacional, por exemplo, “*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Configurar a política de segurança do servidor

Configure a política de segurança do servidor para permitir que as contas criadas recentemente sejam executadas como serviço.
> [!NOTE] 
> Dependendo da sua configuração, o servidor único (tudo em um) ou servidores distribuídos que você só precisará adicionar, com base na função da máquina membro, como servidor de sincronização. 

1. Inicie o programa de Política de Segurança Local

2. Navegue até **Políticas Locais > Atribuição de Direitos do Usuário**.

3. No painel de detalhes, clique com o botão direito do mouse em **Fazer logon como um serviço** e selecione **Propriedades**.

    ![Imagem de política de segurança local](media/MIM-DeployWS3.png)

4. Clique em **Adicionar Usuário ou Grupo** e, na caixa de texto, digite o seguinte com base na função `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, clique em **Verificar Nomes** e clique em **OK**.

5. Clique em **OK** para fechar a janela **Propriedades de Logon como um serviço**.

6.  No painel de detalhes, clique com o botão direito do mouse em **Negar acesso a este computador pela rede** e selecione **Propriedades**.>

7. Clique em **Adicionar Usuário ou Grupo** e na caixa de texto digite `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

8. Clique em **OK** para fechar a janela **Negar acesso a este computador das Propriedades de rede**.

9. No painel de detalhes, clique com o botão direito do mouse em **Negar logon localmente** e selecione **Propriedades**.

10. Clique em **Adicionar Usuário ou Grupo** e na caixa de texto digite `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

11. Clique em **OK** para fechar a janela **Negar logon em Propriedades locais**.

12. Feche a janela de Política de Segurança Local.

## <a name="software-prerequisites"></a>Pré-requisitos de software

Antes de instalar os componentes do MIM 2016 SP2, certifique-se de instalar todos os pré-requisitos de software:

13. Instale os [Pacotes Redistribuíveis do Visual C++ 2013](https://www.microsoft.com/download/details.aspx?id=40784).

14. Instale o .NET Framework 4.6.

15. No servidor que o hospedará, o Serviço de Sincronização do MIM precisará do [SQL Server Native Client](https://www.microsoft.com/download/details.aspx?id=50402).

16. No servidor que o hospedará, o Serviço do MIM precisará do .NET Framework 3.5.

17. Opcionalmente, se estiver usando o modo FIPS ou TLS 1.2, confira [MIM 2016 SP2 em "Apenas TL 1.2" ou ambientes de modo FIPS](preparing-tls.md).

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Altere o Modo de autenticação do Windows do IIS, se for necessário

1.  Abra uma janela do PowerShell.

2.  Pare o IIS com o comando *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [« Como preparar um domínio](preparing-domain.md)
> [SQL Server »](prepare-server-sql2016.md)
