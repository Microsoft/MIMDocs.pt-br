---
title: "Trabalhar com Relatórios híbridos no Azure usando o MIM 2016 | Microsoft Docs"
description: "Saiba como combinar dados locais e na nuvem em relatórios de híbridos no Azure e como gerenciar e exibir esses relatórios."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 9e64f930a8fe8422c7f6c8d98e558961ae8b88f2


---

# <a name="working-with-identity-manager-hybrid-reporting"></a>Trabalhando com o Relatório híbrido do Identity Manager

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os três primeiros relatórios do MIM (Microsoft Identity Manager) disponíveis no Azure AD são **Atividade de redefinição de senha**, **Registro de redefinição de senha** e **Atividade de grupos de autoatendimento**.

-   A atividade de redefinição de senha é exibida em todas as instâncias em que um usuário realizou redefinição de senha usando o SSPR e fornece as portas ou os **Métodos** usados para autenticação.

    ![Relatório híbrido do Azure, imagem de atividade de redefinição de senha](media/MIM-Hybrid-passwordreset.jpg)

-   O registro de redefinição de senha é exibido sempre que um usuário registra-se para SSPR e os **Métodos** usados para autenticar, por exemplo um número de telefone celular ou perguntas e respostas.
    Observe que, para registro de redefinição de senha, nenhuma diferenciação é feita entre porta de SMS e porta de MFA – ambas são consideradas **Telefone Celular**.

-   A atividade de grupos de autoatendimento é exibida a cada tentativa feita por alguém de adicionar ou excluir a si próprio de um grupo e criação de grupo.

> [!NOTE]
> Os relatórios atualmente apresentam dados de até um mês atrás.
>
> Se desejar desinstalar relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

1.  Instale o Microsoft Identity Manager 2016 incluindo o serviço MIM.

2.  Verifique se você tem um locatário Azure AD Premium com um administrador licenciado no seu diretório.

3.  Verifique se que você tem conectividade de Internet de saída do servidor do Microsoft Identity Manager para o Azure.

## <a name="install-microsoft-identity-manager-reporting-in-azure-ad"></a>Instale o Relatório do Microsoft Identity Manager no Azure AD
Depois que o agente de relatório é instalado, os dados de atividade do Microsoft Identity Manager são exportados do MIM para o log de eventos do Windows. O agente de relatório do MIM processa os eventos e os carrega no Azure. No Azure, os eventos são analisados, descriptografados e filtrados para os relatórios necessários.

1.  Install Microsoft Identity Manager 2016.

2.  Baixe os agentes de relatório do Microsoft Identity Manager:

    1.  Faça logon no portal de gerenciamento do AD do Azure e clique no ícone do Active Directory.

    2.  Clique duas vezes no diretório do qual você é um Administrador Global e do qual tem uma assinatura do Azure AD Premium.

    3.  Clique em **Configuração** e baixe o agente de relatório.

3.  Instale o agente de emissão de relatórios do Microsoft Identity Manager:

    1.  Criar um diretório no computador.

    2.  Extraia os arquivos `MIMHybridReportingAgent.msi` e `tenant.cert` para o diretório.

    3.  Execute o instalador do agente.

    4.  Certifique-se de que o serviço do agente de relatório do MIM está funcionando

    5.  Reinicie o serviço MIM.

4.  Valide se o relatório do Microsoft Identity Manager está funcionando no Azure.

    Você pode criar dados de relatório usando o portal de redefinição de senha de autoatendimento do Microsoft Identity Manager para redefinir a senha do usuário. Certifique-se de que a redefinição de senha seja concluída com sucesso e, em seguida, verifique se os dados são exibidos no portal de gerenciamento do AD do Azure.

## <a name="view-hybrid-reports-in-the-azure-classic-portal"></a>Exibição dos relatórios híbridos no portal clássico do Azure

1.  Faça logon no [portal clássico do Azure](https://manage.windowsazure.com/) com sua conta de administrador global para o locatário.

2.  Clique no ícone **Active Directory** .

3.  Selecione o diretório do locatário da lista de diretórios disponíveis para sua assinatura.

4.  Clique em **Relatórios** e então em **Atividade de Redefinição de Senha**.

5.  Selecione o **Identity Manager** no menu suspenso de origem.

> [!WARNING]
> Pode levar algum tempo para que os dados do Microsoft Identity Manager sejam exibidos no AD do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar de criar relatórios híbridos
Se você quiser parar de carregar dados de relatório do Microsoft Identity Manager no Azure Active Directory, desinstale o agente de relatórios híbridos. Use a ferramenta **Adicionar ou Remover Programas** do Windows para desinstalar o relatório híbrido do Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows usados pelo relatório híbrido
Eventos gerados pelo Microsoft Identity Manager são registrados no Log de Eventos do Windows e ficam visíveis no Visualizador de Eventos em: logs de Aplicativos e Serviços-&gt; **Log de Solicitação do Identity Manager**. Cada solicitação do MIM é exportada como um evento no log de eventos do Windows na estrutura JSON. Isso pode ser exportado para o SIEM.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações do|4121|Dados de evento do MIM que incluem todos os dados da solicitação.|
|Informações do|4137|Extensão 4121 de evento do MIM, no caso de haver dados demais para um único evento. O cabeçalho neste evento está na seguinte forma: `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jan17_HO4-->


