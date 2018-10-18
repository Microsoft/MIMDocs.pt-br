---
title: Trabalhar com o relatório híbrido no Azure usando o Microsoft Identity Manager 2016  | Microsoft Docs
description: Saiba como combinar dados locais e na nuvem em relatórios de híbridos no Azure e como gerenciar e exibir esses relatórios.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 18e4127b1d854a53734142bb58442627619491ef
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358458"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabalhar com relatórios híbridos no Identity Manager

Este artigo discute como combinar dados locais e na nuvem em relatórios híbridos no Azure e como gerenciar e exibir esses relatórios.

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os três primeiros relatórios do Microsoft Identity Manager disponíveis no Azure Active Directory (Azure AD) são os seguintes:

- **Atividade de redefinição de senha**: exibe todas as instâncias em que um usuário realizou redefinição de senha usando a redefinição de senha de autoatendimento (SSPR) e fornece as portas ou os métodos usados para autenticação.

- **Registro de redefinição de senha**: exibe todas as vezes que um usuário se registra para a SSPR e os métodos usados para autenticar. Exemplos de métodos podem ser um número de telefone celular ou perguntas e respostas.
   > [!NOTE]
   > Para o *Registro de redefinição de senha*, nenhuma diferenciação é feita entre a porta de SMS e a porta de MFA. Ambas são consideradas métodos de telefone celular.

- **Atividade de grupos de autoatendimento**: exibe cada tentativa feita por alguém de adicionar ou excluir a si próprio de um grupo e criação de grupo.

    ![Relatório híbrido do Azure, imagem de atividade de redefinição de senha](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Os relatórios atualmente apresentam dados de até um mês de atividade.
> * É necessário desinstalar o Agente de Relatório Híbrido anterior.
> * Para desinstalar relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

* Serviço Identity Manager do Identity Manager 2016 SP1, build recomendado [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager).

* Um locatário do Azure AD Premium com um administrador licenciado no seu diretório.

* Conectividade de Internet de saída do servidor do Identity Manager para o Azure.

## <a name="requirements"></a>Requisitos
Os requisitos para usar relatórios híbridos do Identity Manager estão listados na tabela a seguir:


|                                         Requisito                                         |                                                                                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        O relatório híbrido é um recurso do Microsoft Azure AD Premium e requer essa oferta. </br>Para saber mais, confira o artigo [Introdução ao Microsoft Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Obtenha uma [avaliação gratuita de 30 dias do Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Você deve ser um administrador global do seu Azure AD                     |                                                                   Por padrão, apenas administradores globais podem instalar e configurar os agentes para começar a usar, acessar o portal e realizar operações no Microsoft Azure. </br>**Importante:** é necessário usar uma conta corporativa ou de estudante quando instalar os agentes. Não pode ser uma conta da Microsoft. Para saber mais, confira o artigo [Inscrever-se no Azure como uma organização](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| O agente híbrido do Identity Manager é instalado em cada servidor direcionado do Serviço do Identity Manager |                                                                                                                                                                                                       Para receber os dados e fornecer recursos de monitoramento e análise, os relatórios híbridos exigem que os agentes sejam instalados e configurados em servidores direcionados.  </br>                                                                                                                                                                                                       |
|                    Conectividade de saída para os pontos de extremidade de serviço do Azure                     | Durante a instalação e o tempo de execução, o agente requer conectividade com os pontos de extremidade do serviço do Microsoft Azure. Se a conectividade de saída estiver bloqueada por firewalls, não deixe de adicionar os seguintes pontos de extremidade à lista de permissões:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net – Porta: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Conectividade de saída com base em endereços IP                         |                                                                                                                                                                                                                      No caso de endereços IP com base em filtragem de firewalls, veja os [Intervalos de IP do Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 A inspeção de SSL para tráfego de saída foi filtrada ou desabilitada                 |                                                                                                                                                                                                               A etapa de registro do agente ou as operações de carregamento de dados podem falhar se houver inspeção de SSL ou se o tráfego de saída for encerrado na camada de rede.                                                                                                                                                                                                                |
|                      Portas de firewall no servidor que executa o agente                       |                                                                                                                                                                                                          Para se comunicar com os pontos de extremidade do serviço do Azure, o agente requer que as seguintes portas de firewall sejam abertas:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Permitir determinados sites se a segurança aprimorada do Internet Explorer estiver habilitada           |                                                                                Se a segurança aprimorada do Internet Explorer estiver habilitada, os sites a seguir devem ser permitidos no servidor no qual o agente está instalado:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>O servidor de federação da sua organização confiável pelo Azure Active Directory (por exemplo, <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instale o Agente de Relatório do Identity Manager no Azure AD
Depois que o Agente de Relatório é instalado, os dados de atividade do Identity Manager são exportados do Identity Manager para o Log de Eventos do Windows. O Agente de Relatório do Identity Manager processa os eventos e carrega-os para o Azure. No Azure, os eventos são analisados, descriptografados e filtrados para os relatórios necessários.

1.  Instalar o Identity Manager 2016.

2.  Baixe o Agente de Relatório do Identity Manager e, em seguida, faça o seguinte:

    a. Entre no portal de gerenciamento do Azure AD e selecione o **Active Directory**.

    b. Clique duas vezes no diretório do qual você é um administrador global e do qual tem uma assinatura do Azure AD Premium.

    c. Selecione **Configuração** e baixe o Agente de Relatório.

3.  Instale o Agente de Relatório fazendo o seguinte:

    a.  Baixe o [arquivo MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) no servidor do Serviço do Microsoft Identity.

    b.  Execute `MIMHReportingAgentSetup.exe`. 

    c.  Execute o instalador do agente.

    d.  Certifique-se de que o serviço do Agente de Relatório do Identity Manager está em execução.

    e.  Reinicie o serviço do Identity Manager.

4.  Verifique se o Agente de Relatório do Identity Manager está funcionando no Azure.

    Você pode criar dados de relatório usando o portal de redefinição de senha de autoatendimento do Identity Manager para redefinir a senha do usuário. Certifique-se de que a redefinição de senha tenha sido concluída com sucesso e verifique se os dados são exibidos no portal de gerenciamento do AD do Azure.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Exibir relatórios híbridos no portal do Azure

1.  Entre no [Portal do Azure](https://portal.azure.com/) com sua conta de administrador global para o locatário.

2.  Selecione **Azure Active Directory**.

3.  Na lista de diretórios disponíveis para sua assinatura, selecione o diretório do locatário.

4.  Selecione **Logs de Auditoria**.

5.  Na lista suspensa **Categoria**, verifique se **Serviço do MIM** está selecionado.

> [!IMPORTANT]
> Pode levar algum tempo para que os dados de auditoria do Identity Manager sejam exibidos no portal do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar de criar relatórios híbridos
Se você quiser parar de carregar os dados de auditoria de relatório do Identity Manager no Azure AD, desinstale o Agente de Relatórios Híbridos. Use a ferramenta Adicionar ou Remover Programas do Windows para desinstalar o relatório híbrido do Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows usados pelo relatório híbrido
Eventos gerados pelo Gerenciador de Identidade são armazenados no Log de Eventos do Windows. Você pode exibir os eventos no **Visualizador de Eventos** selecionando **Logs de aplicativos e serviços** > **Log de Solicitação do Identity Manager**. Cada solicitação do Identity Manager é exportada como um evento no log de eventos do Windows na estrutura JSON. Você pode exportar o resultados para seu sistema de gerenciamento de eventos e informações de segurança (SIEM).

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações do|4121|Os dados de evento do Identity Manager que incluem todos os dados da solicitação.|
|Informações do|4137|Extensão 4121 de evento do Identity Manager, no caso de haver dados demais para um único evento. O cabeçalho neste evento é exibido no seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`.|
