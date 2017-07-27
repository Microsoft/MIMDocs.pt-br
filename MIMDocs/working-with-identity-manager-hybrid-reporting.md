---
title: "Trabalhar com Relatórios híbridos no Azure usando o MIM 2016 | Microsoft Docs"
description: "Saiba como combinar dados locais e na nuvem em relatórios de híbridos no Azure e como gerenciar e exibir esses relatórios."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>Trabalhando com o relatório híbrido do Microsoft Identity Manager – Visualização Pública (atualização)

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os três primeiros relatórios do MIM (Microsoft Identity Manager) disponíveis no Azure AD são **Atividade de redefinição de senha**, **Registro de redefinição de senha** e **Atividade de grupos de autoatendimento**.

-   A atividade de redefinição de senha é exibida em todas as instâncias em que um usuário realizou redefinição de senha usando o SSPR e fornece as portas ou os **Métodos** usados para autenticação.

-   O registro de redefinição de senha é exibido sempre que um usuário registra-se para SSPR e os **Métodos** usados para autenticar, por exemplo um número de telefone celular ou perguntas e respostas.
    Observe que, para registro de redefinição de senha, nenhuma diferenciação é feita entre porta de SMS e porta de MFA – ambas são consideradas **Telefone Celular**.

-   A atividade de grupos de autoatendimento é exibida a cada tentativa feita por alguém de adicionar ou excluir a si próprio de um grupo e criação de grupo.

    ![Relatório híbrido do Azure, imagem de atividade de redefinição de senha](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Os relatórios atualmente apresentam dados de até um mês atrás.</br>
> É necessário desinstalar o agente híbrido anterior</br>
> Se desejar desinstalar relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

1.  Instale o Microsoft Identity Manager 2016 RTM /ou o serviço SP1 MIM.

2.  Verifique se você tem um locatário Azure AD Premium com um administrador licenciado no seu diretório.

3.  Verifique se que você tem conectividade de Internet de saída do servidor do Microsoft Identity Manager para o Azure.

## <a name="requirements"></a>Requisitos
A tabela a seguir descreve uma lista de requisitos destinada ao uso de relatórios híbridos do Microsoft Identity Manager.

| Requisito | Descrição |
| --- | --- |
| Azure AD Premium | O relatório híbrido é um recurso do Microsoft Azure AD Premium e requer essa oferta. </br></br>Para saber mais, confira o artigo [Introdução ao Microsoft Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium) </br>Para iniciar uma avaliação gratuita de 30 dias, confira [Iniciar uma avaliação.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Você deve ser um administrador global do Microsoft Azure AD para começar a usar |Por padrão, apenas os administradores globais podem instalar e configurar os agentes para começar a usar, acessar o portal e realizar operações no Microsoft Azure. </br></br>**Importante:** é necessário usar uma conta corporativa ou de estudante quando instalar os agentes. Não pode ser uma conta da Microsoft. Para saber mais, confira o artigo [Inscrever-se no Azure como uma organização](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) |
| O agente híbrido do Microsoft Identity Manager é instalado em cada servidor direcionado do Serviço do MIM | Os relatórios híbridos exigem que os agentes sejam instalados e configurados em servidores direcionados para receber dados e fornecer recursos de análise e monitoramento </br>|
| Conectividade de saída para os pontos de extremidade de serviço do Azure | Durante a instalação e o tempo de execução, o agente requer conectividade com os pontos de extremidade do serviço do Microsoft Azure. Se a conectividade de saída estiver bloqueada por firewalls, não deixe de adicionar os seguintes pontos de extremidade à lista de permissão: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net – Porta: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Conectividade de saída com base em endereços IP | No caso de endereços IP com base em filtragem de firewalls, veja os [Intervalos de IP do Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| A inspeção de SSL para tráfego de saída foi filtrada ou desabilitada | A etapa de registro do agente ou as operações de carregamento de dados podem falhar se houver inspeção de SSL ou se o tráfego de saída for encerrado na camada de rede. |
| Portas de firewall no servidor que executa o agente. |O agente requer que as seguintes portas de firewall sejam abertas para que o agente se comunique com os pontos de extremidade do serviço do Microsoft Azure.</br></br><li>Porta TCP 443</li><li>Porta TCP 5671</li> |
| Permitir os seguintes sites, se a segurança reforçada do IE estiver habilitada |Quando a Segurança Aprimorada do IE está habilitada, os sites a seguir devem ser permitidos no servidor no qual o agente será instalado.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>O servidor de federação da organização confiável para o Azure Active Directory. Por exemplo: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Instale o Agente de Relatório do Microsoft Identity Manager no Azure AD
Depois que o agente de relatório é instalado, os dados de atividade do Microsoft Identity Manager são exportados do MIM para o log de eventos do Windows. O agente de relatório do MIM processa os eventos e os carrega no Azure. No Azure, os eventos são analisados, descriptografados e filtrados para os relatórios necessários.

1.  Install Microsoft Identity Manager 2016.

2.  Baixe os agentes de relatório do Microsoft Identity Manager:

    1.  Faça logon no portal de gerenciamento do AD do Azure e clique no ícone do Active Directory.

    2.  Clique duas vezes no diretório do qual você é um Administrador Global e do qual tem uma assinatura do Azure AD Premium.

    3.  Clique em **Configuração** e baixe o agente de relatório.

3.  Instale o agente de emissão de relatórios do Microsoft Identity Manager:

    1.  Baixe o [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) no servidor de serviço do Microsoft Identity Manager.
    2.  Execute `MIMHReportingAgentSetup.exe` 
    3.  Execute o instalador do agente.

    4.  Certifique-se de que o serviço do agente de relatório do MIM está funcionando

    5.  Reinicie o serviço MIM.

4.  Valide se o relatório do Microsoft Identity Manager está funcionando no Azure.

    Você pode criar dados de relatório usando o portal de redefinição de senha de autoatendimento do Microsoft Identity Manager para redefinir a senha do usuário. Certifique-se de que a redefinição de senha seja concluída com sucesso e, em seguida, verifique se os dados são exibidos no portal de gerenciamento do AD do Azure.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Exibir relatórios híbridos no Portal do Azure

1.  Faça logon no [Portal do Azure](https://portal.azure.com/) com sua conta do administrador global para o locatário.

2.  Clique no ícone **Azure Active Directory**.

3.  Selecione o diretório do locatário da lista de diretórios disponíveis para sua assinatura.

4.  Clique em **Logs de Auditoria**.

5.  Verifique se você selecionou o **Serviço MIM** no menu suspenso Categoria.

> [!WARNING]
> Pode levar algum tempo para que os dados de auditoria do Microsoft Identity Manager sejam exibidos no Portal do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar de criar relatórios híbridos
Se você quiser parar de carregar os dados de auditoria de relatório do Microsoft Identity Manager no Azure Active Directory, desinstale o agente de relatórios híbridos. Use a ferramenta **Adicionar ou Remover Programas** do Windows para desinstalar o relatório híbrido do Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows usados pelo relatório híbrido
Eventos gerados pelo Microsoft Identity Manager são registrados no Log de Eventos do Windows e ficam visíveis no Visualizador de Eventos em: logs de Aplicativos e Serviços-&gt; **Log de Solicitação do Identity Manager**. Cada solicitação do MIM é exportada como um evento no log de eventos do Windows na estrutura JSON. Isso pode ser exportado para o SIEM.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações do|4121|Dados de evento do MIM que incluem todos os dados da solicitação.|
|Informações do|4137|Extensão 4121 de evento do MIM, no caso de haver dados demais para um único evento. O cabeçalho neste evento está na seguinte forma: `"Request: <GUID> , message <xxx> out of <xxx>`|
