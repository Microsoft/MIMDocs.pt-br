---
title: "Instalar o portal e o serviço do Microsoft Identity Manager | Microsoft Docs"
description: "Obtenha as etapas para configurar e instalar o Serviço e o Portal do MIM para o Microsoft Identity Manager 2016"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: eb572bc3f062763265a9f24910a82be0cac77a5e


---

# <a name="install-mim-2016-mim-service-and-portal"></a>Instalação do MIM 2016: Serviço e Portal do MIM

>[!div class="step-by-step"]
[« Serviço de Sincronização do MIM](install-mim-sync.md)
[Sincronizar bancos de dados »](install-mim-sync-ad-service.md)

> [!NOTE]
> Este passo a passo usa nomes e valores de exemplo de uma empresa chamada Contoso. Substitua-os pelos seus próprios valores. Por exemplo:
> - Nome do controlador de domínio - **mimservername**
> - Nome do domínio - **contoso**
> - Senha – **Pass@word1**
> - Nome da conta de serviço: **MIMService**

Se você não configurou o pacote de instalação do MIM na última etapa, volte e instale os componentes do Microsoft Identity Manager 2016 antes de continuar.


## <a name="configure-mim-service-and-portal-for-installation"></a>Configure o Serviço e o Portal do MIM para instalação

1. Execute o **instalador de Serviço e Portal do MIM** na subpasta **Serviço e Portal** descompactada.

2. Na tela de boas-vindas, clique em **Avançar**.

3. Leia o Contrato de Licença do Usuário Final e clique em **Avançar** se você aceitar os termos de licença.

4. Na tela do **Programa de Aperfeiçoamento da Experiência do Usuário do MIM**, clique em **Avançar**.

5. Ao selecionar os recursos do componente para essa implantação, é necessário incluir o Serviço do MIM (exceto o Relatório do MIM) e os recursos do Portal do MIM. Você também pode selecionar o Portal de Redefinição de Senha do MIM e o Serviço de Notificação de Alteração de Senha do MIM.

6. Na página **Configurar a conexão de banco de dados do MIM**, escolha **Criar um novo banco de dados**.

    ![Configure a imagem de conexão de banco de dados do MIM](media/MIM-Install10.png)

7. Em **Configurar conexão do servidor de email**, insira o nome do seu Exchange server como **Servidor de Email**. Se você não tiver um servidor de email configurado, use **localhost** como o nome do servidor de email e desmarque as duas caixas de seleção superiores. Clique em **Avançar**.

    ![Configurar a imagem de conexão do servidor de email](media/MIM-Install11.png)

8. Especifique se deseja gerar um novo certificado autoassinado ou selecione o certificado apropriado.

9. Especifique o nome da Conta de Serviço a ser usada, por exemplo, *MIMService*, a senha da Conta de Serviço, por exemplo, *Pass@word1*, seu domínio da Conta de Serviço, por exemplo, *contoso* e a Conta de Email de Serviço, por exemplo, *contoso*.

    ![Configurar a imagem da conta de serviço do MIM](media/MIM-Install12.png)

10. Observe que pode aparecer um aviso dizendo que a conta de serviço não é segura na sua configuração atual.

11. Aceite os padrões para o local do servidor de sincronização e especifique a conta do Agente de Gerenciamento do MIM como *contoso\MIMsync*.

    ![Configuração da imagem do Serviço e Portal do MIM](media/MIM-Install13.png)

12. Especifique *CORPIDM* (nome deste computador) como o endereço do Servidor do Serviço do MIM para o Portal do MIM.

13. Especifique *http://CorpIDM.contoso.local:82* como a URL do conjunto de sites do SharePoint.

14. Especifique *http://CorpIDM.contoso.local:8080* como a URL de registro de senha.

15. Especifique *http://CorpIDM.contoso.local:8088* como a URL de redefinição de senha.

16. Marque a caixa de seleção para abrir as portas 5725 e 5726 no firewall e a caixa de seleção para conceder acesso ao Portal do MIM a todos os usuários autenticados.

## <a name="configure-mim-password-registration-portal"></a>Configuração da imagem do Portal de Registro de Senha do MIM

1.  Configure o nome da conta de serviço para o registro SSPR como *contoso\MIMSSPR* e sua senha como *Pass@word1*.

2.  Especifique *CORPIDM* como o nome de host para o Registro de Senha do MIM e defina a porta como **8080**. Habilite a opção **Abrir porta no firewall**.

    ![Insira as informações de configuração usadas pela imagem do IIS](media/MIM-Install14.png)

3.  Um aviso será exibido. Leia-o e clique em **Avançar**.

4. Na próxima tela de configuração do Portal de Registro de Senha do MIM, especifique *http://CorpIDM.contoso.local* como o Endereço do Servidor do Serviço de MIM para o Portal de Registro de Senha.

## <a name="configure-mim-password-reset-portal"></a>Configuração do Portal de Redefinição de Senha do MIM

1.  Configure o nome da conta de serviço para o registro SSPR como *Contoso\MIMSSPRService* e sua senha como *Pass@word1*.

2.  Especifique *CORPIDM* como o nome de host para o Portal de Redefinição de Senha do MIM e defina a porta como **8088**. Habilite a opção **Abrir porta no firewall**.

    ![Insira as informações de configuração usadas pela imagem do IIS](media/MIM-Install15.png)

3.  Um aviso será exibido. Leia-o e clique em **Avançar**.

4. Na próxima tela de configuração do Portal de Registro de Senha do MIM, especifique *CorpIDname  http://CorpIDname.domain.local* como o Endereço do Servidor do Serviço do MIM para o Portal de Redefinição de Senha.

## <a name="install-mim-service-and-portal"></a>Instalar o portal e o serviço do MIM

Quando todas as definições de pré-instalação estiverem prontas, clique em **Instalar** para começar a instalação dos componentes de **Serviço e Portal** selecionados.

Após a conclusão da instalação, verifique se o Portal do MIM está ativo.

1. Inicie o Internet Explorer e conecte-se ao Portal do MIM em *http://corpidm.contoso.local:82/identitymanagement*. Observe que pode haver um pequeno atraso na primeira visita a esta página.

    - Se necessário, autentique como *contoso\Administrator* para o Internet Explorer.

2. No Internet Explorer, abra **Opções da Internet**altere para a guia **Segurança** e adicione o site à zona da **Intranet Local** se ainda não estiver lá.  Feche a caixa de diálogo **Opções da Internet** .

3. Permita que os usuários exibam suas próprias entradas no MIM.

    1.  Usando o Internet Explorer, no **Portal do MIM**, clique em **Regras de da Política de Gerenciamento**.

    2.  Pesquise a regra de política de gerenciamento **Gerenciamento de usuários: os usuários podem ler seus próprios atributos**.

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

>[!div class="step-by-step"]  
[« Serviço de Sincronização do MIM](install-mim-sync.md)
[Sincronizar bancos de dados »](install-mim-sync-ad-service.md)



<!--HONumber=Jan17_HO4-->


