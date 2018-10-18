---
title: Implantando o Microsoft Identity Manager Certificate Manager | Microsoft Docs
description: Instalar o Microsoft Identity Manager 2016 Certificate Manager
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 01c5c8357c8cb0424bd38b61836919f5c2c3e96a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358866"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implantando o MIM CM (Microsoft Identity Manager Certificate Manager) 2016

A instalação do MIM CM (Microsoft Identity Manager Certificate Manager) 2016 envolve várias etapas. Como uma maneira de simplificar o processo, vamos dividi-lo em partes. Há etapas preliminares que devem ser executadas antes de executar as etapas reais do MIM CM. Sem o trabalho preliminar, a instalação provavelmente falhará.

O diagrama a seguir mostra um exemplo do tipo de ambiente que pode ser usado. Os sistemas com números são incluídos na lista abaixo do diagrama e são necessários para concluir com êxito as etapas abordadas neste artigo. Por fim, os servidores Windows 2016 Datacenter são usados:

![Diagrama do ambiente](media/mim-cm-deploy/image001.png)

1. CORPDC – controlador de domínio
2. CORPCM – servidor do MIM CM
3. CORPCA – autoridade de certificação
4. CORPCMR – API REST do MIM CM na Web – Portal do CM para API REST – uso posterior
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – ingressado no domínio do Windows 10

## <a name="deployment-overview"></a>Visão geral da implantação

- Instalação do sistema operacional base

    O laboratório consiste em servidores Windows 2016 Datacenter.

    >[!NOTE]
    >Para obter mais detalhes sobre as plataformas com suporte para o MIM 2016 examine o artigo intitulado [Supported platforms for MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md) (Plataformas com suporte para o MIM 2016)

1. Etapas de pré-implantação

    - [Estender o esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Criar contas de serviço

    - [Criar modelos de certificado](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configurando o Kerberos

    - Etapas relacionadas ao banco de dados

        - Requisitos de configuração de SQL

        - Permissões de banco de dados

2. Implantação

## <a name="pre-deployment-steps"></a>Etapas de pré-implantação

O assistente de configuração do MIM CM requer que sejam fornecidas informações ao longo do processo para que ele seja concluído com êxito.

![diagrama](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Estender o esquema

O processo para estender o esquema é simples, mas deve ser abordado com cuidado devido à sua natureza irreversível.

>[!NOTE]
>Esta etapa requer que a conta usada tenha direitos de administrador de esquema.

1. Navegue até o local da mídia do MIM e navegue até a pasta \\Certificate Management\\x64.

2. Copie a pasta de esquema para CORPDC e, em seguida, navegue até ela.

    ![diagrama](media/mim-cm-deploy/image005.png)

3. Execute o cenário de floresta única do script resourceForestModifySchema.vbs. Para o cenário de floresta de recursos, execute os scripts:
   - DomainA – usuários localizados (userForestModifySchema.vbs)
   - ResourceForestB – local da instalação do CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >As alterações de esquema são uma operação unidirecional e exigem uma recuperação da floresta para serem revertidas, portanto, verifique se você tem os backups necessários. Para obter detalhes sobre as alterações feitas no esquema ao realizar esta operação examine o artigo [Forefront Identity Manager 2010 Certificate Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx) (Alterações de esquema do Forefront Identity Manager 2010 Certificate Management)

     ![diagrama](media/mim-cm-deploy/image007.png)

4. Execute o script e você deverá receber uma mensagem de êxito quando ele for concluído.

    ![Mensagem de êxito](media/mim-cm-deploy/image009.png)

Agora, o esquema no AD foi estendido para dar suporte ao MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Criando grupos e contas de serviço

A tabela a seguir resume as contas e permissões exigidas pelo MIM CM. Você pode permitir que o MIM CM crie as seguintes contas automaticamente ou criá-las antes da instalação. Os nomes reais das contas podem ser alterados. Se você mesmo criar as contas, tente nomeá-las de uma forma fácil de corresponder o nome da conta de usuário à sua função.

Usuários:

![Diagrama](media/mim-cm-deploy/image010.png)

![Diagrama](media/mim-cm-deploy/image012.png)

| **Função**                   | **Nome de logon do usuário** |
|----------------------------|---------------------|
| Agente do MIM CM               | MIMCMAgent          |
| Agente de recuperação de chave do MIM CM  | MIMCMKRAgent        |
| Agente de autorização do MIM CM | MIMCMAuthAgent      |
| Agente do gerenciador de AC do MIM CM    | MIMCMManagerAgent   |
| Agente de pool da Web do MIM CM      | MIMCMWebAgent       |
| Agente de Registro do MIM CM    | MIMCMEnrollAgent    |
| Serviço de atualização do MIM CM      | MIMCMService        |
| Conta de instalação do MIM        | MIMINSTALL          |
| Agente de suporte técnico            | CMHelpdesk1-2       |
| Gerenciador do CM                 | CMManager1-2        |
| Usuário assinante            | CMUser1-2           |

Groups:

| **Função**               | **Grupo**         |
|------------------------|-------------------|
| Membros de suporte técnico do CM    | MIMCM-Helpdesk    |
| Membros do gerenciador do CM     | MIMCM-Managers    |
| Membros assinantes do CM | MIMCM-Subscribers |

PowerShell: contas de agente:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Atualizar a política local do servidor **CORPCM** para contas de agente 

| **Nome de logon do usuário** | **Descrição e permissões**   |
|------|---------------------|
| MIMCMAgent          | Fornece os seguintes serviços: </br>– Recupera chaves privadas criptografadas da AC. </br>– Protege as informações de PIN do cartão inteligente no banco de dados do FIM CM. </br>– Protege a comunicação entre o FIM CM e a AC. </br></br> Esta conta de usuário exige as seguintes configurações de controle de acesso:</br>-   Direito de usuário **Permitir logon localmente**.</br>-   Direito de usuário **Emitir e gerenciar certificados**. </br>– Permissão de leitura e gravação na pasta Temp do sistema no seguinte local: %WINDIR%\\Temp.</br>– Um certificado de criptografia e de assinatura digital emitido e instalado no repositório do usuário.
|MIMCMKRAgent        | Recupera as chaves privadas arquivadas da AC. Esta conta de usuário exige as seguintes configurações de controle de acesso:</br> -   Direito de usuário **Permitir logon localmente**.</br>– Associação ao grupo **Administradores** local. </br>– Permissão de inscrição no modelo de certificado **KeyRecoveryAgent**. </br>– O certificado do agente de recuperação de chave é emitido e instalado no repositório do usuário. O certificado deve ser adicionado à lista de agentes de recuperação de chave na AC. </br>– Permissão de leitura e de gravação na pasta Temp do sistema no seguinte local: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina os direitos de usuário e as permissões para usuários e grupos. Esta conta de usuário exige as seguintes configurações de controle de acesso: </br>– Associação ao grupo de domínio de acesso de compatível com as versões anteriores ao Windows 2000. </br> – Concessão do direito de usuário **Gerar auditorias de segurança**.             |
| MIMCMManagerAgent   | Executa as atividades de gerenciamento da AC. </br> Esse usuário deve receber a permissão Gerenciar AC.        |
| MIMCMWebAgent       | Fornece a identidade para o pool de aplicativos do IIS. O FIM CM é executado em um processo da interface de programação de aplicativo Microsoft Win32® que usa as credenciais do usuário. </br> Esta conta de usuário exige as seguintes configurações de controle de acesso:</br> – Associação ao grupo **IIS_WPG, windows 2016 = IIS_IUSRS** local. </br>– Associação ao grupo **Administradores** local.</br>– Concessão do direito de usuário **Gerar auditorias de segurança**. </br>– Concessão do direito de usuário **Agir como parte do sistema operacional**. </br>– Concessão do direito de usuário **Substituir token no nível do processo**.</br>- Atribuição como a identidade do pool de aplicativos do IIS, **CLMAppPool**. </br>– Concessão da permissão de leitura na chave do Registro **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser**. </br>– Essa conta também deve ser confiável para delegação.|
| MIMCMEnrollAgent    | Realiza o registro em nome do usuário. Esta conta de usuário exige as seguintes configurações de controle de acesso:</br>– Um certificado do Agente de Registro que é emitido e instalado no repositório do usuário.</br>-   Direito de usuário **Permitir logon localmente**. </br>– Permissão no modelo de certificado do **Agente de Registro** (ou no modelo personalizado, se algum for usado).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Criando modelos de certificado para contas de serviço do MIM CM

Três das contas de serviço usadas pelo MIM CM exigem um certificado e o assistente de configuração requer que você forneça o nome dos modelos de certificados que ele deverá usar para solicitar certificados para elas.

As contas de serviço que exigem certificados são:

- MIMCMAgent: essa conta precisa de um certificado de usuário

- MIMCMEnrollAgent: essa conta precisa de um certificado de agente de registro

- MIMCMKRAgent: essa conta precisa de um certificado do **Agente de recuperação de chave**

Há modelos que já estão presentes no AD, mas precisamos criar as nossas próprias versões para trabalhar com o MIM CM. O motivo é que precisamos fazer modificações dos modelos de linha de base originais.

As três contas acima terão direitos elevados na organização e deverão ser tratadas com cuidado.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Criar o modelo de certificado de autenticação do MIM CM

1. Em **Ferramentas Administrativas**, abra **Autoridade de Certificação**.

2. No console **Autoridade de Certificação**, na árvore de console, expanda **Contoso-CorpCA** e clique em **Modelos de Certificado**.

3. Clique com o botão direito do mouse em **Modelos de Certificado**e clique em **Gerenciar**.

4. No **Console de Modelos de Certificado**, no painel **Detalhes**, selecione **Usuário**, clique com botão direito do mouse e, em seguida, clique em **Duplicar Modelo**.

5. Na caixa de diálogo **Duplicar Modelo**, selecione **Windows Server 2003 Enterprise** e, em seguida, clique em **OK**.

    ![Mostrar alterações resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >O MIM CM não funciona com certificados baseados nos modelos de certificado da versão 3. Você precisa criar um modelo de certificado do Windows Server® 2003 Enterprise (versão 2). Confira [Detalhes da V3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) para obter mais informações.

6. Na caixa de diálogo **Propriedades do Novo Modelo**, na guia **Geral**, na caixa **Nome de Exibição do Modelo**, digite **Assinatura do MIM CM**. Altere o **Período de Validade** para **2 anos** e, em seguida, desmarque a caixa de seleção **Publicar certificado no Active Directory**.

7. Na guia **Tratamento de Solicitação**, verifique se a caixa de seleção **Permitir que a chave privada seja exportada** está selecionada e, em seguida, clique na **guia Criptografia**.

8. Na caixa de diálogo **Seleção de Criptografia**, desabilite **Microsoft Enhanced Cryptographic Provider v1.0**, habilite **Microsoft Enhanced Cryptographic Provider RSA e AES** e, em seguida, clique em **OK**.

9. Na guia **Nome da Entidade**, desmarque as caixas de seleção **Incluir nome de email no nome da entidade** e **Nome de email**.

10. Na guia **Extensões**, na lista **Extensões incluídas neste modelo** lista, verifique se **Políticas de Aplicativo** está selecionado e, em seguida, clique em **Editar**.

11. Na caixa de diálogo **Editar extensão de Políticas de Aplicativo**, selecione as políticas de aplicativo **Encrypting File System** e **Proteger Email**. Clique em **Remover**e depois em **OK**.

12. Na guia **Segurança**, execute as seguintes etapas:

    - Remova o **Administrador**.

    - Remova os **Admins do Domínio**.

    - Remova os **Usuários do Domínio**.

    - Atribua somente as permissões **Leitura** e **Gravação** para **Admins Corporativos**.

    - Adicione o **MIMCMAgent.**

    - Atribua as permissões de **Leitura** e **Inscrição** ao **MIMCMAgent**.

13. Na caixa de diálogo **Propriedades do Novo Modelo**, clique em **OK**.

14. Deixe o **Console de Modelos de Certificado** aberto.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Criar o modelo de certificado do agente de registro do MIM CM

1. No **Console de Modelos de Certificado**, no painel **Detalhes**, selecione **Agente de Registro**, clique com botão direito do mouse e, em seguida, clique em **Duplicar Modelo**.

2. Na caixa de diálogo **Duplicar Modelo**, selecione **Windows Server 2003 Enterprise** e, em seguida, clique em **OK**.

3. Na caixa de diálogo **Propriedades do Novo Modelo**, na guia **Geral**, na caixa **Nome de exibição do modelo**, digite **Agente de Registro do MIM CM**. Verifique se o **Período de Validade** é **2 anos**.

4. Na guia **Tratamento de Solicitação**, habilite **Permitir que a chave privada seja exportada** e, em seguida, clique na **Guia CSPs ou Criptografia.**

5. Na caixa de diálogo **Seleção de CSP**, desabilite **Microsoft Base Cryptographic Provider v1.0**, desabilite **Microsoft Enhanced Cryptographic Provider v1.0** habilite **Microsoft Enhanced Cryptographic Provider RSA e AES** e, em seguida, clique em **OK**.

6. Na guia **Segurança**, execute as o seguinte:

    - Remova o **Administrador**.

    - Remova os **Admins do Domínio**.

    - Atribua somente as permissões **Leitura** e **Gravação** para **Admins Corporativos**.

    - Adicione o **MIMCMEnrollAgent**.

    - Atribua as permissões de **Leitura** e **Inscrição** ao **MIMCMEnrollAgent**.

7. Na caixa de diálogo **Propriedades do Novo Modelo**, clique em **OK**.

8. Deixe o **Console de Modelos de Certificado** aberto.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Criar o modelo de certificado do agente de recuperação de chave do MIM CM

1. No console **Modelos de Certificado**, no painel **Detalhes**, selecione **Agente de Recuperação de Chave**, clique com botão direito do mouse e, em seguida, clique em **Duplicar Modelo**.

2. Na caixa de diálogo **Duplicar Modelo**, selecione **Windows Server 2003 Enterprise** e, em seguida, clique em **OK**.

3. Na caixa de diálogo **Propriedades do Novo Modelo**, na guia **Geral**, na caixa **Nome de exibição do modelo**, digite **Agente de Recuperação de Chave do MIM CM**. Verifique se o **Período de Validade** é **2 anos** na **Guia Criptografia.**

4. Na caixa de diálogo **Seleção de Provedores**, desabilite **Microsoft Enhanced Cryptographic Provider v1.0**, habilite **Microsoft Enhanced Cryptographic Provider RSA e AES** e, em seguida, clique em **OK**.

5. Na guia **Requisitos de Emissão**, verifique se a opção **Aprovação do gerenciador de certificados de autoridade de certificação** está **desabilitada**.

6. Na guia **Segurança**, execute as o seguinte:

    - Remova o **Administrador**.

    - Remova os **Admins do Domínio**.

    - Atribua somente as permissões **Leitura** e **Gravação** para **Admins Corporativos**.

    - Adicione o **MIMCMKRAgent**.

    - Atribua as permissões **Leitura** e **Inscrição** ao **KRAgent**.

7. Na caixa de diálogo **Propriedades do Novo Modelo**, clique em **OK**.

8. Feche o **Console de Modelos de Certificado**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicar os modelos de certificado necessários na Autoridade de Certificação

1. Restaure o console **Autoridade de Certificação**.

2. No console **Autoridade de Certificação**, na árvore de console, clique com o botão direito do mouse em **Modelos de Certificado**, aponte para **Novo** e clique em **Modelo de Certificado a Ser Emitido**.

3. Na caixa de diálogo **Habilitar Modelos de Certificado**, selecione **Agente de Registro do MIM CM**, **Agente de Recuperação de Chave do MIM CM** e **Assinatura do MIM CM**. Clique em **OK**.

4. Na árvore de console, clique em **Modelos de Certificado**.

5. Verifique se os três modelos novos aparecem no painel **Detalhes** e, em seguida, feche **Autoridade de Certificação**.

    ![Assinatura do MIM CM](media/mim-cm-deploy/image016.png)

6. Feche todas as janelas abertas e faça logoff.

### <a name="iis-configuration"></a>Configuração do IIS

Para hospedar o site para o CM, instale e configure o IIS.

#### <a name="install-and-configure-iis"></a>Instalar e configurar o IIS

1. Faça logon no **CORLog** com a conta **MIMINSTALL**

    >[!IMPORTANT]
    >A conta de instalação do MIM deve ser um administrador local

2. Abra o PowerShell e execute o seguinte comando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Um site denominado Site Padrão é instalado por padrão com o IIS 7. Se esse site for renomeado ou removido, um site com o nome Site Padrão deverá estar disponível para que o MIM CM possa ser instalado.

#### <a name="configuring-kerberos"></a>Configurando o Kerberos

A conta MIMCMWebAgent executará o portal do MIM CM. Por padrão a autenticação no modo kernel é usada no IIS. Nesse caso, você vai desabilitar a autenticação no modo kernel do Kerberos e configurar os SPNs na conta do MIMCMWebAgent. Algum comando exigirá permissão elevada no Active Directory e no servidor CORPCM.

![Diagrama](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Atualizando o IIS no CORPCM**

![diagrama](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Você precisará adicionar um Registro A do DNS para o "cm.contoso.com" e apontar para o IP do CORPCM

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Exigindo SSL no portal do MIM CM

É altamente recomendável exigir SSL no portal do MIM CM. Se você não exigir, o assistente o lembrará de fazer isso.

1. Inscreva o certificado da Web para a atribuição de **cm.contoso.com** ao site padrão

2. Abra **Gerenciador do IIS** e navegue até **Certificate Management**

3. No modo de exibição Recursos, clique duas vezes em Configurações de SSL.

4. Na página Configurações de SSL, selecione **Exigir SSL**.

5. No painel Ações, clique em **Aplicar.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configuração do banco de dados **CORPSQL** para o MIM CM

1. Verifique se você está conectado ao servidor CORPSQL01.

2. Verifique se você fez logon como um DBA do SQL.

3. Execute o seguinte script T-SQL para permitir que a conta CONTOSO\\MIMINSTALL crie o banco de dados quando passarmos para a etapa de configuração

    >[!NOTE]
    >Será necessário voltar ao SQL quando você estiver pronto para o módulo de política e de saída

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Mensagem de erro do assistente de configuração do MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implantação do Microsoft Identity Manager 2016 Certificate Management

1. Verifique se você está conectado ao servidor CORPCM e se a conta **MIMINSTALL** é membro de grupo de **administradores locais**.

2. Verifique se você efetuou logon como Contoso\\MIMINSTALL.

3. Monte o ISO do Microsoft Identity Manager SP1.

4. **Abra** o diretório **Certificate Management\\x64**.

5. Na janela **x64**, clique com botão direito do mouse em **Instalação** e, em seguida, clique em **Executar como administrador**.

6. Na página Bem-vindo ao Assistente de Configuração do Microsoft Identity Manager Certificate Management, clique em **Avançar.**

7. Na página Termos de Licença, leia o contrato, habilite a **caixa de seleção** Aceito os termos do contrato de licença e, em seguida, clique em Avançar.

8. Na página Instalação Personalizada, verifique se o **Portal do MIM CM** e os **Componentes do Serviço de Atualização do MIM CM** estão marcados para serem instalados e, em seguida, **clique em Avançar**.

9. Na página Pasta da Web Virtual, verifique se o nome da pasta Virtual é **CertificateManagement** e, em seguida, **clique em Avançar**.

10. Na página Instalar o Microsoft Identity Manager Certificate Management, **clique em instalar**.

11. Na página **O Assistente de Instalação do Microsoft Identity Manager Certificate Management foi concluído**, **clique em Concluir**.

![Assistente do MIM CM concluído](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Assistente de Configuração do Microsoft Identity Manager 2016 Certificate Management

Antes de fazer logon no CORPCM, adicione MIMINSTALL ao grupo **Admins de domínio, admins de esquema e administradores locais** do assistente de configuração. Essa opção poderá ser removida após a conclusão da configuração.

![Mensagem de erro](media/mim-cm-deploy/image028.png)

1. No menu **Iniciar**, clique em **Assistente de Configuração do Certificate Management**. E execute como **Administrador**

2. Na página **Bem-vindo ao Assistente de Configuração**, clique em **Avançar**.

3. Na página **Configuração de AC**, verifique se a AC selecionada é **Contoso-CORPCA-CA**, verifique se o servidor selecionado é **CORPCA.CONTOSO.COM** e, em seguida, clique em **Avançar**.

4. Na página **Configurar o Banco de Dados do Microsoft® SQL Server®**, no **Nome do SQL Server**, digite **CORPSQL1**, habilite a caixa de seleção **Usar minhas credenciais para criar o banco de dados** e, em seguida, clique em **Avançar**.

5. Na página **Configurações de Banco de Dados**, aceite o nome do banco de dados padrão **FIMCertificateManagement**, verifique se a **Autenticação integrada do SQL** está selecionada e, em seguida, clique em **Avançar**.

6. Na página **Configurado o Active Directory**, aceite o nome padrão fornecido para o ponto de conexão de serviço e, em seguida, clique em **Avançar**.

7. na página **Método de autenticação** confirme se a **Autenticação integrada do Windows** está selecionada e clique em **Avançar**.

8. Na página **Agentes – FIM CM**, desmarque a caixa de seleção **Usar as configurações padrão do FIM CM** e, em seguida, clique em **Contas Personalizadas**.

9. Na caixa de diálogo **Agentes – FIM CM** com várias guias, em cada guia, digite as seguintes informações:

   - Nome de usuário: **Atualizar**

   - Senha: **Pass\@word1**

   - Confirmar senha: **Pass\@word1**

   - Usar um usuário existente: **Habilitado**

     >[!NOTE]
     >Nós já criamos essas contas anteriormente. Repita os procedimentos da etapa 8 para as seis guias de conta de agente.

     ![Contas do MIM CM](media/mim-cm-deploy/image030.png)

10. Quando todas as informações de conta de agente forem concluídas, clique em **OK**.

11. Na página **Agentes – MIM CM**, clique em **Avançar**.

12. Na página **Configurar certificados de servidor**, habilite os seguintes modelos de certificado:

    - Modelo de certificado a ser usado para o certificado do Agente de Recuperação de Chave do agente de recuperação: **MIMCMKeyRecoveryAgent**.

    - Modelo de certificado a ser usado para o certificado do agente do FIM CM: **MIMCMSigning**.

    - Modelo de certificado a ser usado para o certificado do agente de registro: **FIMCMEnrollmentAgent**.

13. Na página **Configurar certificados de servidor**, clique em **Avançar**.

14. Na página **Instalar Servidor de Email, Impressão de Documentos**, preencha a caixa **Especificar o nome do servidor SMTP que deseja usar para as notificações do registro de email** e clique em **Avançar.**

15. Na página **Pronto para configurar**, clique em **Configurar**.

16. Na caixa de diálogo de aviso **Assistente de Configuração – Microsoft Forefront Identity Manager 2010 R2**, clique em **OK** para confirmar que o SSL não está habilitado no diretório virtual do IIS.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Não clique no botão Concluir até que a execução do assistente de configuração seja concluída. O registro em log do assistente pode ser encontrado aqui: **%programfiles%\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log**

17. Clique em **Finalizar**.

    ![Assistente do MIM CM concluído](media/mim-cm-deploy/image033.png)

18. Feche todas as janelas abertas.

19. Adicione https://cm.contoso.com/certificatemanagement à zona da intranet local no seu navegador.

20. Visitar o site do servidor CORPCM https://cm.contoso.com/certificatemanagement  

    ![diagrama](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verificar o serviço de isolamento de chave CNG

1. Em **Ferramentas Administrativas**, abra **Serviços**.

2. No painel **Detalhes**, clique duas vezes em **Isolamento de Chave CNG**.

3. Na guia **Geral**, altere o **Tipo de Inicialização** para **Automático**.

4. Na guia **Geral**, inicie o serviço se ele não estiver no estado iniciado.

5. Na guia **Geral**, clique em **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalando e configurando os módulos de AC:

Nesta etapa, vamos instalar e configurar os módulos de AC do FIM CM na autoridade de certificação.

1. Configurar o FIM CM para inspecionar apenas permissões de usuário para operações de gerenciamento

2. Na janela **C:\\Arquivos de Programas\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web**, faça uma cópia de **web.config** e nomeie a cópia como **web.1.config**.

3. Na janela **Web**, clique com botão direito do mouse em **Web.config** e, em seguida, clique em **Abrir**.

    >[!Note]
    >O arquivo Web.config é aberto no bloco de notas

4. Quando o arquivo abrir, pressione CTRL + F.

5. Na caixa de diálogo **Localizar e Substituir**, na caixa **Localizar**, digite **UseUser** e, em seguida, clique em **Localizar Próxima** três vezes.

6. Feche a caixa de diálogo **Localizar e Substituir**.

7. Você deverá estar na linha **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>**. Altere a linha para o seguinte: **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>**.

8. Feche o arquivo, salvando todas as alterações.

9. Criar uma conta para o computador da AC no SQL Server \<sem script\>

10. Verifique se você está conectado ao servidor **CORPSQL01**.

11. Verifique se você efetuou logon como um **DBA**

12. No menu **Iniciar**, inicie o **SQL Server Management Studio**.

13. Na caixa de diálogo **Conectar ao servidor**, na caixa **Nome do servidor**, digite **CORPSQL01** e clique em **Conectar**.

14. Na árvore de console, expanda **Segurança** e, em seguida, clique em **Logons**.

15. Clique com o botão direito do mouse em **Logons**e clique em **Novo Logon**.

16. Na página **Geral**, na caixa **Nome de logon**, digite **contoso\\CORPCA\$**. Selecione **Autenticação do Windows**. O banco de dados padrão é o **FIMCertificateManagement**.

17. No painel esquerdo, selecione **Mapeamento de Usuário**. No painel direito, clique na caixa de seleção na coluna **Mapa** ao lado de **FIMCertificateManagement**. Na lista **Associação de função de banco de dados para: FIMCertificateManagement**, habilite a função **clmApp**.

18. Clique em **OK**.

19. Saia do **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar os módulos de AC do FIM CM na Autoridade de Certificação

1. Verifique se você está conectado ao servidor **CORPCA**.

2. Na janela **X64**, clique com botão direito do mouse em **Setup.exe** e, em seguida, clique em **Executar como administrador**.

3. Na página **Bem-vindo ao Assistente de Configuração do Microsoft Identity Manager Certificate Management**, clique em **Avançar**.

4. Na página **Termos de Licença**, leia o contrato. Marque a caixa de seleção **Aceito os termos do Contrato de Licença** e clique em **Avançar**.

5. Na página **Instalação Personalizada**, selecione **Portal do MIM CM** e, em seguida, clique em **Esse recurso não estará disponível**.

6. Na página **Instalação Personalizada**, selecione **Serviço de Atualização do MIM CM** e, em seguida, clique em **Esse recurso não estará disponível**.

    >[!Note]
    >Isso deixará os arquivos de AC do MIM CM como o único recurso habilitado para a instalação.

7. Na página **Instalação Personalizada**, clique em **Avançar**.

8. Na página **Instalar o Microsoft Identity Manager Certificate Management**, clique em **Instalar**.

9. Na página **O Assistente de Instalação do Microsoft Identity Manager Certificate Management foi concluído**, clique em **Concluir**.

10. Feche todas as janelas abertas.

### <a name="configure-the-mim-cm-exit-module"></a>Configurar o módulo de saída do MIM CM

1. Em **Ferramentas Administrativas**, abra **Autoridade de Certificação**.

2. Na árvore de console, clique com botão direito do mouse em **contoso-CORPCA-CA** e, em seguida, em **Propriedades**.

3. Na guia **Módulo de Saída**, selecione **Módulo de Saída do FIM CM** e, em seguida, clique em **Propriedades**.

4. Na caixa **Especificar a cadeia de conexão de banco de dados do CM**, digite **Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01**. Deixe a caixa de seleção **Criptografar a Cadeia de Conexão** habilitada e, em seguida, clique em **OK**.
5. Na caixa de mensagem **Microsoft FIM Certificate Management**, clique em **OK**.

6. Na caixa de diálogo **Propriedades de contoso-CORPCA-CA**, clique em **OK**.

7. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **Todas as Tarefas** e clique em **Interromper Serviço**. Aguarde até que os Serviços de Certificados do Active Directory sejam interrompidos.

8. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **Todas as Tarefas** e clique em **Iniciar Serviço**.

9. Minimize o console **Autoridade de Certificação**.

10. Em **Ferramentas Administrativas**, abra **Visualizador de Eventos**.

11. Na árvore de console, expanda **Logs de Aplicativos e de Serviços** e, em seguida, clique em **FIM Certificate Management**.

12. Na lista de eventos, verifique se os eventos mais recentes *não* incluem nenhum evento de **Aviso** ou **Erro** desde a última reinicialização dos Serviços de Certificados do Active Directory.

    >[!NOTE] 
    >O último evento deverá indicar que o Módulo de Saída foi carregado usando as configurações de: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimize o **Visualizador de Eventos**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copiar a impressão digital do certificado MIMCMAgent para a área de transferência do Windows®

1. Restaure o console **Autoridade de Certificação**.

2. Na árvore de console, expanda **contoso-CORPCA-CA** e, em seguida, clique em **Certificados Emitidos**.

3. No painel **Detalhes**, clique duas vezes no certificado com **CONTOSO\\MIMCMAgent** na coluna **Nome do Solicitante** e com **Assinatura do FIM CM** na coluna **Modelo de Certificado**.

4. Na guia **Detalhes** , selecione o campo **Impressão Digital** .

5. Selecione a impressão digital e, em seguida, pressione CTRL + C.

    >[!NOTE]
    >**Não** inclua o espaço à esquerda na lista de caracteres de impressão digital.

6. Na caixa de diálogo **Certificado**, clique em **OK**.

7. No menu **Iniciar**, na caixa **Pesquisar programas e arquivos**, digite **Bloco de notas** e pressione Enter.

8. No **Bloco de notas**, no menu **Editar**, clique em **Colar**.

9. No menu **Editar**, clique em **Substituir**.

10. Na caixa **Localizar**, digite um caractere de espaço e, em seguida, clique em **Substituir Tudo**.

    >[!Note]
    >Isso remove todos os espaços entre os caracteres na impressão digital.

11. Na caixa de diálogo **Substituir**, clique em **Cancelar**.

12. Selecione o *thumbprintstring* convertido e, em seguida, pressione CTRL + C.

13. Feche o **Bloco de notas** sem salvar as alterações.

### <a name="configure-the-fim-cm-policy-module"></a>Configurar o Módulo de Política do FIM CM

1. Restaure o console **Autoridade de Certificação**.

2. Clique com botão direito do mouse em **contoso-CORPCA-CA** e, em seguida, clique em **Propriedades**.

3. Na caixa de diálogo **Propriedades de contoso-CORPCA-CA** na guia **Módulo de Política**, clique em **Propriedades**.

    - Na guia **Geral**, verifique se **Passar solicitações que não sejam do FIM CM para o módulo de política padrão para processamento** está selecionado.

    - Na guia **Certificados de Assinatura**, clique em **Adicionar**.

    - Na caixa de diálogo Certificado, clique com botão direito do mouse na caixa **Especifique o hash do certificado codificado em hexa** e, em seguida, clique em **Colar**.

    - Na caixa de diálogo **Certificado**, clique em **OK**.

        >[!Note]
        >Se o botão **OK** não está habilitado, você incluiu acidentalmente um caractere oculto na cadeia de impressão digital quando copiou a impressão digital do certificado clmAgent. Repita todas as etapas a partir da **Tarefa 4: Copiar a impressão digital do certificado MIMCMAgent para área de transferência do Windows** neste exercício.

4. Na caixa de diálogo **Propriedades de Configuração**, verifique se a impressão digital é exibida na lista **Certificados de Autenticação Válidos** e, em seguida, clique em **OK**.

5. Na caixa de mensagem **FIM Certificate Management**, clique em **OK**.

6. Na caixa de diálogo **Propriedades de contoso-CORPCA-CA**, clique em **OK**.

7. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **Todas as Tarefas** e clique em **Interromper Serviço**.

8. Aguarde até que os Serviços de Certificados do Active Directory sejam interrompidos.

9. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **Todas as Tarefas** e clique em **Iniciar Serviço**.

10. Feche o console **Autoridade de Certificação**.

11. Feche todas as janelas abertas e, em seguida, faça logoff.

A **última etapa da implantação** é verificar se os CONTOSO\\MIMCM-Managers podem implantar e criar modelos e configurar o sistema sem ser administradores de esquema e de domínio. O próximo script será fornecer à ACL as permissões para os modelos de certificado usando dsacls. Execute com a conta que tem permissão total para alterar as permissões de segurança de Leitura e Gravação para cada modelo de certificado existente na floresta.

Primeiro passo: **Configurando permissões no ponto de conexão de serviço e no grupo de destino e delegando o gerenciamento de modelo de perfil**

1. Configure permissões no SCP (ponto de conexão de serviço).

2. Configure o gerenciamento de modelo de perfil delegado.

3. Configure permissões no SCP (ponto de conexão de serviço). **\<sem script\>**

4.   Verifique se você está conectado ao servidor virtual **CORPDC**.

5. Faça logon como **contoso\\corpadmin**

6. Em **Ferramentas Administrativas**, abra **Usuários e Computadores do Active Directory**.

7. Em **Usuários e Computadores do Active Directory**, no menu **Exibição**, verifique se **Recursos Avançados** está habilitado.

8. Na árvore de console, expanda **Contoso.com** \| **Sistema** \| **Microsoft** \| **Gerenciador de Ciclo de Vida de Certificado** e, em seguida, clique em **CORPCM**.

9. Clique com botão direito do mouse em **CORPCM** e, em seguida, clique em **Propriedades**.

10. Na caixa de diálogo **Propriedades de CORPCM** na guia **Segurança**, adicione os seguintes grupos com as permissões correspondentes:

    | Group          | Permissões      |
    |----------------|------------------|
    | mimcm-Managers | Ler </br> Auditoria do FIM CM</br> Agente de Registro do FIM CM</br> Solicitar Inscrição do FIM CM</br> Solicitar Recuperação do FIM CM</br> Solicitar Renovação do FIM CM</br> Solicitar Revogação do FIM CM </br> Solicitar Desbloqueio de Cartão Inteligente do FIM CM |
    | mimcm-HelpDesk | Ler</br> Agente de Registro do FIM CM</br> Solicitar Revogação do FIM CM</br> Solicitar Desbloqueio de Cartão Inteligente do FIM CM |

11. Na caixa de diálogo **Propriedades de CORPDC**, clique em **OK**.

12. Deixe **Usuários e Computadores do Active Directory** aberto.

**Configurar permissões nos objetos de usuário descendentes**

1. Verifique se você ainda está no console **Usuários e Computadores do Active Directory**.

2. Na árvore de console, clique com botão direito do mouse em **Contoso.com** e, em seguida, clique em **Propriedades**.

3. Na guia **Segurança**, clique em **Avançado**.

4. Na caixa de diálogo **Configurações de Segurança Avançadas para Contoso**, clique em **Adicionar**.

5. Na caixa de diálogo **Selecionar Usuário, Computador, Conta de Serviço ou Grupo**, na caixa **Inserir o nome do objeto a ser selecionado**, digite **mimcm-Managers** e, em seguida, clique em **OK**.

6. Na caixa de diálogo **Entrada de Permissão para Contoso**, na lista **Aplica-se a**, selecione **Objetos de Usuário Descendentes** e, em seguida, habilite a caixa de seleção **Permitir** para as seguintes **Permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Auditoria do FIM CM**

    - **Agente de Registro do FIM CM**

    - **Solicitar Inscrição do FIM CM**

    - **Solicitar Recuperação do FIM CM**

    - **Solicitar Renovação do FIM CM**

    - **Solicitar Revogação do FIM CM**

    - **Solicitar Desbloqueio de Cartão Inteligente do FIM CM**

7. Na caixa de diálogo **Entrada de Permissão para Contoso**, clique em **OK**.

8. Na caixa de diálogo **Configurações de Segurança Avançadas para Contoso**, clique em **Adicionar**.

9. Na caixa de diálogo **Selecionar Usuário, Computador, Conta de Serviço ou Grupo**, na caixa **Inserir o nome do objeto a ser selecionado**, digite **mimcm-HelpDesk** e, em seguida, clique em **OK**.

10. Na caixa de diálogo **Entrada de Permissão para Contoso**, na lista **Aplica-se a**, selecione **Objetos de Usuário Descendentes** e, em seguida, marque a caixa de seleção **Permitir** para as seguintes **Permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Agente de Registro do FIM CM**

    - **Solicitar Revogação do FIM CM**

    - **Solicitar Desbloqueio de Cartão Inteligente do FIM CM**

11. Na caixa de diálogo **Entrada de Permissão para Contoso**, clique em **OK**.

12. Na caixa de diálogo **Configurações de Segurança Avançadas para Contoso**, clique em **OK**.

13. Na caixa de diálogo **Propriedades de contoso.com**, clique em **OK**.

14. Deixe **Usuários e Computadores do Active Directory** aberto.

**Configurar permissões nos objetos de usuário descendentes \<sem script\>**

1. Verifique se você ainda está no console **Usuários e Computadores do Active Directory**.

2. Na árvore de console, clique com botão direito do mouse em **Contoso.com** e, em seguida, clique em **Propriedades**.

3. Na guia **Segurança**, clique em **Avançado**.

4. Na caixa de diálogo **Configurações de Segurança Avançadas para Contoso**, clique em **Adicionar**.

5. Na caixa de diálogo **Selecionar Usuário, Computador, Conta de Serviço ou Grupo**, na caixa **Inserir o nome do objeto a ser selecionado**, digite **mimcm-Managers** e, em seguida, clique em **OK**.

6. Na caixa de diálogo **Entrada de Permissão para CONTOSO**, na lista **Aplica-se a**, selecione **Objetos de Usuário Descendentes** e, em seguida, habilite a caixa de seleção **Permitir** para as seguintes **Permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Auditoria do FIM CM**

    - **Agente de Registro do FIM CM**

    - **Solicitar Inscrição do FIM CM**

    - **Solicitar Recuperação do FIM CM**

    - **Solicitar Renovação do FIM CM**

    - **Solicitar Revogação do FIM CM**

    - **Solicitar Desbloqueio de Cartão Inteligente do FIM CM**

7. Na caixa de diálogo **Entrada de Permissão para CONTOSO**, clique em **OK**.

8. Na caixa de diálogo **Configurações de Segurança Avançadas para CONTOSO**, clique em **Adicionar**.

9. Na caixa de diálogo **Selecionar Usuário, Computador, Conta de Serviço ou Grupo**, na caixa **Inserir o nome do objeto a ser selecionado**, digite **mimcm-HelpDesk** e, em seguida, clique em **OK**.

10. Na caixa de diálogo **Entrada de Permissão para CONTOSO**, na lista **Aplica-se a**, selecione **Objetos de Usuário Descendentes** e, em seguida, marque a caixa de seleção **Permitir** para as seguintes **Permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Agente de Registro do FIM CM**

    - **Solicitar Revogação do FIM CM**

    - **Solicitar Desbloqueio de Cartão Inteligente do FIM CM**

11. Na caixa de diálogo **Entrada de Permissão para contoso**, clique em **OK**.

12. Na caixa de diálogo **Configurações de Segurança Avançadas para Contoso**, clique em **OK**.

13. Na caixa de diálogo **Propriedades de contoso.com**, clique em **OK**.

14. Deixe **Usuários e Computadores do Active Directory** aberto.

Segundo passo: **Delegando permissões de gerenciamento de modelo de certificado \<script\>**

- Delegando permissões no contêiner Modelos de Certificado.

- Delegando permissões no contêiner OID.

- Delegando permissões nos modelos de certificado existentes.

Defina permissões no contêiner de modelos de certificado:

1. Restaure o console **Sites e Serviços do Active Directory**.

2. Na árvore de console, expanda **Serviços**, expanda **Serviços de Chave Pública** e, em seguida, clique em **Modelos de Certificado**.

3. Na árvore de console, clique com botão direito do mouse em **Modelos de Certificado** e, em seguida, clique em **Delegar Controle**.

4. No Assistente **Delegação de Controle**, clique em **Avançar**.

5. Na página **Usuários ou Grupos**, clique em **Adicionar**.

6. Na caixa de diálogo **Selecionar Usuários, Computadores ou Grupos**, na caixa **Inserir os nomes de objeto a serem selecionados**, digite **mimcm-Managers** e clique em **OK**.

7. Na página **Usuários ou Grupos**, clique em **Avançar**.

8. Na página **Tarefas a Serem Delegadas**, clique em **Criar uma tarefa personalizada a ser delegada** e, em seguida, clique em **Avançar**.

9.  Na página **Tipo de Objeto do Active Directory**, verifique se a opção **Esta pasta, os objetos existentes nesta pasta e a criação de novos objetos nesta pasta** está selecionada e, em seguida, clique em **Avançar**.

10. Na página **Permissões**, na lista **Permissões**, marque a caixa de seleção **Controle Total** e, em seguida, clique em **Avançar**.

11. Na página **Concluindo o Assistente para Delegação de Controle**, clique em **Concluir**.

Defina permissões no contêiner de OID:

1. Na árvore de console, clique com botão direito do mouse em **OID** e, em seguida, clique em **Propriedades**.

2. Na caixa de diálogo **Propriedades de OID**, na guia **Segurança**, clique em **Avançado**.

3. Na caixa de diálogo **Configurações de Segurança Avançadas para OID**, clique em **Adicionar**.

4. Na caixa de diálogo **Selecionar Usuário, Computador, Conta de Serviço ou Grupo**, na caixa **Inserir o nome do objeto a ser selecionado**, digite **mimcm-Managers** e, em seguida, clique em **OK**.

5. Na caixa de diálogo **Entrada de Permissões para OID**, verifique se as permissões se aplicam a **Este objeto e todos os objetos descendentes**, clique em **Controle Total** e, em seguida, clique em **OK**.

6. Na caixa de diálogo **Configurações de Segurança Avançadas para OID**, clique em **OK**.

7. Na caixa de diálogo **Propriedades de OID**, clique em **OK**.

8. Feche **Sites e Serviços do Active Directory**.

**Scripts: permissões no contêiner OID, Modelo de Perfil e Modelos de Certificado**

![diagrama](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Scripts: delegando permissões nos modelos de certificado existentes.**  

![diagrama](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
