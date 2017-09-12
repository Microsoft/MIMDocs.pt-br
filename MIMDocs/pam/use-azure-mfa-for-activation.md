---
title: Usar o Azure MFA para ativar o PAM | Microsoft Docs
description: "Configure o Azure MFA como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: dd77c0135bee40a90f3ea9fd5c1b2771cbc21793
ms.sourcegitcommit: c049dceaf02ab8b6008fe440daae4d07b752ca2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2017
---
# <a name="using-azure-mfa-for-activation"></a>Usando o Azure MFA para ativação

Ao configurar uma função do PAM, você pode escolher como autorizar os usuários que solicitam a ativação da função. As opções que a atividade de autorização de PAM implementa são:

- Aprovação do proprietário da função
- [MFA (Autenticação Multifator) do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Se nenhuma das verificações estiver habilitada, os usuários de candidatos serão ativados automaticamente para sua função.

O MFA (Microsoft Azure Multi-Factor Authentication) é um serviço de autenticação que exige que os usuários verifiquem suas tentativas de entrada usando aplicativo móvel, chamada telefônica ou mensagem de texto. Ele está disponível para uso com o Active Directory do Microsoft Azure e como um serviço para aplicativos corporativos de nuvem e locais. Para o cenário do PAM, o Azure MFA fornece um mecanismo de autenticação adicional. O Azure MFA pode ser usado para autorização, independentemente de como um usuário foi autenticado no domínio PRIV do Windows.

## <a name="prerequisites"></a>Pré-requisitos

Para usar o Azure MFA com MIM, será necessário:

- Acesso à Internet de cada serviço de MIM fornecendo PAM para entrar em contato com o serviço Azure MFA
- Uma assinatura do Azure
- Licenças do Azure Active Directory Premium para usuários candidatos, ou um meio alternativo de licenciar o Azure MFA
- Números de telefone para todos os usuários candidatos

## <a name="creating-an-azure-mfa-provider"></a>Criando um Provedor de Azure MFA

Nesta seção, você configurará o provedor de Azure MFA no Microsoft Azure Active Directory.  Se você já estiver usando o Azure MFA, de modo autônomo ou configurado com o Azure Active Directory Premium, pule para a próxima seção.

1.  Abra um navegador da Web e conecte-se ao [portal clássico do Azure](https://manage.windowsazure.com) como administrador da assinatura do Azure.

2.  No canto inferior esquerdo, clique em **Novo**.

3.  Clique em **Serviços de Aplicativos > Active Directory > Provedor de Multi-Factor Auth > Criação Rápida**.

4.  No campo **Nome** , insira **PAM**e no campo Modelo de Uso, selecione Por Usuário Habilitado. Se você já tem um diretório do AD do Azure, selecione esse diretório. Por fim, clique em **Criar**.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Baixando as Credenciais do Serviço Azure MFA

Em seguida, você gerará um arquivo que inclui o material de autenticação que o PAM usa para entrar em contato com o Azure MFA.

1. Abra um navegador da Web e conecte-se ao [portal clássico do Azure](https://manage.windowsazure.com) como administrador da assinatura do Azure.

2.  Clique em **Active Directory** no menu Portal do Azure e, em seguida, clique na guia **Provedores de Autenticação Multifator** .

3.  Clique no provedor de Azure MFA que você usará para o PAM e, em seguida, clique em **Gerenciar**.

4.  Na nova janela, no painel esquerdo, em **Configurar**, clique em **Configurações**.

5.  Na janela **Azure Multi-Factor Authentication** , clique em **SDK** em **Downloads**.

6.  Clique no link **Baixar** na coluna ZIP do arquivo com a linguagem **SDK para ASP.net 2.0 C#\#**.

![Baixar um SDK do Multi-Factor Authentication – captura de tela](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Copie o arquivo ZIP resultante para cada sistema em que o serviço do MIM está instalado. 

>[!NOTE]
> O arquivo ZIP contém o material para chave usado para autenticar o serviço Azure MFA.

## <a name="configuring-the-mim-service-for-azure-mfa"></a>Configurando o Serviço MIM para o Azure MFA

1.  No computador em que o Serviço MIM está instalado, entre como administrador ou como o usuário que instalou o MIM.

2.  Crie uma nova pasta de diretório no diretório em que o Serviço MIM foi instalado, como ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Usando o Windows Explorer, navegue até a pasta ```pf\certs``` do arquivo ZIP baixado na seção anterior. Copie o arquivo ```cert\_key.p12``` no novo diretório.

4.  Usando o Windows Explorer, navegue até a pasta ```pf``` do ZIP e abra o arquivo ```pf\_auth.cs``` em um editor de texto como o Wordpad.

5. Localize estes três parâmetros: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Copiar valores do arquivo pf\_auth.cs – captura de tela](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Usando o Bloco de Notas, abra **MfaSettings.xml** localizado em ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copie os valores dos parâmetros LICENSE\_KEY, GROUP\_KEY e CERT\_PASSWORD no arquivo pf\_auth.cs em seus respectivos elementos xml no arquivo MfaSettings.xml.

8. No elemento XML **<CertFilePath>**, especifique o nome do caminho completo do arquivo cert\_key.p12 extraído anteriormente.

9. No elemento **<username>**, insira qualquer nome de usuário.

10. No elemento **<DefaultCountryCode>**, insira o código do país para discar para seus usuários, como 1 para os Estados Unidos e Canadá. Esse valor é usado caso usuários sejam registrados com números de telefone que não têm um código de país. Se o número de telefone de um usuário tem um código de país internacional diferente do que é configurado para a organização, esse código de país deve ser incluído no número de telefone que será registrado.

11. Salve e substitua o arquivo **MfaSettings.xml** na pasta ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service``` do Serviço MIM.

> [!NOTE]
> Ao final do processo, verifique se o arquivo **MfaSettings.xml** ou quaisquer cópias dele ou se o arquivo ZIP não podem ser lidos publicamente.

## <a name="configure-pam-users-for-azure-mfa"></a>Configurar os usuários do PAM para o Azure MFA

Para um usuário ativar uma função que requer o Azure MFA, o número de telefone do usuário deve ser armazenado em MIM. Há duas maneiras de definir esse atributo.

Primeiro, o comando `New-PAMUser` copia um atributo de número de telefone da entrada de diretório do usuário no domínio CORP para o banco de dados do Serviço MIM. Observe que essa é uma operação realizada uma única vez.

Em segundo lugar, o comando `Set-PAMUser` atualiza o atributo de número de telefone no banco de dados do Serviço MIM. Por exemplo, o apresentado a seguir substitui o número de telefone do usuário de PAM presente no Serviço MIM. A entrada de diretório deles não é alterada.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Configurar as funções do PAM para o Azure MFA

Depois que todos os usuários candidatos a uma função do PAM tiverem seus números de telefone armazenados no banco de dados do Serviço MIM, a função pode ser configurada para exigir o Azure MFA. Faça isso usando os comandos `New-PAMRole` ou `Set-PAMRole`. Por exemplo,

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Especificando o parâmetro “-MFAEnabled 0” no comando `Set-PAMRole`, é possível desabilitar o Azure MFA para uma função.

## <a name="troubleshooting"></a>Solução de problemas

Os eventos a seguir podem ser encontrados no log de eventos de Privileged Access Management:

| ID  | Severidade | Gerado por | Descrição |
|-----|----------|--------------|-------------|
| 101 | Erro       | Serviço MIM            | Usuário não concluiu a Azure MFA (por exemplo, não respondeu ao telefone) |
| 103 | Informações do | Serviço MIM            | Usuário concluiu a Azure MFA durante a ativação                       |
| 825 | Aviso     | Serviço de Monitoramento do PAM | O número de telefone foi alterado                                |

Para obter mais informações sobre falhas de chamadas telefônicas (evento 101), você também pode exibir ou baixar um relatório do Azure MFA.

1.  Abra um navegador da Web e conecte-se ao [portal clássico do Azure](https://manage.windowsazure.com) como administrador global do Azure AD.

2.  Selecione **Active Directory** no menu do Portal do Azure e a guia **Provedores de Multi-Factor Auth**.

3.  Selecione o provedor de Azure MFA que você usará para o PAM e clique em **Gerenciar**.

4.  Na nova janela, clique em **Uso**e em **Detalhes do Usuário**.

5.  Selecione o intervalo de tempo e marque a caixa ao lado do **Nome** na coluna adicional do relatório. Clique em **Exportar para CSV**.

6.  Assim que o relatório tiver sido gerado, você poderá exibi-lo no portal ou, se o relatório de MFA for extenso, baixe-o em um arquivo CSV. Os valores **SDK** na coluna **AUTH TYPE** indicam linhas que são relevantes como as solicitações de ativação de PAM: essas solicitações são eventos originados do MIM ou de outro software local. O campo **USERNAME** é o GUID do objeto de usuário no banco de dados do serviço do MIM. Se uma chamada não foi bem-sucedida, o valor da coluna **AUTHD** será **No** e o valor da coluna **CALL RESULT** conterá os detalhes do motivo da falha.

## <a name="next-steps"></a>Próximas etapas

- [O que é a Autenticação Multifator do Azure?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Crie hoje mesmo sua conta gratuita do Azure](https://azure.microsoft.com/free/)