---
title: "Personalizações do portal do Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Personalizações do portal do Microsoft Identity Manager 2016


>[!Warning]
Limpe o cache do navegador quando forem feitas personalizações no CSS.

No MIM (Microsoft Identity Manager) 2016, você pode personalizar elementos selecionados dos portais de senha, incluindo o logotipo da faixa, os recursos de cadeia de caracteres e folhas de estilo em cascata.

Para fazer isso, há algumas exigências dependendo do nível de personalização. Veja a seguir uma lista de itens envolvidos na personalização dos portais de registro e redefinição de senha do MIM 2016.

-   Pasta Personalizações – Essa é a pasta que o MIM 2016 consultará antes de usar os padrões. Cada portal que será personalizado exige uma pasta Personalizações. As personalizações só devem ser feitas nesta pasta, pois a configuração não a substituirá em atualizações, instalações no modo de alteração ou instalações no modo de reparo.

-   Strings.resources – é um arquivo baseado em XML que permite a modificação das cadeias de caracteres que aparecem no portal. Esse arquivo deve residir na pasta Personalizações.

-   Style.css – é a folha de estilos em cascata usada pelos portais para personalização. Essa folha de estilos deve ser criada e modificada para alterar o logotipo, ou pode ser totalmente substituída por suas próprias personalizações.

Para obter instruções detalhadas sobre como personalizar os portais de registro e redefinição de senhas, confira o Guia do Laboratório de Teste: demonstração da personalização do portal de registro e redefinição de senha do MIM 2016.

>[!AVISO] Para que o MIM reconheça as alterações personalizadas, você deve reiniciar o IIS executando iisreset.


## <a name="creating-the-customizations-folder"></a>Criação da pasta Personalizações

Na inicialização, o MIM procurará pelo arquivo Strings.resources na pasta Personalizações antes de usar os padrões. Você deve criar uma pasta Personalizações no diretório do portal que você quer personalizar (ou seja, Portal de Registro de Senha ou Portal de Redefinição de Senha). Se você quiser personalizar os dois portais, será necessário criar uma pasta Personalizações sob cada um dos seguintes itens:

-   C:\\Arquivos de Programas\\Microsoft Forefront Identity Manager\\2010\\Portal de Registro de Senha

-   C:\\Arquivos de Programas\\Microsoft Forefront Identity Manager\\2010\\Portal de Redefinição de Senha

### <a name="to-create-the-customization-folder"></a>Para criar a pasta de personalização

1.  Navegue até a pasta C:\\Arquivos de Programas\\Microsoft Forefront Identity Manager\\2010\\Portal de Registro de Senha.

2.  Crie uma pasta chamada Personalizações.

3.  Navegue um nível acima até a pasta do Portal de Redefinição de Senha e crie uma pasta chamada Personalizações.

## <a name="customizing-strings"></a>Personalização de cadeias de caracteres

Muitas das cadeias de caracteres na interface do usuário do portal podem ser personalizadas criando um arquivo Strings.resources e adicionando esse arquivo à pasta Personalizações. Será necessário criar um arquivo Strings.resources para cada portal que você quer personalizar.

### <a name="to-customize-strings"></a>Para personalizar as cadeias de caracteres

1.  Usando o bloco de notas, copie o código a seguir e salve-o na pasta Personalizações como Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Na seção `<!-- Customizations begin here -->`, altere o nome dos dados para corresponder às cadeias de caracteres que você deseja personalizar e insira o valor para essa cadeia de caracteres entre as marcas <value></value>. Veja a lista abaixo para conhecer as cadeias de caracteres que podem ser personalizadas e seus valores padrão.

>[!NOTE]
O arquivo **Strings.resources** tem neutralidade de idioma. Para criar cadeias de caracteres personalizadas específicas de um idioma, você deve ter esse pacote de idiomas instalado e salvar o arquivo no formato Cadeias de caracteres. `<language>-<culture>.resources`, por exemplo Strings.en-us.resources.

Veja a seguir uma lista de cadeias de caracteres de portais que podem ser personalizadas.

<a name="portal-strings"></a>Cadeias de caracteres de portais
--------------

| Nome                                                     | Valor padrão                                                                                                                                                                                                                                                                                                                                         | Comentário                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Sobre o         |       |
| ButtonCancel                                             | Cancelar                                                                                 |     |
| ButtonFinish                                             | Concluir    |    |
| ButtonNext                                               | Avançar    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | Sua sessão não está mais ativa. Você pode fechar a janela ou pode reiniciar clicando no link abaixo.         |      |
| CancelFinishedTitle                                      | Sessão Encerrada                                                              |      |
| ErrorDescription_3000                                    | Ocorreu um erro. Tente novamente e, se o problema persistir, entre em contato com o administrador do sistema ou o suporte técnico. (Erro 3000)       |          |
| ErrorDescription_3001                                    | Insira seu nome de usuário corretamente. Se ainda não for possível redefinir sua senha, entre em contato com o      |           |
|                                                          | suporte técnico para obter assistência. (Erro 3001)   |                   |
| ErrorDescription_3002                                    | Sua sessão foi encerrada. Retorne à home page para começar novamente. (Erro 3002)                                     |                |
| ErrorDescription_3003                                    | A conta de usuário atual não é reconhecida pelo Forefront Identity Manager. Entre em contato com o administrador do sistema ou com o suporte técnico. (Erro 3003)           |               |
| ErrorDescription_3004                                    | Você não está autorizado a se registrar para redefinição de senha. Entre em contato com o administrador do sistema ou o suporte técnico. (Erro 3004)     |              |
| ErrorDescription_3005                                    | Uma ou mais respostas que você forneceu não correspondem às respostas fornecidas durante o Registro de Senha. Para redefinir sua senha, as respostas fornecidas agora devem coincidir com as respostas fornecidas durante o registro. Comece novamente na home page ou entre em contato com o administrador do sistema ou o suporte técnico. (Erro 3005) |           |
| ErrorDescription_3006                                    | A senha inserida está incorreta. Você deve inserir a senha correta para se registrar na Redefinição de Senha. (Erro 3006)            |              |
| ErrorDescription_3007                                    | Você está temporariamente proibido de redefinir sua senha. Tente novamente mais tarde ou entre em contato com o administrador do sistema ou o suporte técnico para obter assistência. (Erro 3007)     |         |
| ErrorDescription_3008                                    | Ocorreu um erro. Tente novamente e, se o problema persistir, entre em contato com o administrador do sistema ou o suporte técnico. (Erro 3008)        |          |
| ErrorDescription_3009                                    | Sua entrada contém texto em um formato que não é permitido. Tente novamente com uma entrada diferente ou contate o administrador do sistema ou o suporte técnico. (Erro 3009)     |             |
| ErrorDescription_3010_Registration                       | O script não está habilitado em seu navegador. Habilite o script e retorne à home page do Registro de Senha, ou entre em contato com o suporte técnico para obter assistência.      |               |
| ErrorDescription_3010_Reset                              | O script não está habilitado em seu navegador. Habilite o script e retorne à home page de Redefinição de Senha, ou entre em contato com o suporte técnico para obter assistência.     |            |
| ErrorDescription_3011                                    | Este site usa cookies. Configure seu navegador para aceitar cookies e tente novamente, ou entre em contato com o suporte técnico para obter assistência.          |                                           |
| ErrorDescription_3012                                    | Os dados inseridos não correspondem ao código de segurança enviado a você. Você pode tentar redefinir a senha novamente ou entrar em contato com o suporte técnico para obter assistência.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Não é possível enviar um código de segurança. Entre em contato com o suporte técnico para obter assistência.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Insira seu nome de usuário no formato correto.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Insira um nome de usuário para continuar.                                              |                         |
| ErrorMessagePasswordRequired                             | Insira uma senha.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Certifique-se de que as duas senhas correspondam.                                |           |
| ErrorPageDefaultHeading                                  | Erro do aplicativo                                            |               |
| ErrorPageServerTime                                      | Hora do servidor: {0:T}                     | {0} é a hora na qual a exceção foi capturada. O 'T' faz com que o tempo decorrido seja formatado como um "horário longo". Ele terminará mostrando a hora, minuto e segundo, e possivelmente a designação de AM/PM (dependendo da região). |
| ErrorPageTitle                                           | Forefront Identity Management – Erro de senha                     |   |
| ErrorTitle_3000                                          | Erro do                                  |  |
| ErrorTitle_3001                                          | Acesso Negado                          |  |
| ErrorTitle_3002                                          | Sessão Encerrada                          |  |
| ErrorTitle_3003                                          | Usuário não reconhecido                      |  |
| ErrorTitle_3004                                          | Usuário não autorizado                      |  |
| ErrorTitle_3005                                          | As respostas não correspondem                    |  |
| ErrorTitle_3006                                          | Senha incorreta                     |  |  
| ErrorTitle_3007                                          | Acesso negado temporariamente              |  |
| ErrorTitle_3008                                          | Erro de comunicação                    |  |
| ErrorTitle_3009                                          | Entrada proibida                       |  |
| ErrorTitle_3010                                          | Erro de configuração do navegador            |  |
| ErrorTitle_3011                                          | Erro de configuração do navegador            |  |
| ErrorTitle_3012                                          | Falha na verificação                    |  |
| ErrorTitle_3013                                          | Não é possível enviar um código de segurança   |  |
| FinalizeRegistrationHeading1                             | Se você precisar redefinir sua senha:        |   |
| FinalizeRegistrationSubHeading1                          | Acesse o portal de redefinição de senha   |   |
| FinalizeRegistrationSubHeading2                          | Verifique sua identidade   |   |
| FinalizeRegistrationSubHeading3                          | Escolha sua nova senha    |     |
| FinishingDescription                                     | Escolha sua nova senha        |    |
| FinishingTitle                                           | Redefinição de senha:      |     |
| GotoPortalPrefix                                         | Acesse     |        |
| GotoPortalSuffix                                         | home page     |      |
| LabelTroubleshootingLinkText                             | Exibir Detalhes           |    |
| LoadingText                                              | Carregando...     |  |
| NoScriptTagErrorMessage                                  | O script não está habilitado em seu navegador. Habilite o script e retorne à home page, ou entre em contato com o suporte técnico para obter assistência.      |   |
| PasswordResetOperationGeneralErrorMessage                | Erro ao tentar redefinir a senha.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | A senha não atende às políticas de senha de sua organização.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Erro ao redefinir a senha, o usuário não pode alterar a senha.    |   |
| PrivacyStatement                                         | Política de privacidade                                                      |    |
| RegistrationDescription                                  | Registro de senha de autoatendimento     |    |
| RegistrationMission                                      | Se você esquecer sua senha, poderá redefini-la por conta própria sem chamar o suporte técnico.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management – Registro de Senha                                          |   |
| RegistrationSteps                                        | Clique em "Avançar" para começar o processo de registro.   |      |
| RegistrationSuccessDescription                           | Você está registrado        |   |
| RegistrationSuccessTitle                                 | Concluído:   |    |
| RegistrationWelcomeTitle                                 | Registro da Senha:    | |
| ResetDescription                                         | Autoatendimento de redefinição de senha  |    |
| ResetEnterNamePrompt                                     | Insira o nome do usuário abaixo     |     |
| ResetEnterPassword                                       | Insira uma nova senha:                                                  |   |
| ResetExample1                                            | contoso\\ddias                                                                            |      |
| ResetExample2                                            | ddias\@contoso.com     |      |
| ResetExamples                                            | Exemplos:  |    |
| ResetPageTitle                                           | Forefront Identity Management – Redefinição de Senha       |     |
| ResetReenterPassword                                     | Insira a senha novamente:    | |
| ResetSuccessDescription                                  | Sua senha foi redefinida    |    |
| ResetSuccessTitle                                        | Êxito:                                |    |
| ResetUseNewPassword                                      | Agora você pode usar sua nova senha para fazer logon.     |      |
| ResetUsernameTextFormat                                  | (Redefinição de senha para {0})       | {0} é o logon do usuário             |
| ResetWelcomeTitle                                        | Redefinição de senha:     |      |
| TroubleshootingEmailSubject                              | Detalhes do erro de processamento de solicitação do FIM     |       |
| TroubleshootingLabelAttributes                           | Atributos:    |    |
| TroubleshootingLabelCloseButton                          | Fechar       |    |
| TroubleshootingLabelCopyToClipboard                      | Copiar para a área de transferência     |     |
| TroubleshootingLabelCorrelationId                        | Id de correlação:      |          |
| TroubleshootingLabelDetails                              | Detalhes:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | Essas informações foram copiadas para a área de transferência.           |    |
| TroubleshootingLabelRequestId                            | ID da solicitação:                  |    |
| TroubleshootingLabelSendEmail                            | Enviar informações por email                            |    |
| TroubleshootingLabelSource                               | Motivo:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Ver detalhes da solicitação        |              |                                                    
| TroubleshootingLinkText                                  | Informações sobre a solução de problemas          |                  | |



Veja a seguir uma lista de cadeias de caracteres de Portais de Autenticação que podem ser personalizadas.

<a name="authentication-gate-strings"></a>Cadeias de caracteres de portal de autenticação
---------------------------

| Nome                                          | Valor padrão                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | Endereço de email:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | O campo de endereço de email não pode ficar vazio.     |          |
| OTPEmailRegistrationFooterReadOnly            | Para atualizar seu endereço de email, siga o processo definido por sua organização ou entre em contato com o suporte técnico.                   |          |
| OTPEmailRegistrationFooterReadWrite           | O endereço de email é armazenado por sua organização no Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Verificação de endereço de email   |          |
| OTPEmailRegistrationHeaderReadOnly            | Se você precisar redefinir sua senha, um código de segurança de verificação será enviado ao seu email. Se o endereço de email mostrado abaixo não estiver correto, atualize-o para usar a redefinição de senha por autoatendimento. |          |
| OTPEmailRegistrationHeaderReadWrite           | Insira seu endereço de email abaixo. Se você precisar redefinir sua senha, um código de verificação será enviado ao seu email.                 |          |
| OTPEmailResetGateTitle                        | Verifique sua identidade: Email         |          |
| OTPEmailResetHeader                           | Insira seu código de segurança abaixo. Um código de segurança foi enviado para o endereço de email registrado para esta organização.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | O valor especificado não corresponde ao formato esperado.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | O campo de código de segurança não pode estar vazio.        |          |
| OTPResetVerificationLabel                     | Código de segurança:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Para atualizar seu número de celular, siga o processo definido por sua organização ou entre em contato com o suporte técnico.   ||
| OTPSmsRegistrationFooterReadWrite                     | O número de celular é armazenado por sua organização no Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Verificação de celular                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Se você precisar redefinir sua senha, um código de segurança de verificação será enviado ao seu celular. Se o número de celular mostrado abaixo não estiver correto, atualize-o para usar a redefinição de senha por autoatendimento. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Insira o número de seu celular abaixo. Se você precisar redefinir sua senha, um código de verificação será enviado ao seu celular.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | O campo de número de celular não pode ficar vazio.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Celular:                    |   |
| OTPSmsResetGateTitle                                  | Verifique sua identidade: Celular         |   |
| OTPSmsResetHeader                                     | Insira seu código de segurança abaixo. Um código de segurança foi enviado para o celular registrado para esta organização.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Insira sua senha atual abaixo e clique em 'Avançar'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Insira sua senha atual.                  |   |
| PasswordGateGateTitle                                 | Sua senha atual               |   |
| PasswordGatePasswordLabelText                         | Contrasenha:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(conectado como: `<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Você deve responder pelo menos {0} perguntas.        |   |
| QAGateIncorrectAnswer                                 | Suas respostas não estão corretas.       |   |
| QAGatePrivacyNotice                                   | As respostas fornecidas são armazenadas por sua organização no Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Você deve responder pelo menos {0} perguntas para se registrar.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Uma ou mais respostas não estão em conformidade com a política.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Esta resposta não está em conformidade com a política.      |   |
| QAGateRegistrationTitle                               | Registrar suas respostas         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | Você deve responder {0} das {1} perguntas a seguir.   |   |
| QAGateResetTitle                                      | Verifique sua identidade: Enviar suas respostas                                  |   |



## <a name="customizing-the-logo-banner"></a>Personalização da faixa do logotipo

A faixa padrão nas páginas do portal pode ser personalizada para sua organização.
Para personalizar a faixa do logotipo:

1.  Crie suas faixas personalizadas e salve-as como arquivos .png. Os arquivos devem atender às seguintes recomendações:

 - Tamanho: 490 x 50 pixels.

 - Profundidade de bits: 32

2.  Copie os arquivos na pasta \\Personalizações em cada portal que você quer personalizar.

3.  Crie um arquivo Style.css em cada pasta. Aponte-o para a pasta Personalizações e para o novo logotipo. Você pode alterar o nome do logotipo, se for aplicável (ou seja, /Customizations/contosologo.png). O código será semelhante a este:

   **Exemplo 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Se você estiver usando o Internet Explorer 6.0, será necessário fornecer um logotipo alternativo não transparente e adicionar o seguinte código ao Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Exemplo 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Se você estiver usando o Internet Explorer 6.0, será necessário fornecer um logotipo alternativo não transparente e adicionar o seguinte código ao Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Personalizar a imagem para SmartPhones

Você pode personalizar a imagem para SmartPhones usando o seguinte. Para personalizar a imagem do SmartPhone:

1.  Crie sua imagem e salve-a como um arquivo .png. Os arquivos devem atender às seguintes recomendações:

    - Tamanho: 190 x 50 pixels.
    - Profundidade de bits: 32

2.  Copie os arquivos na pasta \\Personalizações em cada portal que você quer personalizar.

2.  Crie um arquivo Style.css em cada pasta. Aponte-o para a pasta Personalizações e para o novo logotipo. Você pode alterar o nome do logotipo, se for aplicável (ou seja, /Customizations/contosologo.png). O código será semelhante a este:

  **Exemplo 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Personalização das folhas de estilo em cascata

Você pode modificar o layout e o estilo dos portais de senha usando uma folha de estilo em cascata (CSS) personalizada. Para usar uma CSS personalizada:

1.  Crie seus arquivos CSS personalizados e salve-os como Style.css.

2.  Copie os arquivos na pasta \\Personalizações em cada portal que você quer personalizar.

Este é um exemplo básico de um arquivo Style.css:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Para que o MIM reconheça as alterações personalizadas, você deve reiniciar o IIS executando iisreset.                                                                                                                                                                                       

Este é um exemplo mais avançado de um arquivo Style.css. Esse arquivo fornece informações específicas para smartphone e ipad para exibição dos portais nesses dispositivos.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Problemas comuns de personalização

A tabela a seguir é uma lista dos problemas comuns e conhecidos que podem ocorrer com a atualização do Serviço e do Portal do FIM. Esta tabela também inclui as soluções para esses problemas

|Problema |Resolução                                                                    |
|------|------------------------------|
| Fiz uma personalização de cadeia de caracteres, mas ela não foi refletida na interface do usuário         | Personalizações de cadeia de caracteres no strings.resources sempre exigem iisreset         |
| Depois de fazer uma alteração de strings.resources, não vejo mais nenhuma das minhas alterações de cadeia de caracteres     | Provavelmente, o formato de strings.Resources está incorreto e, portanto, é ignorado pelo portal. Verifique o log de eventos em Logs do Windows – Logs de Aplicativos e Serviços – Forefront Identity Manager                        |
| Na primeira vez que adicionei o Style.css, não vi minhas alterações de estilo no portal            | Na primeira vez que você introduz um arquivo Style.css, é necessário realizar um iisreset     |
| Novos estilos são adicionados/modificados no Style.css, mas as alterações não são vistas no navegador      | Limpe o cache do navegador e atualize a página <br/> Verifique a sintaxe do CSS    |
| Alterei diretamente o conteúdo da pasta CSS em path_to_sspr_portal\\css\\\*.css ou o logotipo da faixa em path_to_sspr_portal\\imagens\\fimlogo.png e perdi essas alterações na atualização | Em primeiro lugar, você nunca deve alterar esses arquivos. Use a pasta Personalização apenas como um meio de fornecer um logotipo de faixa e personalizações no estilo CSS em Style.css. A pasta Personalizações não é substituída propositalmente pelas atualizações principais <br/><br/>Não tente usar ferramentas como ILSpy e Reflector para alterar as cadeias de caracteres nos assemblies do portal. Use strings.resources para substituir as cadeias de caracteres padrão. Os assemblies são substituídos na atualização  |
| O logotipo do banner não é exibido nos portais / Eu ainda vejo o logotipo do FIM     | O caminho/nome da imagem em Style.css não é válido ou o cache do navegador não foi limpo          |
| O logotipo do banner é feio no IE6       | Você precisará fornecer uma imagem não transparente para IE6 e um estilo especial acompanhante no style.css        |
