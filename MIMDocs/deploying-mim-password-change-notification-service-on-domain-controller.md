---
title: Implantar o serviço de notificação de alteração de senha | Microsoft Docs
description: Obter as etapas para instalar e configurar o Serviço de notificação de alteração de senha do MIM em seu controlador de domínio.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f195a6db259bf0fefabcd05c8890ca65c9624314
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041957"
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>Implante o Serviço de notificação de alteração de senha do MIM em um controlador de domínio

## <a name="install-the-password-change-notification-service"></a>Instale o Serviço de notificação de alteração de senha
O PCNS (Serviço de notificação de alteração de senha) é um serviço que você instala nos controladores de domínio que permite a sincronização de senhas pelo MIM com outros sistemas, como o servidor de diretório de outro fornecedor. Para a sincronização de senha, instale o PCNS em cada servidor do controlador de domínio.

1.  Faça logon como administrador de domínio para um servidor em execução no Windows Server com a função dos Serviços de Domínio do Active Directory.

2.  Copie a pasta de instalação do Serviço de Notificação de Alteração de Senha para o computador.

3.  Localize o arquivo *Password Change Notification Service.msi* , clique com o botão direito do mouse nele e crie um atalho.

4.  Localize o arquivo de atalho, clique com o botão direito do mouse e exiba suas **Propriedades**.

5.  No campo Destino, adicione o preâmbulo *msiexec.exe /i* antes do caminho para o arquivo msi e o sufixo *SCHEMAONLY=TRUE* depois do caminho do msi. Por exemplo, se a pasta de instalação for *C:\PCNS*, o comando para executar seria assim: (tudo em uma linha).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Salve as alterações no arquivo de atalho.

7.  Execute o arquivo de atalho para iniciar a instalação do PCNS no modo de extensão do esquema. Quando aparecer a tela a seguir, clique em **Avançar**.

8.  Você será notificado de que a instalação agora atualizará o esquema do Active Directory para o Serviço de Notificação de Alteração de Senha. Clique em **OK** para prosseguir com a atualização do esquema.

9. Quando o processo de extensão do esquema for concluído e a tela a seguir aparecer, clique em **Concluir**.

10. Execute o arquivo *Password Change Notification Service.msi* novamente – desta vez, diretamente (não é necessária cadeia de caracteres de execução).  Quando aparecer a tela a seguir, clique em **Avançar**.

11. Aceite o contrato de licença e clique em **Avançar**.

12. Clique para iniciar a instalação.

13. Quando a tela de instalação for concluída com sucesso aparecer, clique em **Concluir**.

14. Reinicie o computador para que as alterações feitas ao Serviço de Notificação de Alteração de Senha do MIM entrem em vigor. Você pode fazer isso clicando em **Sim** na janela pop-up que aparece ou pode reiniciar mais tarde.

## <a name="configuring-the-password-change-notification-service"></a>Configuração do Serviço de notificação de alteração de senha
Uma vez reconectado ao servidor de DC como um administrador de domínio, vá para *C:\Program Files\Microsoft Password Change Notification.* Execute *pcnscfg.exe*.
