---
title: "Como trabalhar com o portal de Redefinição de Senha de Autoatendimento | Microsoft Docs"
description: "Veja as novidades de redefinição de senha de autoatendimento no MIM 2016, incluindo como a SSPR funciona com autenticação multifator."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bbcfee70c71bc2f1a637327721d015434600c5e0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# Como trabalhar com a redefinição de senha de autoatendimento
<a id="working-with-self-service-password-reset" class="xliff"></a>
O Microsoft Identity Manager 2016 fornece funcionalidade adicional para o recurso de redefinição de senha do serviço de autoatendimento. Essa funcionalidade foi aprimorada com vários recursos importantes:

-   O portal de redefinição de senha de autoatendimento e a tela Logon do Windows agora permitem aos usuários desbloquear suas contas sem alterar suas senhas ou chamar os administradores de suportes. Normalmente, os usuários ficam bloqueados fora das suas contas por muitos motivos legítimos, como porque acidentalmente inseriram uma senha antiga, usam computadores bilíngues e estavam com o teclado definido para o idioma errado ou tentaram fazer logon em uma estação de trabalho compartilhada já aberta para a conta de outra pessoa.

-   Um novo portão de autenticação, Portão de Telefone, foi adicionado. Isso permite a autenticação do usuário por meio de chamada telefônica.

-   Foi adicionado suporte para o serviço Microsoft Azure Multi-Factor Authentication (MFA) do Microsoft Azure. Pode ser usado para o Portão de senha de uso único de SMS existente ou para a nova Porta de telefone.

## Azure para Multi-Factor Authentication
<a id="azure-for-multi-factor-authentication" class="xliff"></a>
O Microsoft Azure Multi-Factor Authentication é um serviço de autenticação que exige que os usuários verifiquem suas tentativas de entrada com um aplicativo móvel, chamada telefônica ou mensagem de texto. Ele está disponível para uso com o Active Directory do Microsoft Azure e como um serviço para aplicativos corporativos de nuvem e local.

O Azure MFA fornece um mecanismo de autenticação adicional que pode reforçar os processos de autenticação existentes, como aqueles realizados pelo MIM para assistência de logon de autoatendimento.

Ao usar o Azure MFA, os usuários são autenticados com o sistema para verificar sua identidade ao tentar recuperar o acesso à conta e aos recursos. A autenticação pode ser por meio de SMS ou chamada telefônica.   Quanto mais forte for a autenticação, maior será a confiança de que a pessoa que está tentando acessar é realmente o usuário que possui a identidade. Uma vez autenticado, o usuário pode escolher uma nova senha para substituir a antiga.

## Pré-requisitos para configurar o desbloqueio da conta de autoatendimento e a redefinição de senha usando o MFA
<a id="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa" class="xliff"></a>
Esta seção pressupõe que você tenha baixado e concluído a implantação do Microsoft Identity Manager 2016, incluindo os seguintes componentes e serviços:

-   Um Windows Server 2008 R2 ou posterior foi configurado como servidor do Active Directory, incluindo serviços de domínio do AD e o controlador de domínio com um domínio designado (um “domínio corporativo”)

-   Uma política de grupo é definida para o bloqueio de conta

-   O Serviço de Sincronização do MIM 2016 é instalado e executado em um servidor ingressado no domínio para o domínio do AD

-   O Serviço e o Portal do MIM 2016, incluindo o Portal de Registro do SSPR e o Portal de Redefinição do SSPR, são instalados e executados em um servidor (podem estar colocalizados com Sincronização)

-   A Sincronização do MIM deve ser configurada para sincronização de identidades do AD-MIM, incluindo:

    -   Configurando o ADMA (Agente de Gerenciamento do Active Directory) para conectividade com o AD DS e a capacidade para importar dados de identidade e exportá-los para o Active Directory.

    -   Configurando o MIM MA (Agente de Gerenciamento do MIM) para conectividade com o DB (Banco de Dados) de serviço do FIM (Forefront Identity Manager) e a capacidade para importar dados de identidade e exportá-los para o banco de dados do FIM.

    -   Configurando as regras de sincronização no Portal do MIM para permitir a sincronização de dados de usuário e facilitar a atividades baseadas em sincronização no Serviço do MIM.

-   Os Suplementos e as Extensões do MIM 2016, incluindo o cliente integrado de Logon do Windows do SSPR, são implantados no servidor ou em um computador cliente separado.

## Preparação do MIM para trabalhar com à autenticação multifator
<a id="prepare-mim-to-work-with-multi-factor-authentication" class="xliff"></a>
Configure a Sincronização do MIM para dar suporte para redefinição de senha e funcionalidade de desbloqueio de conta. Para obter mais informações, consulte [Instalação dos complementos e extensões FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalação do FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Portões de autenticação de SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) e [Guia de laboratório de teste de SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

Na próxima seção, você configurará o provedor do Azure MFA no Active Directory do Microsoft Azure. Como parte dessa ação, você gerará um arquivo que contém o material de autenticação que o MFA exige para poder entrar em contato com o Azure MFA.  Para continuar, você precisará de uma assinatura do Azure.

### Como registrar o provedor de autenticação multifator no Azure
<a id="register-your-multi-factor-authentication-provider-in-azure" class="xliff"></a>

1.  Vá para o [portal clássico do Azure](http://manage.windowsazure.com) e entre como um administrador de assinatura do Azure.

2.  No canto inferior esquerdo, clique em **Novo**.

3.  Clique em **Serviços de Aplicativos &gt; Active Directory &gt; Provedor de Multi-Factor Auth &gt; Criação Rápida**.

![Imagem de MFA de criação rápida no portal do Azure](media/MIM-SSPR-Azureportal.png)

4.  No campo **Nome** insira **SSPRMFA**, em seguida, clique em **Criar**.

![Imagem de MFA do portal do Azure](media/MIM-SSPR-Azureportal-2.png)

5.  Clique em **Active Directory** no menu do portal clássico do Azure e, em seguida, clique na guia **Provedores de Autenticação Multifator**.

6.  Clique em **SSPRMFA** e, em seguida, clique em **Gerenciar** na parte inferior da tela.

    ![Ícone Gerenciar portal do Azure](media/MIM-SSPR-ManageButton.png)

7.  Na nova janela, no painel esquerdo, em **Configurar**, clique em **Configurações**.

8.  Em **Alerta de Fraude**, desmarque **Bloquear usuário quando a fraude for reportada** . Isso é feito para evitar o bloqueio de todo o serviço.

9. Na janela **Azure Multi-Factor Authentication** que se abre, clique em **SDK** sob **Downloads** no menu à esquerda.

10. Clique no link **Baixar** na coluna ZIP do arquivo com a linguagem **SDK para o ASP.net 2.0 C#**.

    ![Imagem do arquivo zip baixada do Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Copie o arquivo ZIP resultante para cada sistema em que o serviço do MIM está instalado.  Esteja ciente de que o arquivo ZIP contém o material para chave que é usado para autenticar o serviço Azure MFA.

### Atualizar o arquivo de configuração
<a id="update-the-configuration-file" class="xliff"></a>

1. Entre no computador em que Serviço do MIM está instalado, como o usuário que instalou o MIM.

2. Crie uma nova pasta do diretório localizada abaixo do diretório em que o Serviço do MIM foi instalado, como **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Usando o Windows Explorer, navegue até a pasta **\pf\certs** do arquivo ZIP baixado na seção anterior e copie o arquivo **cert_key.p12** no novo diretório.

4.  No arquivo zip do SDK, na pasta **\pf**, abra o arquivo **pf_auth.cs**.

5.  Localize estes três parâmetros: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![imagem de código pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  Em **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**, abra o arquivo: **MfaSettings**.xml.

7.  Copie os valores dos parâmetros `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` no arquivo pf_aut.cs para seus respectivos xml elementos no arquivo MfaSettings.xml.

8.  No arquivo zip do SDK, sob \pf\certs, extraia o arquivo **cert_key.p12** e digite o caminho completo para ele no arquivo MfaSettings.xml no elemento xml `<CertFilePath>` .

9. No elemento `<username>` , insira qualquer nome de usuário.

10. No elemento `<DefaultCountryCode>`, insira seu código de país padrão. No caso de os números de telefone serem registrados para usuários sem um código de país, este é o código de país que eles receberão. Caso um usuário tenha um código de país internacional, ele deve ser incluído no número de telefone registrado.

11. Salve o arquivo MfaSettings.xml com o mesmo nome no mesmo local.

#### Configurar a porta de telefone ou o grupo de SMS de senha de uso único
<a id="configure-the-phone-gate-or-the-one-time-password-sms-gate" class="xliff"></a>

1.  Inicie o Internet Explorer e navegue até o Portal do MIM, autenticando como o administrador do MIM e, em seguida, clique em  **Fluxos de Trabalho** na barra de navegação à esquerda.

    ![Imagem de navegação do Portal do MIM](media/MIM-SSPR-workflow.jpg)

2.  Marque **Fluxo de trabalho AuthN de redefinição de senha**.

    ![Imagem de fluxos de trabalho do Portal do MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Clique na guia **Atividades** e, em seguida, role para baixo até **Adicionar atividade**.

4.  Selecione **Porta do Telefone** ou **Portão de SMS de Senha de Uso Único**, clique em **Selecionar** e, em seguida, em **OK**.

Agora, os usuários em sua organização podem se registrar para redefinição de senha.  Durante este processo, eles deverão inserir o número de telefone de trabalho e de celular para que o sistema possa ligar para eles (ou enviar mensagens SMS).

#### Registrar usuários para redefinição de senha
<a id="register-users-for-password-reset" class="xliff"></a>

1.  Um usuário inicia um navegador da Web e navega até o Portal de Registro de Redefinição de Senha do MIM.  (Normalmente, este portal estará configurado com a autenticação do Windows).  No portal, eles fornecerão o nome de usuário e a senha novamente para confirmar sua identidade.

    Eles precisam entrar no Portal de Registro de Senha e autenticar-se usando o nome de usuário e a senha.

2.  No campo **Número de Telefone** ou **Telefone Celular**  , eles precisam inserir um código de país, um espaço e o número de telefone e clicar em **Avançar**.

    ![Imagem de verificação de telefone do MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Imagem de verificação de celular do MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## Como ele funciona para seus usuários?
<a id="how-does-it-work-for-your-users" class="xliff"></a>
Agora que tudo está configurado e em execução, convém saber pelo que os seus usuários terão de passar ao redefinirem suas senhas logo antes das férias e perceberem ao voltar que as esqueceram completamente.

Há duas maneiras de o usuário usar a funcionalidade de redefinir a senha e desbloquear conta: na tela de entrada do Windows ou no portal de autoatendimento.

Ao instalar os Suplementos e Extensões do MIM em um computador ingressado no domínio conectado pela rede organizacional ao Serviço do MIM, os usuários podem recuperar uma senha esquecida na experiência de logon na área de trabalho.  As etapas a seguir guiarão você pelo processo.

#### Redefinição de senha integrada do logon na área de trabalho do Windows
<a id="windows-desktop-login-integrated-password-reset" class="xliff"></a>

1.  Se o usuário inserir a senha incorreta várias vezes, na tela de entrada, haverá a opção de clicar em **Problemas ao efetuar logon?**. .

    ![Imagem de tela de entrada](media/MIM-SSPR-problemsloggingin.JPG)

    Clicar nesse link vai levá-los à tela de redefinição de senha do MIM, na qual eles podem alterar a senha ou desbloquear sua conta.

    ![Imagem de redefinição de senha do MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  O usuário será direcionado para autenticar. Se MFA tiver sido configurado, o usuário receberá uma chamada telefônica.

3.  No segundo plano, o que está acontecendo é que o Azure MFA então faz um telefonema para o número que o usuário forneceu ao se inscrever para o serviço.

4.  Quando um usuário atender o telefone, ele será solicitado a pressionar a tecla de cerquilha (#) no telefone. Então o usuário clica em **Avançar** no portal.

    Se você configurar outros portões também, o usuário será solicitado a fornecer mais informações nas telas subsequentes.

    > [!NOTE]
    > Se o usuário ficar impaciente e clicar em **Avançar** antes de pressionar a tecla jogo-da-velha, a autenticação falhará.

5.  Após a autenticação bem-sucedida, o usuário terá duas opções: desbloquear sua conta e manter senha atual ou definir uma nova senha.

6.  Então o usuário deve digitar uma nova senha duas vezes e a senha será redefinida.

#### Acesso no portal de autoatendimento
<a id="access-from-the-self-service-portal" class="xliff"></a>

1.  Os usuários podem abrir um navegador da Web, navegar até o **Portal de Redefinição de Senha** , digitar o nome de usuário e clicar em **Avançar**.

    Se MFA tiver sido configurado, o usuário receberá uma chamada telefônica. No segundo plano, o que está acontecendo é que o Azure MFA então faz um telefonema para o número que o usuário forneceu ao se inscrever para o serviço.

    Quando um usuário atender o telefone, ele será solicitado a pressionar a tecla sustenido # no telefone. Então o usuário clica em **Avançar** no portal.

2.  Se você configurar outros portões também, o usuário será solicitado a fornecer mais informações nas telas subsequentes.

    > [!NOTE]
    > Se o usuário ficar impaciente e clicar em **Avançar** antes de pressionar a tecla jogo-da-velha, a autenticação falhará.

3.  O usuário precisará escolher se deseja redefinir sua senha ou desbloquear sua conta. Se ele optar por desbloquear a conta, a conta será desbloqueada.

    ![Imagem de desbloqueio de conta do assistente de logon do MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Após a autenticação bem-sucedida, o usuário terá duas opções: manter sua senha atual ou definir uma nova senha.

5.  ![Imagem de desbloqueio da conta do MIM bem-sucedido](media/MIM-SSPR-account-unlock.JPG)

6.  Se o usuário optar por redefinir a senha, será necessário digitar uma nova senha duas vezes e clicar em **Avançar** para alterá-la.

    ![Imagem de redefinição de senha do assistente de logon do MIM](media/MIM-SSPR-PR1.JPG)
