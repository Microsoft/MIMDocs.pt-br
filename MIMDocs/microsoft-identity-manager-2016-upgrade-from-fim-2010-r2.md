---
title: Atualizar do FIM 2010 R2 para o Microsoft Identity Manager 2016 | Microsoft Docs
description: Saiba como atualizar os componentes do FIM 2010 R2 e, em seguida, instale os componentes que são novos no MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7df1a4ca7a33aa2a00ac0b2c3ef24056c9720b5b
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043232"
---
# <a name="upgrade-from-forefront-identity-manager-2010-r2"></a>Atualização do Forefront Identity Manager 2010 R2

Se você tiver um ambiente do FIM (Forefront Identity Manager) 2010 R2 e quiser testar o MIM (Microsoft Identity Manager) 2016, use este artigo como seu guia. Há três fases nessa atualização:

1.  Instale o Serviço de Sincronização (Sync) do MIM em um servidor que esteja ingressado no domínio do seu AD (Active Directory). Isso substitui a instância do FIM 2010 R2 da sincronização.

2.  Instale o Serviço e o Portal do MIM. Neste ponto, também é possível instalar o portal de serviço e o portal de registro para SSPR (redefinição de senha por autoatendimento). E o conjunto de recursos do Privileged Access Management será instalado.

3.  Implante os complementos e extensões do MIM em um computador cliente separado. Isso inclui o cliente integrado de Logon do Windows por SSPR.


Este guia pressupõe que você tenha o seguinte já configurado:
- O FIM 2010 R2 implantado em um ambiente de teste
- Servidores em execução no Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2008 R2
- Pré-requisitos locais e de ambiente (SQL Server, Exchange Server, SharePoint Services etc.) configurados para o FIM 2010 R2.


## <a name="preparation"></a>Preparação

1.  Faça backup do banco de dados de serviço do FIM, do banco de dados de sincronização do FIM e do software e configuração do serviço de sincronização do FIM.

2.  Em cada servidor em que os componentes do FIM 2010 R2 estejam instalados – por exemplo, *CORPIDM*, faça logon como Contoso\Administrador. Neste exemplo de implantação, os direitos administrativos são necessários para atualizar o FIM 2010 R2 para **MIM**.

3.  Baixe ou desempacote o software MIM.  Caso não tenha este software, acesse [Downloads e licenciamento do Microsoft Identity Manager](microsoft-identity-manager-licensing.md).

## <a name="upgrade-the-synchronization-service"></a>Atualize o Serviço de Sincronização

1.  Faça logon como administrador para um servidor em que o Serviço de Sincronização do FIM 2010 R2 ("sincronização") esteja implantado.

2.  Certifique-se de fazer backup de seu banco de dados antes de iniciar este procedimento.

3.  Abra o console **Serviços** , localize o **Serviço de Sincronização do Forefront Identity Manager**e interrompa-o.

    ![Imagem do console de serviços](media/MIM-UpgFIM1.PNG)

4.  Execute o **Instalador do Serviço de Sincronização do MIM**. O instalador vai detectar a versão de sincronização existente e sugerir uma atualização. Clique no botão **Atualizar** para continuar.

5.  Se você aceitar os termos de licença, clique em **Avançar** para continuar.

6.  Digite a senha da conta de serviço que a sincronização está usando e clique em **Avançar**.

    ![Configure a imagem da conta de serviço da Sincronização do MIM](media/MIM-UpgFIM3.png)

7.  Valide que os nomes do grupo de segurança estão corretos e clique em **Avançar**.

    ![Configure a imagem dos grupos de segurança da Sincronização do MIM](media/MIM-UpgFIM4.png)

8.  Deixe a caixa de seleção para regras de firewall para comunicações de entrada do RPC inalterado.

9. O instalador está pronto para atualizar a sincronização do FIM 2010 R2 para o MIM. Clique em **Atualizar** para iniciar o processo de atualização.

10. A atualização agora está em andamento. Não saia do instalador nem reinicie o computador quando a atualização estiver em andamento.

    ![Imagem de status de instalação de Sincronização do MIM](media/MIM-UpgFIM7.png)

11. Durante a atualização, é mostrado um aviso sobre a atualização do banco de dados de sincronização. É recomendável fazer backup do banco de dados antes de esse processo iniciar.

12. Quando a atualização for concluída com sucesso, clique em **Concluir**.

    ![Imagem de instalação bem-sucedida do Sincronização do MIM](media/MIM-UpgSP1.png)

13. Observe que o **Serviço de Sincronização** reiniciou.

## <a name="upgrade-the-service-and-portal"></a>Atualização do Serviço e do Portal

1.  Faça logon como um administrador em um servidor em que o Serviço e o Portal do FIM 2010 R2 e estejam implantados.

2.  Abra o console **Serviços** , localize **Forefront Identity Manager Service**e interrompa-o.

    ![Imagem do console de serviços](media/MIM-UpgFIM9.PNG)

3.  Execute o instalador do Serviço e Portal do MIM. Clique no botão **Instalar** para continuar.

    ![Instale a imagem do Serviço e Portal do MIM](media/MIM-UpgSP2.png)

4.  Se você aceitar os termos de licença, clique em **Avançar** para continuar.

5.  Na tela do Programa de Aperfeiçoamento da Experiência do Usuário do MIM, clique em **Avançar** para continuar.

6.  Selecione os recursos e componentes do MIM que deseja instalar e clique em **Avançar** quando terminar.

    ![Imagem de configuração personalizada](media/MIM-UpgSP4.png)

    1.  **Serviço do MIM:** esse recurso é obrigatório em pelo menos um servidor e exige que um servidor de banco de dados do SQL Server esteja posicionado ou em outro servidor.

    2.  **Portal do MIM:** esse recurso é obrigatório em pelo menos um servidor e requer o SharePoint 2013 Foundation.

    3.  **Portal de Registro de Senha do MIM:** esse recurso é necessário para a redefinição de senha de autoatendimento.

    4.  **Portal de Redefinição de Senha do MIM:** esse recurso é necessário para redefinição de senha.

7.  Forneça detalhes do SQL Server que está sendo usado para o banco de dados do serviço FIM. Selecione a opção para usar novamente o banco de dados existente e preservar os dados. Clique em **Próximo** para continuar.

8. Se a opção de usar novamente o banco de dados existente foi selecionada, aparecerá um lembrete para fazer backup do banco de dados.

9. Insira os detalhes do servidor de email. Se o servidor de email estiver localizado no servidor atual, digite "localhost" como o local do servidor de email. Clique em **Próximo** para continuar.

    ![Configurar a imagem de conexão do servidor de email](media/MIM-UpgSP6.png)

10. Selecione um certificado para o serviço a ser usado para validar os clientes. Você deve usar o certificado existente do repositório de certificados local que era usado anteriormente pelo Serviço do FIM.

    ![Configure a imagem do certificado de serviço](media/MIM-UpgSP7.png)

    1.  Se a opção de repositório de certificados local estiver selecionada, clique no botão **Selecionar Cert** e selecione um certificado da lista na janela pop-up. Clique em **OK** e, em seguida, em **Avançar**.

        ![Selecione uma imagem da janela pop-up Certificado](media/MIM-UpgSP8.PNG)

11. Configure as credenciais da conta de serviço para o Serviço do MIM. Observe que a conta de serviço não pode ser a mesma conta de serviço usada pelo serviço de sincronização. Ela deve ser a mesma conta usada pelo serviço do FIM.

    ![Configurar a imagem da conta de serviço do MIM](media/MIM-UpgSP9.png)

12. Configure os detalhes do servidor de sincronização do MIM de acordo com a implantação do Serviço do MIM configurada na etapa anterior.

    ![Configure a imagem de sincronização do Serviço e Portal do MIM](media/MIM-UpgSP10.png)

13. Ao instalar o Portal do MIM, forneça o endereço do servidor do Serviço do MIM. Clique em **Avançar**.

14. Ao instalar o Portal do MIM, forneça a URL do conjunto de sites do SharePoint no qual o Portal do FIM está hospedado no momento. Clique em **Avançar**.

## <a name="install-the-mim-password-registration-portal"></a>Instalação do Portal de Registro e Redefinição de Senha do MIM

1. Se você estiver instalando o Portal de Registro de Senha do MIM, forneça a URL solicitada para o Portal de Registro de Senha. Clique em **Avançar**.

2. Configure a capacidade de clientes e usuários finais de usar o Serviço e o Portal.

    1.  Verifique se você deseja **Abrir as portas 5725 e 5726 no firewall**.

    2.  Verifique se você deseja **Conceder aos usuários acesso autenticado ao site do Portal do MIM**.

    3.  Clique em **Avançar**.

3. Forneça detalhes de acesso e credenciais para o registro de senha do MIM.

    ![Configure a imagem do Portal de Registro de Senha do MIM](media/MIM-UpgSP15.png)

    1.  Forneça o nome da conta de serviço (inclusive domínio) e a senha para o Registro da Senha do MIM.

    2.  Forneça os detalhes do host – nome e porta (por exemplo, 8080) – do Portal de Registro de Senha.

    3.  Marque a opção **abrir porta no firewall** .

    4.  Clique em **Avançar**.

4. Na próxima tela de configuração do Registro de Senha do MIM:

    1.  Informe o Registro de senha do MIM em que o serviço do MIM está instalado, normalmente o mesmo sistema.

    2.  Determine se este portal pode ser acessado por usuários de extranet e intranet ou apenas por usuários da intranet, como foi configurado anteriormente para redefinição da senha do FIM.

## <a name="install-the-mim-password-reset-portal"></a>Instalação do Portal de Redefinição de Senha do MIM

1. Se você estiver instalando o Portal de redefinição de senha do MIM, forneça detalhes de acesso e credenciais para a redefinição de senha do MIM.

    ![Configure a imagem do Portal de Redefinição de Senha do MIM](media/MIM-UpgSP17.png)

    1.  Forneça o nome da conta de serviço (inclusive domínio) e a senha para redefinição da senha do MIM.

    2.  Forneça os detalhes do host – nome e porta (por exemplo, 8088) – do Portal de Redefinição de Senha.

    3.  Marque a opção **abrir porta no firewall** .

    4.  Clique em **Avançar**.

2. Na próxima tela de configuração de Redefinição de Senha do MIM:

    1.  Informe a Redefinição de Senha do MIM em que o Serviço do MIM está instalado.

    2.  Especifique se este portal pode ser acessado por usuários da extranet e da intranet, ou apenas por usuários da intranet.

## <a name="finish-installation-and-upgrade"></a>Conclusão da instalação e da atualização

1. Depois que todas as definições de configuração forem concluídas com sucesso, a página de instalação será exibida. Clique em **Instalar** para iniciar a instalação e atualização do Serviço e do Portal do MIM.

2. A instalação e a atualização do Serviço e do Portal do MIM estão em andamento. Não cancele o instalador nem reinicie o computador durante a instalação.

3. Após a instalação (atualização) do Serviço e do Portal do MIM ser concluída com sucesso, a tela de confirmação será exibida. Clique em **Concluir** para concluir a instalação e sair do instalador.

4. Observe que o **Forefront Identity Manager Service** reiniciou.

Observação: Se os complementos e extensões do FIM estão implantadas no momento nos computadores dos usuários para SSPR, não configure a nova porta de telefone MFA para redefinição de senha até que todos os complementos e extensões do FIM tenham sido atualizados para o MIM 2016.  Como os complementos e extensões do FIM 2010 e FIM 2010 R2 não reconhecem as novas portas, eles fornecerão um erro e o usuário não poderá concluir a redefinição de senha.

Para obter orientação sobre atualização do Microsoft Identity Manager 2016 SP1, consulte o seguinte [pacote de atualização do Microsoft Identity Manager 2016 Service Pack 1](https://blogs.technet.microsoft.com/iamsupport/2016/11/08/microsoft-identity-manager-2016-service-pack-1-update-package/)
