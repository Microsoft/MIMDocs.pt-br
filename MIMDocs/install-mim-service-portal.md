---
title: Instalar o portal e o serviço do Microsoft Identity Manager | Microsoft Docs
description: Obtenha as etapas para configurar e instalar o Serviço e o Portal do MIM para o Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 4604c17d3e58c57f9819aaa036dc12a669aed55d
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78289475"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Instalação do MIM 2016: Serviço e Portal do MIM

> [!div class="step-by-step"]
> [« Serviço de Sincronização do MIM](install-mim-sync.md)
> [Sincronizar bancos de dados »](install-mim-sync-ad-service.md)
 
> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **mimservername**
> - Nome do domínio - **contoso**
> - Senha – <strong>Pass@word1</strong>
> - Nome da conta de serviço: **MIMService**

Se você não configurou o pacote de instalação do MIM na última etapa, volte e instale os componentes do Microsoft Identity Manager 2016 antes de continuar.


## <a name="configure-mim-service-and-portal-for-installation"></a>Configure o Serviço e o Portal do MIM para instalação

1. Execute o **instalador de Serviço e Portal do MIM** na subpasta **Serviço e Portal** descompactada.

2. Na tela de boas-vindas, clique em **Avançar**.

3. Leia o Contrato de Licença do Usuário Final e clique em **Avançar** se você aceitar os termos de licença.

4. Na tela do **Programa de Aperfeiçoamento da Experiência do Usuário do MIM**, clique em **Avançar**.

5. Ao selecionar os recursos do componente para essa implantação, é necessário incluir o Serviço do MIM (exceto o Relatório do MIM) e os recursos do Portal do MIM. Você também pode selecionar o Portal de Redefinição de Senha do MIM e o Serviço de Notificação de Alteração de Senha do MIM.

6. Na página **Configurar a conexão de banco de dados do MIM**, escolha **Criar um novo banco de dados**.

    ![Configure a imagem de conexão de banco de dados do MIM](media/install-mim-service-portal/MIM_Install10.png)

7. Em **Configurar conexão do servidor de email**, insira o nome do seu servidor do Exchange como **Servidor de Email**, ou use a Caixa de **Correio do O365**. Se você não tiver um servidor de email configurado, use **localhost** como o nome do servidor de email e desmarque as duas caixas de seleção superiores. Clique em **Avançar**.
    >[!NOTE]
    >MIM 2016 SP2 e posterior: se você estiver usando contas de serviço gerenciado de grupo, marque a caixa de seleção **Usar usuário diferente para o Exchange** mesmo que não planeje usar o Exchange.
    
    >[!NOTE]
    >Quando a opção **Usar o Exchange Online** está selecionada, para habilitar o Serviço do MIM para processar respostas de aprovação do complemento do Outlook do MIM, você precisa definir o valor da chave do Registro HKLM\SYSTEM\CurrentControlSet\Services\FIMService de PollExchangeEnabled como 1 após a instalação.

    ![Configurar a imagem de conexão do servidor de email](media/install-mim-service-portal/MIM_Install11.png)

8. Especifique se deseja gerar um novo certificado autoassinado ou selecione o certificado apropriado.

9. Especifique o nome da Conta de Serviço a ser usada, por exemplo, *MIMService*, a senha da Conta de Serviço, por exemplo, <em>Pass@word1</em>, seu domínio da Conta de Serviço, por exemplo, *contoso* e a Conta de Email de Serviço, por exemplo, *contoso*.
    >[!NOTE]
    >MIM 2016 SP2 e posterior: se você estiver usando as Contas de Serviço Gerenciado de Grupo, será necessário garantir que o caractere **$** esteja no final do nome da conta de serviço, por exemplo, MIMService$, e deixar em branco o campo de Senha da Conta de Serviço.

    ![Configurar a imagem da conta de serviço do MIM](media/install-mim-service-portal/MIM_Install12.png)

10. Observe que pode aparecer um aviso dizendo que a conta de serviço não é segura na sua configuração atual.

11. Aceite os padrões para o local do servidor de sincronização e especifique a conta do Agente de Gerenciamento do MIM como *contoso\MIMMA*.
    >[!NOTE]
    >MIM 2016 SP2 e posterior: se você planeja usar a Conta de Serviço Gerenciado do Grupo de Serviço de Sincronização do MIM na sincronização do MIM e habilitar o recurso "Usar conta de sincronização do MIM", insira o nome gMSA do Serviço de Sincronização do MIM como a conta do MA do MIM, por exemplo, *contoso\MIMSync$* .

    ![Configuração da imagem do Serviço e Portal do MIM](media/install-mim-service-portal/MIM_Install13.png)

12. Especifique *CORPIDM* (nome deste computador) como o endereço do Servidor do Serviço do MIM para o Portal do MIM.

13. Especifique `http://mim.contoso.com` como a URL do conjunto de sites do SharePoint.

14. Especifique `http://passwordregistration.contoso.com` como a porta 80 da URL de Registro de Senha. Recomendamos que você atualize mais tarde com o certificado SSL em 443.

15. Especifique `http://passwordreset.contoso.com` como a porta 80 da URL de Redefinição de Senha. Recomendamos que você atualize mais tarde com o certificado SSL em 443.

16. Marque a caixa de seleção para abrir as portas 5725 e 5726 no firewall e a caixa de seleção para conceder acesso ao Portal do MIM a todos os usuários autenticados.

## <a name="configure-mim-password-registration-portal"></a>Configuração da imagem do Portal de Registro de Senha do MIM

1. Configure o nome da conta de serviço para o registro SSPR como *contoso\MIMSSPR* e sua senha como <em>Pass@word1</em>.

2. Especifique *passwordregistration.contoso.com* como o Nome de Host para o Registro de Senha do MIM e defina a porta como **80**. Habilite a opção **Abrir porta no firewall**.

   ![Insira as informações de configuração usadas pela imagem do IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Um aviso será exibido. Leia-o e clique em **Avançar**.

4. Na próxima tela de configuração do Portal de Registro de Senha do MIM, especifique *mim.contoso.com* como o Endereço do Servidor do Serviço de MIM para o Portal de Registro de Senha.

## <a name="configure-mim-password-reset-portal"></a>Configuração do Portal de Redefinição de Senha do MIM

1. Configure o nome da conta de serviço para o registro SSPR como *Contoso\MIMSSPR* e sua senha como <em>Pass@word1</em>.

2. Especifique *passwordreset.contoso.com* como o Nome de Host do Portal de Redefinição de Senha do MIM e defina a porta como **80**. Habilite a opção **Abrir porta no firewall**.

   ![Insira as informações de configuração usadas pela imagem do IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Um aviso será exibido. Leia-o e clique em **Avançar**.

4. Na próxima tela de configuração do Portal de Registro de Senha do MIM, especifique *mim.contoso.com* como o Endereço do Servidor do Serviço de MIM para o Portal de Redefinição de Senha.

## <a name="install-mim-service-and-portal"></a>Instalar o portal e o serviço do MIM

Quando todas as definições de pré-instalação estiverem prontas, clique em **Instalar** para começar a instalação dos componentes de **Serviço e Portal** selecionados.

Após a conclusão da instalação, verifique se o Portal do MIM está ativo.

1. Inicie o Internet Explorer e conecte-se ao Portal do MIM em *http://mim.contoso.com/identitymanagement* . Observe que pode haver um pequeno atraso na primeira visita a essa página.
    - Se for necessário, autentique-se como *contoso\miminstall* no Internet Explorer.

2. No Internet Explorer, abra **Opções da Internet**altere para a guia **Segurança** e adicione o site à zona da **Intranet Local** se ainda não estiver lá.  Feche a caixa de diálogo **Opções da Internet** .

3. Permita que os usuários exibam suas próprias entradas no MIM.

    1.  Usando o Internet Explorer, no **Portal do MIM**, clique em **Regras de da Política de Gerenciamento**.

    2.  Procure a regra da política de gerenciamento, **Gerenciamento de usuário: os usuários podem ler atributos próprios**.

    3.  Selecione essa regra de política de gerenciamento, desmarque **Política está desabilitada**.

    4.  Clique em **OK** e em **Enviar**.

4.  Verifique se o firewall permite conexões de entrada para as portas TCP 5725 e 5726.

    1.  Inicie **Ferramentas Administrativas » Firewall do Windows** com **Segurança Avançada**.

    2.  Clique em **Regras de entrada**.

    3.  Verifique se as duas regras a seguir estão listadas:

    -   STS (Serviço do Identity Manager) do Forefront.
    -   Serviço do Identity Manager do Forefront (Webservice).

    4.  Conclua o assistente e feche o aplicativo **Firewall do Windows** .

    5.  Inicie **Painel de controle » Rede e Internet » Exibir status da rede e tarefas**.

    6.  Verifique se há uma rede ativa listado como contoso.local como uma rede de domínio.

    7.  Feche o **Painel de controle**.

> [!NOTE]
> Opcional: neste ponto, você pode instalar os complementos e extensões do MIM.
 
> [!div class="step-by-step"]  
> [« Serviço de Sincronização do MIM](install-mim-sync.md)
> [Sincronizar bancos de dados »](install-mim-sync-ad-service.md)
