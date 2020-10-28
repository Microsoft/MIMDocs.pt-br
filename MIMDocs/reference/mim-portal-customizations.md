---
title: Personalizações do portal Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 144ce2344327f16f60269b4f505c30fd470e46da
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757464"
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Personalização do portal Microsoft Identity Manager 2016

No MIM (Microsoft Identity Manager) 2016, você pode personalizar elementos selecionados dos portais de senha, incluindo o logotipo da faixa, os recursos de cadeia de caracteres e folhas de estilo em cascata.

>[!WARNING]
>Sempre limpe o cache do navegador quando quaisquer personalizações de CSS forem feitas.

Os seguintes itens são envolvidos em uso para personalizar os portais de registro e redefinição de senha do MIM 2016:

- Pasta personalizações: essa é a pasta que o MIM 2016 verifica antes de usar os padrões. Cada portal a ser personalizado requer uma pasta personalizações. As personalizações devem ser feitas somente nesta pasta, pois o processo de instalação não substitui essa pasta em atualizações, instalações do modo de alteração ou instalações do modo de reparo.

- Strings. Resources: é um arquivo baseado em XML que permite que você modifique as cadeias de caracteres que aparecem no Portal. Esse arquivo deve residir na pasta Personalizações.

- Style. css: é a folha de estilos em cascata usada pelos portais para personalização. Crie e modifique essa folha de estilos para alterar o logotipo. Você também pode substituir todo o conteúdo da folha de estilos por suas próprias personalizações.

Para obter instruções passo a passo sobre como personalizar os portais de registro de senha e de redefinição de senha, consulte Guia de laboratório de teste: demonstrando o registro de senha do MIM 2016 e a personalização do portal de redefinição.

>[!WARNING]
>Para que o MIM reconheça as alterações personalizadas, é necessário reiniciar o IIS executando `iisreset` .


## <a name="customizations-folder"></a>Pasta personalizações

Na inicialização, o MIM procura o arquivo Strings. Resources na pasta personalizações antes de usar os padrões. Crie uma pasta personalizações no diretório do portal que você deseja personalizar (ou seja, o portal de registro de senha ou o portal de redefinição de senha). Se você quiser personalizar os dois portais, crie uma pasta personalizações em cada um dos seguintes locais:

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`

Para criar uma pasta de personalizações para o portal de registro de senha:

1. Navegue até a pasta `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`.
   
2. Crie uma pasta chamada **personalizações** .

Para criar uma pasta de personalizações para o portal de redefinição de senha:

1. Navegue até a pasta `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`.
   
2. Crie uma pasta chamada **personalizações** .


## <a name="custom-strings-in-the-stringsresources-file"></a>Cadeias de caracteres personalizadas no arquivo Strings. Resources

Muitas das cadeias de caracteres na interface do usuário do portal podem ser personalizadas criando um arquivo Strings.resources e adicionando esse arquivo à pasta Personalizações. Crie um arquivo Strings. Resources para cada portal que você deseja personalizar.

Para criar cadeias de caracteres personalizadas no arquivo Strings. Resources:

1. No bloco de notas, copie e cole o código a seguir no arquivo Strings. Resources. Salve o arquivo na pasta personalizações do Portal.

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

2.  Na `<!-- Customizations begin here -->` seção no código, altere o nome de dados para corresponder às cadeias de caracteres que você deseja personalizar. Insira o valor para a cadeia de caracteres entre as `<value></value>` marcas. Consulte as próximas seções para as cadeias de caracteres que podem ser personalizadas e seus valores padrão.

>[!NOTE]
>O arquivo Strings. Resources é neutro à linguagem. Para criar cadeias de caracteres personalizadas específicas a um idioma, você deve ter esse pacote de idiomas instalado e salvar o arquivo no formato de cadeias de caracteres `<language>-<culture>.resources` . Por exemplo, para a cultura do idioma inglês, o nome do arquivo é **Strings. en-US. Resources** .


## <a name="portal-strings"></a>Cadeias de caracteres do portal
A tabela a seguir mostra as cadeias de caracteres do portal que podem ser personalizadas:

<!-- The default values are actual UI strings and should not copy edited. -->

| Nome da cadeia de caracteres do portal | Valor padrão |
|---|---|
| AboutLinkText                                            | Sobre  |
| ButtonCancel                                             | Cancelar |
| ButtonFinish                                             | Concluir |
| ButtonNext                                               | Avançar   |
| ButtonOk                                                 | OK     |
| CancelFinishedMessage                                    | Sua sessão não está mais ativa. Você pode fechar a janela ou pode reiniciar clicando no link abaixo. |
| CancelFinishedTitle                                      | Sessão Encerrada |
| ErrorDescription_3000                                    | Ocorreu um erro. Tente novamente e, se o problema persistir, entre em contato com o administrador do sistema ou o suporte técnico. (Erro 3000) |
| ErrorDescription_3001                                    | Certifique-se de inserir seu nome de usuário corretamente. Se você ainda não conseguir redefinir sua senha, entre em contato com a assistência técnica para obter assistência. (Erro 3001) |
| ErrorDescription_3002                                    | Sua sessão foi encerrada. Retorne à home page para começar novamente. (Erro 3002) |
| ErrorDescription_3003                                    | A conta de usuário atual não é reconhecida pelo Forefront Identity Manager. Entre em contato com o suporte técnico ou o administrador do sistema. (Erro 3003) |
| ErrorDescription_3004                                    | Você não está autorizado a se registrar para a redefinição de senha. Entre em contato com o suporte técnico ou o administrador do sistema. (Erro 3004) |
| ErrorDescription_3005                                    | Uma ou mais respostas que você forneceu não correspondem às respostas que você forneceu durante o registro de senha. Para redefinir sua senha, as respostas fornecidas agora devem corresponder às respostas que você forneceu ao se registrar. Você pode iniciar novamente na home page ou entrar em contato com o suporte técnico ou o administrador do sistema. (Erro 3005) |
| ErrorDescription_3006                                    | A senha que você inseriu está incorreta. Você deve inserir a senha correta para se registrar para a redefinição de senha. (Erro 3006) |
| ErrorDescription_3007                                    | Você está temporariamente proibido de redefinir sua senha. Tente novamente mais tarde ou contate o administrador do sistema ou o suporte técnico para obter assistência. (Erro 3007) |
| ErrorDescription_3008                                    | Ocorreu um erro. Tente novamente e, se o problema persistir, entre em contato com o administrador do sistema ou o suporte técnico. (Erro 3008) |
| ErrorDescription_3009                                    | Sua entrada contém texto em um formato que não é permitido. Tente novamente com uma entrada diferente ou contate o administrador do sistema ou o suporte técnico. (Erro 3009) |
| ErrorDescription_3010_Registration                       | O script não está habilitado em seu navegador. Habilite o script e retorne à home page do Registro de Senha, ou entre em contato com o suporte técnico para obter assistência. |
| ErrorDescription_3010_Reset                              | O script não está habilitado em seu navegador. Habilite o script e retorne à home page de Redefinição de Senha, ou entre em contato com o suporte técnico para obter assistência. |
| ErrorDescription_3011                                    | Este site usa cookies. Configure seu navegador para aceitar cookies e tente novamente, ou entre em contato com o suporte técnico para obter assistência. |
| ErrorDescription_3012                                    | Os dados inseridos não correspondem ao código de segurança enviado a você. Você pode tentar redefinir a senha novamente ou entrar em contato com o suporte técnico para obter assistência.  |
| ErrorDescription_3013                                    | Não é possível enviar um código de segurança. Entre em contato com o suporte técnico para obter assistência. |
| ErrorMessageDomainUsernameFormat                         | Insira seu nome de usuário no formato correto. |
| ErrorMessageDomainUsernameRequired                       | Insira um nome de usuário para continuar. |
| ErrorMessagePasswordRequired                             | Digite uma senha. |
| ErrorMessagePasswordsDoNotMatch                          | Certifique-se de que as duas senhas correspondam. |
| ErrorPageDefaultHeading                                  | Erro do aplicativo <br/>**Observação** : o título é seguido por ' = ' e a mensagem de erro.
| ErrorPageServerTime                                      | Hora do servidor: {0:T} <br/>**Observação** : {0} é a hora em que a exceção foi detectada. ' T' faz com que o tempo passado seja formatado como um "tempo longo" mostrando a hora, o minuto e o segundo. A designação AM/PM também é mostrada dependendo da cultura atual. |
| ErrorPageTitle                                           | Forefront Identity Management – Erro de senha | 
| ErrorTitle_3000                                          | Erro |
| ErrorTitle_3001                                          | Acesso negado |
| ErrorTitle_3002                                          | Sessão Encerrada |
| ErrorTitle_3003                                          | Usuário não reconhecido |
| ErrorTitle_3004                                          | Usuário não autorizado |
| ErrorTitle_3005                                          | As respostas não correspondem |
| ErrorTitle_3006                                          | Senha incorreta |
| ErrorTitle_3007                                          | Acesso negado temporariamente |
| ErrorTitle_3008                                          | Erro de comunicação |
| ErrorTitle_3009                                          | Entrada proibida |
| ErrorTitle_3010                                          | Erro de configuração do navegador |
| ErrorTitle_3011                                          | Erro de configuração do navegador |
| ErrorTitle_3012                                          | Falha na verificação |
| ErrorTitle_3013                                          | Não é possível enviar um código de segurança |
| FinalizeRegistrationHeading1                             | Se você precisar redefinir sua senha: |
| FinalizeRegistrationSubHeading1                          | Acesse o portal de redefinição de senha |
| FinalizeRegistrationSubHeading2                          | Verifique sua identidade |
| FinalizeRegistrationSubHeading3                          | Escolha sua nova senha |
| FinishingDescription                                     | Escolha sua nova senha |
| FinishingTitle                                           | Redefinição de senha: |
| GotoPortalPrefix                                         | Ir para |
| GotoPortalSuffix                                         | home page |
| LabelTroubleshootingLinkText                             | Exibir Detalhes |
| LoadingText                                              | Carregando... |
| NoScriptTagErrorMessage                                  | O script não está habilitado em seu navegador. Habilite o script e retorne à home page, ou entre em contato com o suporte técnico para obter assistência.  |
| PasswordResetOperationGeneralErrorMessage                | Erro ao tentar redefinir a senha. |
| PasswordResetOperationPolicyViolationErrorMessage        | A senha não atende às políticas de senha de sua organização. |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Erro ao redefinir a senha, o usuário não pode alterar a senha. |
| PrivacyStatement                                         | Política de Privacidade |
| RegistrationDescription                                  | Registro de senha de autoatendimento |
| RegistrationMission                                      | Se você esquecer sua senha, poderá redefini-la por conta própria sem chamar o suporte técnico. |
| RegistrationPageTitle                                    | Forefront Identity Management – Registro de Senha |
| RegistrationSteps                                        | Clique em "Avançar" para começar o processo de registro. |
| RegistrationSuccessDescription                           | Você está registrado |
| RegistrationSuccessTitle                                 | Concluído: |
| RegistrationWelcomeTitle                                 | Registro da Senha: |
| ResetDescription                                         | Redefinição de senha de autoatendimento |
| ResetEnterNamePrompt                                     | Insira o nome do usuário abaixo |
| ResetEnterPassword                                       | Insira uma nova senha: |
| ResetExample1                                            | `contoso\mmeyers` |
| ResetExample2                                            | `mmeyers\@contoso.com` |
| ResetExamples                                            | Exemplos: |
| ResetPageTitle                                           | Forefront Identity Management – Redefinição de Senha |
| ResetReenterPassword                                     | Insira a senha novamente: |
| ResetSuccessDescription                                  | Sua senha foi redefinida |
| ResetSuccessTitle                                        | Sucesso: |
| ResetUseNewPassword                                      | Agora você pode usar sua nova senha para fazer logon. |
| ResetUsernameTextFormat                                  | (Redefinição de senha para {0} ) <br/>**Observação** : {0} é o logon do usuário. |
| ResetWelcomeTitle                                        | Redefinição de senha: |
| TroubleshootingEmailSubject                              | Detalhes do erro de processamento de solicitação do FIM |
| TroubleshootingLabelAttributes                           | Atributos: |
| TroubleshootingLabelCloseButton                          | Feche |
| TroubleshootingLabelCopyToClipboard                      | Copiar para a Área de Transferência |
| TroubleshootingLabelCorrelationId                        | Id de correlação: |
| TroubleshootingLabelDetails                              | Detalhes: |
| TroubleshootingLabelPostCopyClipboardMessage             | Essas informações foram copiadas para a área de transferência. |
| TroubleshootingLabelRequestId                            | ID da solicitação: |
| TroubleshootingLabelSendEmail                            | Enviar informações por email |
| TroubleshootingLabelSource                               | Motivo: |
| TroubleshootingLabelViewRequestDetails                   | Ver detalhes da solicitação |
| TroubleshootingLinkText                                  | Informações sobre a solução de problemas |


## <a name="authentication-gate-strings"></a>Cadeias de caracteres de portão de autenticação
A tabela a seguir mostra as cadeias de caracteres de porta de autenticação que podem ser personalizadas:

<!-- The default values are actual UI strings and should not copy edited. -->

| Nome da cadeia de autenticação do portão | Valor padrão |
|---|---|
| OTPEmailRegistraionEmailTextboxLabel                  | Endereço de email: |
| OTPEmailRegistrationEmailRequiredErrorMessage         | O campo de endereço de email não pode ficar vazio. |
| OTPEmailRegistrationFooterReadOnly                    | Para atualizar seu endereço de email, siga o processo definido por sua organização ou entre em contato com o suporte técnico. |
| OTPEmailRegistrationFooterReadWrite                   | O endereço de email é armazenado por sua organização no Forefront Identity Manager. |
| OTPEmailRegistrationGateTitle                         | Verificação de endereço de email |
| OTPEmailRegistrationHeaderReadOnly                    | Se você precisar redefinir sua senha, um código de segurança de verificação será enviado ao seu email. Se o endereço de email mostrado abaixo não estiver correto, atualize-o para usar a redefinição de senha por autoatendimento. |
| OTPEmailRegistrationHeaderReadWrite                   | Insira seu endereço de email abaixo. Se você precisar redefinir sua senha, um código de verificação será enviado ao seu email. |
| OTPEmailResetGateTitle                                | Verifique sua identidade: Email |
| OTPEmailResetHeader                                   | Insira seu código de segurança abaixo. Um código de segurança foi enviado para o endereço de email registrado para esta organização. |
| OTPRegularExpressionErrorMessage                      | O valor especificado não corresponde ao formato esperado. |
| OTPResetOneTimePasswordRequiredErrorMessage           | O campo de código de segurança não pode estar vazio. |
| OTPResetVerificationLabel                             | Código de segurança: |
| OTPSmsRegistrationFooterReadOnly                      | Para atualizar seu número de celular, siga o processo definido por sua organização ou entre em contato com o suporte técnico. |
| OTPSmsRegistrationFooterReadWrite                     | O número de celular é armazenado por sua organização no Forefront Identity Manager. |
| OTPSmsRegistrationGateTitle                           | Verificação de celular |
| OTPSmsRegistrationHeaderReadOnly                      | Se você precisar redefinir sua senha, um código de segurança de verificação será enviado ao seu celular. Se o número de celular mostrado abaixo não estiver correto, atualize-o para usar a redefinição de senha por autoatendimento. |
| OTPSmsRegistrationHeaderReadWrite                     | Insira o número de seu celular abaixo. Se você precisar redefinir sua senha, um código de verificação será enviado ao seu celular. |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | O campo de número de celular não pode ficar vazio. |
| OTPSmsRegistrationSMSTextBoxLabel                     | Celular: |
| OTPSmsResetGateTitle                                  | Verifique sua identidade: Celular |
| OTPSmsResetHeader                                     | Insira seu código de segurança abaixo. Um código de segurança foi enviado para o celular registrado para esta organização. |
| PasswordGateDescriptionText                           | Insira sua senha atual abaixo e clique em 'Avançar'. |
| PasswordGateErrorMessagePasswordRequired              | Insira sua senha atual. |
| PasswordGateGateTitle                                 | Sua senha atual |
| PasswordGatePasswordLabelText                         | Senha: |
| PasswordGateUsernameTextFormat                        | `<i>` (conectado como: `<b>{0}</b>` ) `</i>` |
| QAGateErrorNotEnoughQuestionsAnswered                 | Você deve responder a pelo menos {0} perguntas. |
| QAGateIncorrectAnswer                                 | Suas respostas não estão corretas. |
| QAGatePrivacyNotice                                   | As respostas fornecidas são armazenadas por sua organização no Forefront Identity Manager. |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Você deve responder a pelo menos {0} perguntas para registrar. |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Uma ou mais respostas não estão em conformidade com a política. |
| QAGateRegistrationThisAnswerValidationFailed          | Esta resposta não está em conformidade com a política. |
| QAGateRegistrationTitle                               | Registrar suas respostas |
| QAGateResetNumberOfQuestionsExplanation_Format        | Você deve responder às {0} perguntas a seguir {1} . |
| QAGateResetTitle                                      | Verifique sua identidade: Enviar suas respostas  |


## <a name="custom-logo-banners"></a>Faixas de logotipo personalizadas

A faixa padrão nas páginas do portal pode ser personalizada para sua organização.

Para personalizar a faixa do logotipo:

1. Crie suas faixas personalizadas e salve-as como arquivos .png. Os arquivos devem atender às seguintes recomendações:

   - Tamanho: 490 x 50 pixels.
   - Profundidade de bits: 32 pixels.

2. Copie os arquivos na pasta Personalizações em cada portal que você quer personalizar.

3. Crie um arquivo Style.css em cada pasta. Aponte o arquivo para a pasta personalizações do portal e o novo logotipo. Você pode alterar o nome do logotipo conforme necessário, como `/Customizations/contosologo.png` . O CSS deve ser semelhante ao seguinte código:

   `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4. Se você estiver usando o Internet Explorer 6,0, deverá fornecer um logotipo alternativo não transparente e adicionar o seguinte código ao Style. css:

   `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

   O CSS deve ser semelhante ao seguinte código:
   
   `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`


## <a name="custom-images-for-smartphones"></a>Imagens personalizadas para smartphones

Você pode personalizar a imagem de logotipo para smartphones. 

Para personalizar uma imagem para um smartphone:

1. Crie suas imagens e salve-as como arquivos. png. Os arquivos devem atender às seguintes recomendações:

   - Tamanho: 190 x 50 pixels.
   - Profundidade de bits: 32 pixels.

2. Copie os arquivos na pasta Personalizações em cada portal que você quer personalizar.

3. Crie um arquivo Style.css em cada pasta. Aponte o arquivo para a pasta personalizações do portal e o novo logotipo. Você pode alterar o nome do logotipo conforme necessário, como `/Customizations/contosologo.png` . O CSS deve ser semelhante ao seguinte código:

   ```css
   @media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="custom-style-sheets"></a>Folhas de estilo personalizadas

Você pode modificar o layout e o estilo dos portais de senha usando uma folha de estilo em cascata (CSS) personalizada.

Para usar uma CSS personalizada:

1. Crie seus arquivos CSS personalizados e salve-os como Style.css.

2. Copie os arquivos na pasta Personalizações em cada portal que você quer personalizar.

O código a seguir é um exemplo básico de um arquivo Style. css:

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
>Para que o MIM reconheça as alterações personalizadas, é necessário reiniciar o IIS executando `iisreset` .

O código a seguir é um exemplo mais avançado de um arquivo Style. css. Esse arquivo fornece informações específicas para um smartphone ou um iPad da Apple para exibir os portais nesses dispositivos.

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

A tabela a seguir lista os problemas comuns que podem ocorrer ao atualizar o serviço FIM e o portal do MIM.

| Problema | Resolução |
|---|---|
| Fiz uma personalização da cadeia de caracteres, mas ela não se reflete na interface do usuário.                       | Personalizações de cadeia em cadeias de caracteres. os recursos exigem a reinicialização do IIS executando `iisreset` . |
| Depois de criar cadeias de caracteres. os recursos são alterados, não vejo nenhuma das minhas alterações de cadeia.          | O formato Strings. Resources provavelmente está malformado e pode ser ignorado pelo portal. Verifique o log de eventos em **logs** de  >  **aplicativos e serviços logs** do Windows  >  **Forefront Identity Manager** . |
| Na primeira vez que adicionei Style. CSS, não vi minhas alterações de estilo no Portal.     | Na primeira vez que você introduzir um arquivo Style. CSS, precisará executar `iisreset` . |
| Novos estilos são adicionados ou modificados em Style. CSS, mas as alterações não são vistas no navegador. | Limpe o cache do navegador e atualize a página. Verifique a sintaxe do CSS. |
| Alterei diretamente o conteúdo da pasta CSS `<path_to_sspr_portal>\css\*.css` ou o logotipo de faixa `<path_to_sspr_portal>\images\fimlogo.png` . Perdi essas alterações na atualização. | É recomendável que os usuários não alterem esses arquivos diretamente. Use apenas a pasta personalizações para fornecer um logotipo de faixa e faça apenas personalizações de estilo CSS em Style. css. A pasta personalizações não é substituída deliberadamente por atualizações principais. Não use ferramentas como ILSpy e refletor para alterar cadeias de caracteres nos assemblies do Portal. Use strings.resources para substituir as cadeias de caracteres padrão. Os assemblies são substituídos na atualização.  |
| O logotipo de faixa não é exibido nos portais. Ainda vejo o logotipo do FIM.                  | O nome/caminho da imagem em Style. CSS não é válido ou o cache do navegador não foi limpo.  |
| O logotipo de faixa parece feio no Internet Explorer 6.                                          | Forneça uma imagem não transparente para o Internet Explorer 6 com um estilo correspondente para a imagem em Style. css. |
