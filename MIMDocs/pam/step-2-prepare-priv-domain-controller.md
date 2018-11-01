---
title: Etapa 2 para implantar o PAM – PRIV DC | Microsoft Docs
description: Prepare o controlador de domínio PRIV, que fornecerá o ambiente de bastiões em que o Privileged Access Management é isolado.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0364b0d14d08ab3af32010675e0ba80d39404342
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380044"
---
# <a name="step-2---prepare-the-first-priv-domain-controller"></a>Etapa 2 – Preparar o primeiro controlador de domínio PRIV

> [!div class="step-by-step"]
> [« Etapa 1](step-1-prepare-corp-domain.md)
> [Etapa 3 »](step-3-prepare-pam-server.md)

Nesta etapa, você vai criar um novo domínio que fornecerá o ambiente de bastiões para autenticação do administrador.  Essa floresta precisará de, pelo menos, um controlador de domínio e um servidor membro. O servidor membro será configurado na próxima etapa.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>Criar um novo controlador de domínio do Privileged Access Management

Nesta seção, você vai configurar uma máquina virtual para atuar como um controlador de domínio para uma nova floresta

### <a name="install-windows-server-2012-r2"></a>Instalar o Windows Server 2012 R2

Em outra máquina virtual nova sem software instalado, instale o Windows Server 2012 R2 para tornar um computador “PRIVDC”.

1. Selecione executar uma instalação personalizada (não atualização) do Windows Server. Durante a instalação, especifique **Windows Server 2012 R2 Standard (Servidor com GUI) x64**; _não selecione_ **Data Center nem Server Core**.

2. Leia e aceite os termos de licença.

3. Já que o disco estará vazio, selecione **Personalizar: instalar somente o Windows** e use o espaço em disco não inicializado.

4. Depois de instalar a versão do sistema operacional, entre nesse novo computador como o novo administrador. Use o Painel de Controle para definir o nome do computador como *PRIVDC*, dê a ele um endereço IP estático na rede virtual e configure o servidor DNS para ser o do controlador de domínio instalado na etapa anterior. Isso exigirá reinicializar o servidor.

5. Após a reinicialização do servidor, entre como administrador. Usando o Painel de Controle, configure o computador para verificar se há atualizações e instale todas as atualizações necessárias. Isso pode exigir reinicializar o servidor.

### <a name="add-roles"></a>Adicionar funções

Adicione as funções AD DS (Serviços de Domínio do Active Directory) e Servidor DNS.

1. Inicie o PowerShell como um administrador.

2. Digite os comandos a seguir para se preparar para uma instalação do Active Directory do Windows Server.

   ```PowerShell
   import-module ServerManager

   Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
   ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>Definir configurações do Registro para a migração do Histórico de SID

Inicie o PowerShell e digite o seguinte comando para configurar o domínio de origem, a fim de permitir o acesso de RPC (chamada de procedimento remoto) ao banco de dados do SAM (gerente de contas de segurança).

```PowerShell
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>Criar uma nova floresta do Privileged Access Management

Em seguida, promova o servidor para um controlador de domínio em uma nova floresta.

Neste documento, o nome priv.contoso.local é usado como o nome de domínio da nova floresta.  O nome da floresta não é crítico e não precisa ser subordinado a um nome de floresta existente na organização. No entanto, o domínio e os nomes de NetBIOS da nova floresta devem ser exclusivos e diferentes daqueles de qualquer outro domínio na organização.  

### <a name="create-a-domain-and-forest"></a>Criar um domínio e uma floresta

1. Em uma janela do PowerShell, digite os seguintes comandos para criar o novo domínio.  Isso também criará uma delegação de DNS em um domínio superior (contoso.local) que foi criado na etapa anterior.  Se você pretende configurar o DNS mais tarde, omita os parâmetros `CreateDNSDelegation -DNSDelegationCredential $ca`.

   ```PowerShell
   $ca= get-credential
   Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
   ```

2. Quando o pop-up for exibido, forneça as credenciais para o administrador da floresta CORP (por exemplo, o nome de usuário CONTOSO\\Administrator e a senha correspondente da etapa 1).

3. Na janela do PowerShell, será solicitado o uso de uma Senha do Administrador no Modo de Segurança. Insira uma nova senha duas vezes. As mensagens de aviso para configurações de criptografia e delegação de DNS serão exibidas. Isso é normal.

Após a conclusão da criação da floresta, o servidor será reiniciado automaticamente.

### <a name="create-user-and-service-accounts"></a>Criar contas de usuário e de serviço

Crie contas de usuário e de serviço para a instalação do Portal e Serviço MIM. Essas contas entrarão no contêiner Users do domínio priv.contoso.local.

1. Após a reinicialização do servidor, entre em PRIVDC como administrador de domínio (PRIV\\Administrator).

2. Inicie o PowerShell e digite os comandos a seguir. A senha 'Pass@word1' é apenas um exemplo e você deve usar uma senha diferente para as contas.

   ```PowerShell
   import-module activedirectory

   $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   New-ADUser –SamAccountName MIMMA –name MIMMA

   Set-ADAccountPassword –identity MIMMA –NewPassword $sp

   Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

   Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

   Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

   Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

   Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMSync –name MIMSync

   Set-ADAccountPassword –identity MIMSync –NewPassword $sp

   Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMService –name MIMService

   Set-ADAccountPassword –identity MIMService –NewPassword $sp

   Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SharePoint –name SharePoint

   Set-ADAccountPassword –identity SharePoint –NewPassword $sp

   Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SqlServer –name SqlServer

   Set-ADAccountPassword –identity SqlServer –NewPassword $sp

   Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

   Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

   Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

   New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

   Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

   Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

   Add-ADGroupMember "Domain Admins" SharePoint

   Add-ADGroupMember "Domain Admins" MIMService
   ```

### <a name="configure-auditing-and-logon-rights"></a>Configurar a auditoria e os direitos de logon

Você precisa configurar a auditoria para que a configuração do PAM seja estabelecida entre as florestas.

1. Verifique se você está conectado como administrador do domínio (PRIV\\Administrator).

2. Vá para **Iniciar** > **Ferramentas Administrativas** > **Gerenciamento de Política de Grupo**.

3. Navegue até **Floresta: priv.contoso.local** > **Domínios** > **priv.contoso.local** > **Controladores de Domínio** > **Política de Controladores de Domínio Padrão**. Uma mensagem de aviso será exibida.

4. Clique com o botão direito do mouse em **Política de Controladores de Domínio Padrão** e selecione **Editar**.

5. Na árvore de console do Editor de Gerenciamento de Política de Grupo, na árvore Política de Controladores de Domínio Padrão, navegue até **Configuração do Computador** > **Políticas** > **Configurações do Windows** > **Configurações de Segurança** > **Políticas Locais** > **Política de Auditoria**.

6. No painel Detalhes, clique com o botão direito do mouse em **Gerenciamento de conta de auditoria** e selecione **Propriedades**. Clique em **Definir estas configurações de política**, marque a caixa de seleção **Êxito** , marque a caixa de seleção **Falha** , clique em **Aplicar** e em **OK**.

7. No painel Detalhes, clique com o botão direito do mouse em **Acesso ao serviço de diretório de auditoria** e selecione **Propriedades**. Clique em **Definir estas configurações de política**, marque a caixa de seleção **Êxito** , marque a caixa de seleção **Falha** , clique em **Aplicar** e em **OK**.

8. Navegue até **Configuração do Computador** > **Políticas** > **Configurações do Windows** > **Configurações de Segurança** > **Políticas de Conta** > **Política do Kerberos**.

9. No painel Detalhes, clique com o botão direito do mouse em **Tempo de vida máximo para o tíquete de usuário** e selecione **Propriedades**. Clique em **Definir essas configurações de política**, defina o número de horas para *1*, clique em **Aplicar** e em **OK**. Observe que as outras configurações na janela também serão alteradas.

10. Na janela Gerenciamento de Política de Grupo, selecione **Política de Domínio Padrão**, clique com o botão direito do mouse e selecione **Editar**.

11. Expanda **Configuração do Computador** > **Políticas** > **Configurações do Windows** > **Configurações de Segurança** > **Políticas Locais** e selecione **Atribuição de Direitos do Usuário**.

12. No painel Detalhes, clique com o botão direito do mouse em **Negar logon como um trabalho em lotes** e selecione **Propriedades**.

13. Marque a caixa de seleção **Definir estas Configurações de Políticas**, clique em **Adicionar Usuário ou Grupo** e, no campo Nomes de usuário e grupo, digite *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* e clique em **OK**.

14. Clique em **OK** para fechar a janela.

15. No painel Detalhes, clique com o botão direito do mouse em **Negar logon por meio dos Serviços de Área de Trabalho Remota** e selecione **Propriedades**.

16. Clique na caixa de seleção **Definir estas Configurações de Políticas**, clique em **Adicionar Usuário ou Grupo** e, no campo Nomes de usuário e grupo, digite *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* e clique em **OK**.

17. Clique em **OK** para fechar a janela.

18. Feche a janela do Editor de Gerenciamento de Política de Grupo e a janela Gerenciamento de Política de Grupo.

19. Inicie uma janela do PowerShell como administrador e digite o seguinte comando para atualizar o controlador de domínio nas configurações da política de grupo.

    ```cmd
    gpupdate /force /target:computer
    ```

    Após um minuto, será concluído com a mensagem "A atualização da política do computador foi concluída com sucesso."


### <a name="configure-dns-name-forwarding-on-privdc"></a>Configurar o encaminhamento de nome DNS no PRIVDC

Usando o PowerShell em PRIVDC, configure o encaminhamento de nome DNS para que o domínio PRIV reconheça outras florestas existentes.

1. Inicie o PowerShell.

2. Para cada domínio na parte superior de cada floresta existente, digite o seguinte comando, especificando o domínio DNS existente (por exemplo, contoso.local) e o endereço IP do servidor mestre do domínio.  

   Se você criou um domínio contoso.local na etapa anterior, especifique *10.1.1.31* para o endereço IP da rede virtual do computador CORPDC.

   ```PowerShell
   Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
   ```

> [!NOTE]
> As outras florestas também devem poder encaminhar consultas DNS da floresta PRIV para esse controlador de domínio.  Se você tiver várias florestas do Active Directory, também deverá adicionar um encaminhador DNS condicional a cada uma dessas florestas.

### <a name="configure-kerberos"></a>Configurar o Kerberos

1. Usando o PowerShell, adicione SPNs para que o SharePoint, a API REST do PAM e o Serviço MIM possam usar a autenticação Kerberos.

   ```cmd
   setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
   setspn -S http/pamsrv PRIV\SharePoint
   setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
   setspn -S FIMService/pamsrv PRIV\MIMService
   ```

> [!NOTE]
> As próximas etapas deste documento descrevem como instalar os componentes do servidor MIM 2016 em um único computador. Se você planeja adicionar outro servidor para obter alta disponibilidade, será necessária uma configuração adicional do Kerberos, conforme descrito em [FIM 2010: configuração da autenticação Kerberos](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>Configurar a delegação para conceder acesso de contas do serviço MIM

Realize as seguintes etapas em PRIVDC como um administrador de domínio.

1. Inicie **Usuários e Computadores do Active Directory**.
2. Clique com o botão direito do mouse no domínio **priv.contoso.local** e selecione **Delegar Controle**.
3. Na guia Usuários e Grupos Selecionados, clique em **Adicionar**.
4. Na janela Selecionar Usuários, Computadores ou Grupos, digite *mimcomponent; mimmonitor; mimservice* e clique em **Verificar Nomes**. Depois que os nomes forem sublinhados, clique em **OK** e em **Avançar**.
5. Na lista de tarefas comuns, selecione **Criar, excluir e gerenciar contas de usuário** e **Modificar a associação de um grupo**, e clique em **Avançar** e em **Concluir**.

6. Novamente, clique com o botão direito do mouse no domínio **priv.contoso.local** e selecione **Delegar Controle**.
7. Na guia Usuários e Grupos Selecionados, clique em **Adicionar**.  
8. Na janela Selecionar Usuários, Computadores ou Grupos, insira *MIMAdmin* e clique em **Verificar Nomes**. Depois que os nomes forem sublinhados, clique em **OK** e em **Avançar**.
9. Selecione **tarefa personalizada**, aplique à **Essa pasta**, com **Permissões gerais**.
10. Na lista de permissões, selecione o seguinte:
    - **Ler**
    - **Gravar**
    - **Criar todos os Objetos Filho**
    - **Excluir todos os Objetos Filho**
    - **Ler Todas as Propriedades**
    - **Gravar Todas as Propriedades**
    - **Migrar Histórico de SID** Clique em **Avançar** e, em seguida, em **Concluir**.

11. Mais uma vez, clique com o botão direito do mouse no domínio **priv.contoso.local** e selecione **Delegar Controle**.  
12. Na guia Usuários e Grupos Selecionados, clique em **Adicionar**.  
13. Na janela Selecionar Usuários, Computadores ou Grupos, insira *MIMAdmin* e clique em **Verificar Nomes**. Depois que os nomes forem sublinhados, clique em **OK**e em **Avançar**.  
14. Selecione **tarefa personalizada**, aplique à **Essa pasta**e em clique em **apenas objetos do Usuário**.    
15. Na lista de permissões, selecione **Alterar senha** e **Redefinir senha**. Clique em **Avançar** e em **Concluir**.  
16. Feche Usuários e Computadores do Active Directory.

17. Abra um prompt de comando.  
18. Examine a lista de controle de acesso no objeto AdminSDHolder nos domínios PRIV. Por exemplo, se o domínio era “priv.contoso.local”, digite o comando
    ```cmd
    dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
    ```
19. Atualize a lista de controle de acesso, conforme necessário, para garantir que o serviço MIM e o serviço do componente do MIM possa atualizar as associações de grupos protegidos por essa ACL.  Digite o comando:

```cmd
dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"
dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
```

20. Reinicie o servidor PRIVDC para que essas alterações entrem em vigor.

## <a name="prepare-a-priv-workstation"></a>Preparar uma estação de trabalho PRIV

Se você ainda não tiver um computador de estação de trabalho que será ingressado no domínio PRIV para realizar a manutenção de recursos PRIV (como o MIM), siga estas instruções para preparar uma estação de trabalho.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Instalar o Windows 8.1 ou Windows 10 Enterprise

Em outra máquina virtual nova sem software instalado, instale o Windows 8.1 Enterprise ou Windows 10 Enterprise para tornar um computador *“PRIVWKSTN”*.

1. Use as configurações Express durante a instalação.

2. Observe que a instalação pode não ser capaz de se conectar à Internet. Clique para **Criar uma conta local**. Especifique um nome de usuário diferente; não use "Administrador" nem "Jen".

3. Usando o Painel de Controle, dê a esse computador um endereço IP estático na rede virtual e defina o servidor DNS preferencial da interface para que seja o mesmo que o do servidor PRIVDC.

4. Usando o Painel de Controle, ingresse no domínio o computador de PRIVWKSTN no domínio priv.contoso.local. Para isso, será necessário fornecer as credenciais do administrador de domínio PRIV. Após a conclusão, reinicie o computador PRIVWKSTN.

Se você desejar obter mais detalhes, veja [Securing privileged access workstations](https://technet.microsoft.com/library/mt634654.aspx) (Protegendo estações de trabalho de acesso privilegiado).

Na próxima etapa, você vai preparar um servidor PAM.

> [!div class="step-by-step"]
> [« Etapa 1](step-1-prepare-corp-domain.md)
> [Etapa 3 »](step-3-prepare-pam-server.md)
