---
title: Trabalhar com Autoatendimento de Redefinição de Senha | Microsoft Docs
description: Veja as novidades de redefinição de senha de autoatendimento no MIM 2016, incluindo como a SSPR funciona com autenticação multifator.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 41aba931111d6ef46e60dfed173362e59c411dfe
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044269"
---
# <a name="self-service-password-reset-deployment-options"></a>Opções de implantação de Autoatendimento de Redefinição de Senha

Para novos clientes que estão [licenciados para o Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), é recomendável usar o [Autoatendimento de Redefinição de Senha do Azure AD](/azure/active-directory/authentication/concept-sspr-howitworks) para fornecer a experiência do usuário final.  O Autoatendimento de Redefinição de Senha do Azure AD fornece uma experiência baseada na Web e integrada ao Windows para que um usuário redefina sua própria senha e dá suporte a muitas das mesmas funcionalidades que o MIM, incluindo email alternativo e portões de Perguntas e Respostas.  Ao implantar o Autoatendimento de Redefinição de Senha do Azure AD, o Azure AD Connect dá suporte à [realização de write-back das novas senhas no AD DS](/azure/active-directory/authentication/concept-sspr-writeback), e o [Serviço de Notificação de Alteração de Senha](deploying-mim-password-change-notification-service-on-domain-controller.md) do MIM também pode ser usado para encaminhar as senhas para outros sistemas, tais como um servidor de diretório de outro fornecedor.  A implantação do MIM para [gerenciamento de senhas](infrastructure/mim2016-password-management.md) não exige que o Serviço MIM ou os portais de registro ou de autoatendimento de redefinição de senha estejam implantados.  Em vez disso, você pode seguir estas etapas:

- Primeiro, se você precisar enviar senhas de diretórios que não sejam do Azure AD e do AD DS, implantar a Sincronização do MIM com conectores para o Active Directory Domain Services e quaisquer sistemas de destino adicionais, configurar o MIM para [gerenciamento de senhas](infrastructure/mim2016-password-management.md) e implantar o [Serviço de Notificação de Alteração de Senha](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Em seguida, se você precisar enviar senhas em diretórios diferentes do Azure AD, configure o Azure AD Connect para [efetuar write-back das novas senhas para o AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- Opcionalmente, [faça o pré-registro de usuários](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Por fim, [distribua o Autoatendimento de Redefinição de Senha do Azure AD para seus usuários finais](/azure/active-directory/authentication/howto-sspr-deployment).

Para clientes existentes que implantaram o FIM (Forefront Identity Manager) para o Autoatendimento de Redefinição de Senha e são licenciados para o Azure Active Directory Premium, é recomendável planejar a transição para o Autoatendimento de Redefinição de Senha do Azure AD.  Você pode fazer a transição dos usuários finais para o Autoatendimento de Redefinição de Senha do Azure AD sem necessidade de registrá-los novamente, [sincronizando um número de telefone celular ou endereço de email alternativo do usuário ou configurando-o por meio do PowerShell](/azure/active-directory/authentication/howto-sspr-authenticationdata). Após os usuários se registrarem para o Autoatendimento de Redefinição de Senha do Azure AD, o portal de redefinição de senha do FIM pode ser desativado.

Para clientes que ainda não implantaram o Autoatendimento de Redefinição de Senha do Azure AD para seus usuários, o MIM também fornece portais de Autoatendimento de Redefinição de Senha.  Quando comparado ao FIM, o MIM 2016 inclui as seguintes alterações:

- O portal de Redefinição de Senha de Autoatendimento do MIM e a tela de logon do Windows agora permitem aos usuários desbloquear suas contas sem alterar suas senhas.
- Um novo portão de autenticação, Portão de Telefone, foi adicionado ao MIM. Isso permite a autenticação do usuário via meio de chamada de telefone por meio do serviço MFA (Autenticação Multifator do Microsoft Azure).

Builds de versão 2016 do MIM até a versão 4.5.26.0 dependiam de que o cliente baixasse o SDK de MFA do Azure (Software Development Kit de Autenticação Multifator do Azure).  Esse SDK foi preterido e o SDK do MFA do Azure terá suporte para os clientes atuais somente até a data de baixa, em 14 de novembro de 2018. Até essa data, os clientes devem contatar o atendimento ao cliente do Azure para receber seu pacote de credenciais do serviço MFA gerado, pois eles não poderão baixar o SDK do MFA do Azure. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>NOVO! Atualizar a configuração do MFA do Azure para o Servidor de Autenticação Multifator do Azure

Este [artigo](working-with-mfaserver-for-mim.md) descreve como atualizar seu portal de Autoatendimento de Redefinição de Senha do MIM da implantação e a configuração do PAM, usando o Servidor de Autenticação Multifator do Azure para autenticação multifator.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Implantação do portal de Autoatendimento de Redefinição de Senha usando o MFA do Azure para Autenticação Multifator

A seção a seguir descreve como implantar o portal de Autoatendimento de Redefinição de Senha do MIM usando o MFA do Azure para a Autenticação Multifator.  Essas etapas são necessárias apenas para clientes que não estão usando o Autoatendimento de Redefinição de Senha do Azure AD para seus usuários.

O Microsoft Azure Multi-Factor Authentication é um serviço de autenticação que exige que os usuários verifiquem suas tentativas de entrada com um aplicativo móvel, chamada telefônica ou mensagem de texto. Ele está disponível para uso com o Active Directory do Microsoft Azure e como um serviço para aplicativos corporativos de nuvem e local.

O Azure MFA fornece um mecanismo de autenticação adicional que pode reforçar os processos de autenticação existentes, como aqueles realizados pelo MIM para assistência de logon de autoatendimento.

Ao usar o Azure MFA, os usuários são autenticados com o sistema para verificar sua identidade ao tentar recuperar o acesso à conta e aos recursos. A autenticação pode ser por meio de SMS ou chamada telefônica.   Quanto mais forte for a autenticação, maior será a confiança de que a pessoa que está tentando acessar é realmente o usuário que possui a identidade. Uma vez autenticado, o usuário pode escolher uma nova senha para substituir a antiga.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Pré-requisitos para configurar o desbloqueio da conta de autoatendimento e a redefinição de senha usando o MFA

Esta seção pressupõe que você tenha baixado e concluído a implantação [Sincronização do MIM, Serviço MIM e MIM componentes do Portal do MIM](microsoft-identity-manager-deploy.md) do Microsoft Identity Manager 2016, incluindo os seguintes componentes e serviços:

-   Um Windows Server 2008 R2 ou posterior foi configurado como servidor do Active Directory, incluindo serviços de domínio do AD e o controlador de domínio com um domínio designado (um “domínio corporativo”)

-   Uma política de grupo é definida para o bloqueio de conta

-   O Serviço de Sincronização do MIM 2016 é instalado e executado em um servidor ingressado no domínio para o domínio do AD

-   O Serviço e o Portal do MIM 2016, incluindo o Portal de Registro do SSPR e o Portal de Redefinição do SSPR, são instalados e executados em um servidor (podem estar colocalizados com Sincronização)

-   A Sincronização do MIM deve ser configurada para sincronização de identidades do AD-MIM, incluindo:

    -   Configurando o ADMA (Agente de Gerenciamento do Active Directory) para conectividade com o AD DS e a capacidade para importar dados de identidade e exportá-los para o Active Directory.

    -   Configurando o MIM MA (Agente de Gerenciamento do MIM) para conectividade com o DB (Banco de Dados) de serviço do FIM (Forefront Identity Manager) e a capacidade para importar dados de identidade e exportá-los para o banco de dados do FIM.

    -   Configurando as regras de sincronização no Portal do MIM para permitir a sincronização de dados de usuário e facilitar a atividades baseadas em sincronização no Serviço do MIM.

-   Os Suplementos e as Extensões do MIM 2016, incluindo o cliente integrado de Logon do Windows do SSPR, são implantados no servidor ou em um computador cliente separado.

Esse cenário requer que você tenha CALs do MIM para seus usuários, bem como uma assinatura para o MFA do Azure.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Preparação do MIM para trabalhar com à autenticação multifator
Configure a Sincronização do MIM para dar suporte para redefinição de senha e funcionalidade de desbloqueio de conta. Para obter mais informações, consulte [Instalação dos complementos e extensões FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalação do FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Portões de autenticação de SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) e [Guia de laboratório de teste de SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

Na próxima seção, você configurará o provedor do Azure MFA no Active Directory do Microsoft Azure. Como parte dessa ação, você gerará um arquivo que contém o material de autenticação que o MFA exige para poder entrar em contato com o Azure MFA.  Para continuar, você precisará de uma assinatura do Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Como registrar o provedor de autenticação multifator no Azure

1.  Crie um [provedor do MFA](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Abra um caso de suporte e solicite o SDK direto para o ASP.net 2.0 C#. O SDK só será fornecido aos usuários atuais do MIM com o MFA, pois o SDK direto foi descontinuado. Novos clientes devem adotar a próxima versão do MIM que será integrada ao servidor MFA.

3. Copie o arquivo ZIP resultante para cada sistema em que o serviço do MIM está instalado.  Esteja ciente de que o arquivo ZIP contém o material para chave que é usado para autenticar o serviço Azure MFA.

### <a name="update-the-configuration-file"></a>Atualizar o arquivo de configuração

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

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Configurar a porta de telefone ou o grupo de SMS de senha de uso único

1.  Inicie o Internet Explorer e navegue até o Portal do MIM, autenticando como o administrador do MIM e, em seguida, clique em  **Fluxos de Trabalho** na barra de navegação à esquerda.

    ![Imagem de navegação do Portal do MIM](media/MIM-SSPR-workflow.jpg)

2.  Marque **Fluxo de trabalho AuthN de redefinição de senha**.

    ![Imagem de fluxos de trabalho do Portal do MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Clique na guia **Atividades** e, em seguida, role para baixo até **Adicionar atividade**.

4.  Selecione **Porta do Telefone** ou **Portão de SMS de Senha de Uso Único**, clique em **Selecionar** e, em seguida, em **OK**.

Observação: se você estiver usando o Servidor de MFA do Azure ou outro provedor que gere a senha de uso único, verifique se o campo de comprimento acima tem o mesmo comprimento gerado pelo provedor de MFA.  Para o Servidor de MFA do Azure, o comprimento precisa ser de 6.  O Servidor de MFA do Azure também gera mensagens de texto para que a mensagem SMS seja ignorada.

Agora, os usuários em sua organização podem se registrar para redefinição de senha.  Durante este processo, eles deverão inserir o número de telefone de trabalho e de celular para que o sistema possa ligar para eles (ou enviar mensagens SMS).

#### <a name="register-users-for-password-reset"></a>Registrar usuários para redefinição de senha

1.  Um usuário inicia um navegador da Web e navega até o Portal de Registro de Redefinição de Senha do MIM.  (Normalmente, este portal estará configurado com a autenticação do Windows).  No portal, eles fornecerão o nome de usuário e a senha novamente para confirmar sua identidade.

    Eles precisam entrar no Portal de Registro de Senha e autenticar-se usando o nome de usuário e a senha.

2.  No campo **Número de Telefone** ou **Telefone Celular**  , eles precisam inserir um código de país, um espaço e o número de telefone e clicar em **Avançar**.

    ![Imagem de verificação de telefone do MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Imagem de verificação de celular do MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Como ele funciona para seus usuários?
Agora que tudo está configurado e em execução, convém saber pelo que os seus usuários terão de passar ao redefinirem suas senhas logo antes das férias e perceberem ao voltar que as esqueceram completamente.

Há duas maneiras de o usuário usar a funcionalidade de redefinir a senha e desbloquear conta: na tela de entrada do Windows ou no portal de autoatendimento.

Ao instalar os Suplementos e Extensões do MIM em um computador ingressado no domínio conectado pela rede organizacional ao Serviço do MIM, os usuários podem recuperar uma senha esquecida na experiência de logon na área de trabalho.  As etapas a seguir guiarão você pelo processo.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Redefinição de senha integrada do logon na área de trabalho do Windows

1.  Se o usuário inserir a senha incorreta várias vezes, na tela de entrada, haverá a opção de clicar em **Problemas ao efetuar logon?** . .

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

#### <a name="access-from-the-self-service-portal"></a>Acesso no portal de autoatendimento

1.  Os usuários podem abrir um navegador da Web, navegar até o **Portal de Redefinição de Senha** , digitar o nome de usuário e clicar em **Avançar**.

    Se MFA tiver sido configurado, o usuário receberá uma chamada telefônica. No segundo plano, o que está acontecendo é que o Azure MFA então faz um telefonema para o número que o usuário forneceu ao se inscrever para o serviço.

    Quando um usuário atender o telefone, ele será solicitado a pressionar a tecla sustenido # no telefone. Então o usuário clica em **Avançar** no portal.

2.  Se você configurar outros portões também, o usuário será solicitado a fornecer mais informações nas telas subsequentes.

    > [!NOTE]
    > Se o usuário ficar impaciente e clicar em **Avançar** antes de pressionar a tecla jogo-da-velha, a autenticação falhará.

3.  O usuário precisará escolher se deseja redefinir sua senha ou desbloquear sua conta. Se ele optar por desbloquear a conta, a conta será desbloqueada.

    ![Imagem de desbloqueio de conta do assistente de logon do MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Após a autenticação bem-sucedida, o usuário terá duas opções: manter sua senha atual ou definir uma nova senha.

5.  ![Imagem de
6.  conta do MIM desbloqueada com êxito](media/MIM-SSPR-account-unlock.JPG)

6.  Se o usuário optar por redefinir a senha, será necessário digitar uma nova senha duas vezes e clicar em **Avançar** para alterá-la.
