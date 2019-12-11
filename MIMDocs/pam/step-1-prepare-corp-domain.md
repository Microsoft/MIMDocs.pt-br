---
title: Etapa 1 para implantar o PAM – Domínio CORP | Microsoft Docs
description: Prepare o domínio CORP com identidades novas ou existentes para serem gerenciadas pelo Privileged Identity Manager
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: dda6a05337e9a9778135fc7d833a38369540b3e0
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518177"
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>Etapa 1 – Preparar o host e o domínio CORP

> [!div class="step-by-step"]
> [Etapa 2 »](step-2-prepare-priv-domain-controller.md)

Nesta etapa, você vai se preparar para hospedar o ambiente de bastiões. Se necessário, você também criará um controlador de domínio e uma estação de trabalho membro em um novo domínio e uma nova floresta (a floresta *CORP*) com identidades a serem gerenciados pelo ambiente de bastiões. Essa floresta CORP simula uma floresta existente que tem os recursos a serem gerenciados. Este documento inclui um recurso de exemplo a ser protegido, um compartilhamento de arquivos.

Se você já tiver um domínio existente do AD (Active Directory) com um controlador de domínio que executa o Windows Server 2012 R2 ou posterior, no qual você é um administrador de domínio, será possível usar esse domínio.  

## <a name="prepare-the-corp-domain-controller"></a>Preparar o controlador de domínio CORP

Esta seção descreve como configurar um controlador de domínio para um domínio CORP. No domínio CORP, os usuários administrativos são gerenciados pelo ambiente de bastiões. O nome DNS (Sistema de Nomes de Domínio) do domínio CORP usado neste exemplo é *contoso.local*.

### <a name="install-windows-server"></a>Instalar o Windows Server

Instale o Windows Server 2012 R2 ou Windows Server 2016 Technical Preview 4 ou posterior em uma máquina virtual para criar um computador chamado *CORPDC*.

1. Escolha **Windows Server 2012 R2 Standard (Servidor com GUI) x64** ou **Windows Server 2016 Technical Preview (Servidor com a Experiência Desktop)** .

2. Leia e aceite os termos de licença.

3. Já que o disco estará vazio, selecione **Personalizado: instalar somente o Windows** e use o espaço em disco não inicializado.

4. Entre no novo computador como administrador. Navegue até o Painel de Controle. Defina o nome do computador como *CORPDC* e dê a ele um endereço IP estático na rede virtual. Reinicie o servidor.

5. Após a reinicialização do servidor, entre como um administrador. Navegue até o Painel de Controle. Configure o computador para verificar se há atualizações e instale todas as atualizações necessárias. Reinicie o servidor.

### <a name="add-roles-to-establish-a-domain-controller"></a>Adicionar funções para estabelecer um controlador de domínio

Nesta seção, você vai adicionar as funções AD DS (Serviços de Domínio do Active Directory), Servidor DNS e Servidor de Arquivos (parte da seção Serviços de Arquivo e Armazenamento) e promover este servidor a um controlador de domínio de uma nova floresta chamada contoso.local.

> [!NOTE]  
> Se você já tiver um domínio para usar como o domínio CORP e esse domínio usar o Windows Server 2012 R2 ou posterior como o controlador de domínio, será possível ir para [Criar usuários adicionais e grupos para fins de demonstração](#create-additional-users-and-groups-for-demonstration-purposes).

1. Enquanto estiver conectado como administrador, inicie o PowerShell.

2. Digite os seguintes comandos.

   ```PowoerShell
   import-module ServerManager

   Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

   Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
   ```

   Isso solicitará uma senha de administrador no modo de segurança para usar. Observe que as mensagens de aviso para configurações de criptografia e delegação de DNS serão exibidas. Isso é normal.

3. Após a conclusão da criação da floresta, saia do serviço. O computador será reiniciado automaticamente.

4. Após a reinicialização do servidor, entre em CORPDC como administrador de domínio. Normalmente, esse é o usuário CONTOSO\\Administrator, que terá a senha que foi criada quando você instalou o Windows em CORPDC.

### <a name="create-a-group"></a>Criar um grupo

Crie um grupo para fins de auditoria pelo Active Directory, se o grupo não existir. O nome do grupo deve ser o nome de domínio NetBIOS seguido de três cifrões, por exemplo, *CONTOSO$$$* .

Para cada domínio, entre em um controlador de domínio como administrador de domínio e realize as seguintes etapas:

1. Inicie o PowerShell.

2. Digite os seguintes comandos, mas substitua “CONTOSO” pelo nome NetBIOS de seu domínio.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
   ```

Em alguns casos, o grupo já pode existir – isso é normal se o domínio também foi usado em cenários de migração do AD.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Criar usuários e grupos adicionais para fins de demonstração

Se você criou um novo domínio CORP, será necessário criar usuários e grupos adicionais para demonstrar o cenário de PAM. O usuário e o grupo para fins de demonstração não devem ser administradores de domínio nem controlados pelas configurações de adminSDHolder no AD.

> [!NOTE]
> Se você já tiver um domínio que será usado como o domínio CORP e ele tiver um usuário e um grupo que podem ser usados para fins de demonstração, vá para a seção [Configurar a auditoria](#configure-auditing).

Vamos criar um grupo de segurança chamado *CorpAdmins* e um usuário chamado *Julia*. Se desejar, você poderá usar nomes diferentes.

1. Inicie o PowerShell.

2. Digite os seguintes comandos. Substitua a senha 'Pass@word1' por uma cadeia de caracteres de senha diferente.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

   New-ADUser –SamAccountName Jen –name Jen

   Add-ADGroupMember –identity CorpAdmins –Members Jen

   $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   Set-ADAccountPassword –identity Jen –NewPassword $jp

   Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
   ```

### <a name="configure-auditing"></a>Configurar a auditoria

Você precisa habilitar a auditoria nas florestas existentes para estabelecer a configuração do PAM nessas florestas.  

Para cada domínio, entre em um controlador de domínio como administrador de domínio e realize as seguintes etapas:

1. Vá para **Iniciar** > **Ferramentas Administrativas** (ou no Windows Server 2016, **Ferramentas Administrativas do Windows**) e inicie o **Gerenciamento de Política de Grupo**.

2. Navegue até a política de controladores de domínio deste domínio.  Se você criou um novo domínio para contoso.local, navegue até **Floresta: contoso.local** > **Domínios** > **contoso.local** > **Controladores de Domínio** > **Política de Controladores de Domínio Padrão**. É exibida uma mensagem informativa.

3. Clique com o botão direito do mouse em **Política de Controladores de Domínio Padrão** e selecione **Editar**. Uma nova janela é exibida.

4. Na janela do Editor de Gerenciamento de Política de Grupo, na árvore Política de Controladores de Domínio Padrão, navegue até **Configuração do Computador** > **Políticas** > **Configurações do Windows** > **Configurações de Segurança** > **Políticas Locais** > **Política de Auditoria**.

5. No painel de detalhes, clique com o botão direito do mouse em **Gerenciamento de conta de auditoria** e selecione **Propriedades** no menu. Selecione **Definir estas configurações de política**, marque a caixa de seleção **Êxito**, marque a caixa de seleção **Falha**, clique em **Aplicar** e em **OK**.

6. No painel de detalhes, clique com o botão direito do mouse em **Acesso ao serviço de diretório de auditoria** e selecione **Propriedades**. Selecione **Definir estas configurações de política**, marque a caixa de seleção **Êxito**, marque a caixa de seleção **Falha**, clique em **Aplicar** e em **OK**.

7. Feche a janela do Editor de Gerenciamento de Política de Grupo e a janela Gerenciamento de Política de Grupo.

8. Aplique as configurações de auditoria abrindo uma janela do PowerShell e digitando:

   ```cmd
   gpupdate /force /target:computer
   ```

A mensagem **A atualização da Política de Computador foi concluída com êxito** deverá aparecer após alguns minutos.

### <a name="configure-registry-settings"></a>Definir as configurações do Registro

Nesta seção, você definirá as configurações do Registro necessárias para a migração do Histórico do SID, que serão usadas para a criação do grupo Privileged Access Management.

1. Inicie o PowerShell.

2. Digite os seguintes comandos para configurar o domínio de origem, a fim de permitir o acesso de RPC (chamada de procedimento remoto) para o banco de dados do SAM (gerente de contas de segurança).

   ```PowerShell
   New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

   Restart-Computer
   ```

Isso reiniciará o controlador de domínio, CORPDC. Para obter mais informações sobre esta configuração do Registro, veja [Como solucionar problemas de migração de sIDHistory entre florestas ADMTv2](http://support.microsoft.com/kb/322970).

## <a name="prepare-a-corp-workstation-and-resource"></a>Preparar uma estação de trabalho e um recurso CORP

Se você ainda não tiver um computador de estação de trabalho ingressado no domínio, siga estas instruções para preparar um.  

> [!NOTE]
> Se você já tiver uma estação de trabalho ingressada no domínio, vá para [Criar um recurso para fins de demonstração](#create-a-resource-for-demonstration-purposes).

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Instalar o Windows 8.1 ou Windows 10 Enterprise como uma VM

Em outra máquina virtual nova sem software instalado, instale o Windows 8.1 Enterprise ou Windows 10 Enterprise para tornar um computador *CORPWKSTN*.

1. Use as configurações Express durante a instalação.

2. Observe que a instalação pode não ser capaz de se conectar à Internet. Selecione **Criar uma conta local**. Especifique um nome de usuário diferente; não use "Administrador" nem "Jen".

3. Usando o Painel de Controle, dê a esse computador um endereço IP estático na rede virtual e defina o servidor DNS preferencial da interface para que seja o mesmo que o do servidor de CORPDC.

4. Usando o Painel de Controle, ingresse no domínio o computador de CORPWKSTN ao domínio contoso.local. Você precisará fornecer credenciais de administrador do domínio Contoso. Quando terminar, reinicie o computador de CORPWKSTN.

### <a name="create-a-resource-for-demonstration-purposes"></a>Criar um recurso para fins de demonstração

Você precisará de um recurso para demonstrar o controle de acesso baseado em grupo de segurança com o PAM.  Se você ainda não tiver um recurso, poderá usar uma pasta de arquivos para fins de demonstração.  Isso fará o uso de objetos do AD “Julia” e “CorpAdmins” criados no domínio contoso.local.

1. Conecte-se à estação de trabalho CORPWKSTN. Clique no ícone **Trocar usuário** e em **Outro usuário**. Garanta que o usuário CONTOSO\\Julia possa fazer logon em CORPWKSTN.

2. Crie uma nova pasta chamada *CorpFS* e compartilhe-a com o grupo *CorpAdmins*.

3. Abra o PowerShell como administrador.

4. Digite os seguintes comandos.

   ```PowerShell
   mkdir c:\corpfs

   New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

   $acl = Get-Acl c:\corpfs

   $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

   $acl.SetAccessRule($car)

   Set-Acl c:\corpfs $acl
   ```

Na próxima etapa, você vai preparar o controlador de domínio PRIV.

> [!div class="step-by-step"]
> [Etapa 2 »](step-2-prepare-priv-domain-controller.md)
