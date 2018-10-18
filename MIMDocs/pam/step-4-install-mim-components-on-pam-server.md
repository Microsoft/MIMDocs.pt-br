---
title: Etapa 4 para implantar o PAM – instalar o MIM | Microsoft Docs
description: Instale e configure o Serviço e Portal do MIM nas estações de trabalho e o servidor do Privileged Access Management.
keywords: ''
author: barclayn
ms.author: barclayn
manager: barclayn
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3f5ee3e2a6bdbd1ab203ffcf406b4ca3b991b6f5
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333999"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>Etapa 4 – Instalar componentes MIM no servidor PAM e estação de trabalho

> [!div class="step-by-step"]
> [« Etapa 3](step-3-prepare-pam-server.md)
> [Etapa 5 »](step-5-establish-trust-between-priv-corp-forests.md)

Em PAMSRV, entre como PRIV\Administrator para poder instalar o Portal e o Serviço MIM, bem como o aplicativo Web do portal de exemplo.

  > [!NOTE]
  > É necessário ser um administrador de domínio; se você não estiver executando os comandos a seguir como um administrador de domínio, as verificações de validação da relação de confiança na próxima etapa não serão concluídas.

Se você tiver baixado o MIM, descompacte o arquivo de instalação do MIM para uma nova pasta.

## <a name="run-the-service-and-portal-install-program"></a>Execute o programa de instalação de Serviço e Portal.

Siga as diretrizes do instalador e conclua a instalação.

1. Ao selecionar os recursos de componente, incluem o Serviço MIM (com o Privileged Access Management, mas não o Relatório MIM) e o Portal do MIM  

   ![Instalação personalizada – captura de tela](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. Ao configurar serviços comuns e a conexão de banco de dados do MIM, especifique **Criar um novo banco de dados**.

   > [!NOTE]
   > Se você instalar o Serviço MIM várias vezes para alta disponibilidade, especifique **Usar um banco de dados existente** para todas as instalações posteriores.

3. Ao configurar uma conexão de servidor de email, defina o servidor de email com o nome do host de um servidor Exchange ou SMTP para o ambiente CORP (use corpdc.contoso.local se você não tiver um servidor de email) e desmarque as caixas de seleção **Usar SSL** e **O Servidor de Email é o Exchange Server 2007 ou Exchange Server 2010**.

4. Opte por gerar um novo certificado autoassinado.

5. Defina as seguintes credenciais de conta:
   - Nome da Conta de Serviço: *MIMService*  
   - Senha da Conta de Serviço: <em>Pass@word1</em> (ou a senha criada na Etapa 2)  
   - Domínio da Conta de Serviço: *PRIV*  
   - Conta de Email do Serviço: <em>MIMService@priv.contoso.local</em>  

6. Aceite os padrões para o nome do host do servidor de sincronização e especifique a conta do Agente de Gerenciamento do MIM como *PRIV\MIMMA*. Uma mensagem de aviso será exibida informando que o serviço de sincronização do MIM não existe. Isso é normal, já que o serviço de sincronização do MIM não é usado neste cenário.

7. Defina *PAMSRV* como o endereço do servidor do Serviço MIM.

8. Defina *http://pamsrv.priv.contoso.local:82* como a URL do conjunto de sites do SharePoint.

9. Deixe a URL do portal de registro em branco.

10. Marque a caixa de seleção para abrir as portas 5725 e 5726 no firewall e a caixa de seleção para conceder o acesso ao site do portal do MIM a todos os usuários autenticados.

11. Deixe o nome do host da API REST do PAM em branco e defina *8086* como o número da porta.

    ![Informações de associação referentes à API REST do PAM – captura de tela](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Configure a conta da API REST do PAM no MIM para usar a mesma conta que o SharePoint (já que o Portal do MIM está localizado neste servidor):
    - Nome da Conta do Pool de Aplicativos: *SharePoint*  
    - Senha da Conta do Pool de Aplicativos: <em>Pass@word1</em> (ou a senha criada na Etapa 2)  
    - Domínio da Conta do Pool de Aplicativos: *PRIV*  

    ![Credenciais da conta do pool de aplicativos – captura de tela](./media/PAM_GS_Configure_Component_Service.png)

    Você poderá receber um aviso informando que a Conta de Serviço não é segura na configuração atual. Isso é normal.

13. Configure o serviço de componente do PAM no MIM:
    - Nome da Conta de Serviço: *MIMComponent*
    - Senha da Conta de Serviço: <em>Pass@word1</em> (ou a senha criada na Etapa 2)  
    - Domínio da Conta de Serviço: *PRIV*

    ![Credenciais da conta de serviço de Componente do PAM – captura de tela](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Configure o Serviço de Monitoramento do PAM:
    - Nome da Conta de Serviço: *MIMMonitor*  
    - Senha da Conta de Serviço: <em>Pass@word1</em> (ou a senha criada na Etapa 2)  
    - Domínio da Conta de Serviço: *PRIV*  

    ![Credenciais da conta de Serviço de Monitoramento do PAM – captura de tela](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Na página Inserir Informações dos Portais de Senha do MIM, deixe as caixas de seleção em branco e continue. Clique em **Avançar** para continuar a instalação.

Após a conclusão da instalação, o servidor vai reinicializar, então verifique se o Portal do MIM está ativo e permite aos usuários ver o próprio recurso de objeto no MIM.

## <a name="set-up-mim-portal-management-policy-rules"></a>Configurar regras de política de gerenciamento do Portal do MIM

1. Após a reinicialização de PAMSRV, entre como PRIV\Administrator.

2. Inicie o Internet Explorer e conecte-se ao Portal do MIM em http://pamsrv.priv.contoso.local:82/identitymanagement. Pode haver um pequeno atraso na primeira vez que essa página for localizada.

3. Se necessário, entre como PRIV\Administrator no Internet Explorer.

4. No Internet Explorer, abra **Opções da Internet**, mude para a guia **Segurança** e adicione o site à **Zona da intranet local** se ainda não estiver lá. Feche a caixa de diálogo Opções da Internet.

5. Usando o Internet Explorer para exibir o Portal do MIM, clique em **Regras de Política de Gerenciamento**.

6. Pesquise a regra de política de gerenciamento **Gerenciamento de usuários: os usuários podem ler seus próprios atributos**.

7. Selecione essa regra de política de gerenciamento, desmarque a opção **A política está desabilitada**, clique em **OK** e em **Enviar**.

## <a name="verify-the-firewall-connections"></a>Verificar as conexões de firewall

O firewall deve permitir conexões de entrada para as portas TCP 5725, 5726, 8086 e 8090.

1.  Inicie o **Firewall do Windows com Segurança Avançada** (localizado em Ferramentas Administrativas).  
2.  Clique em **Regras de entrada**.  
3.  Verifique se essas duas regras são listadas:  
    - Serviço Forefront Identity Manager (STS)
    - Serviço Forefront Identity Manager (serviço Web)  
4.  Clique em **Nova regra** > **Porta** > **TCP** e digite as portas locais específicas *8086* e *8090*. Clique no assistente, aceitando os padrões, dê um nome para a regra e clique em **Concluir**.  
5.  Depois de concluir o assistente, feche o aplicativo Firewall do Windows.

6.  Inicie o **Painel de Controle**.  
7.  Em Rede e Internet, selecione **Exibir status e tarefas da rede**.  
8.  Verifique se há uma Rede ativa listada como priv.contoso.local e uma rede de Domínio.  
9. Feche o **Painel de controle**.

## <a name="set-up-the-sample-web-application"></a>Configurar o aplicativo Web de exemplo

Nesta seção, você vai instalar e configurar o aplicativo Web de exemplo para a API REST do PAM no MIM.

1. No arquivo morto de aplicativo Web de exemplo, baixe as [amostras do Identity Management](https://github.com/Azure/identity-management-samples) como um arquivo zip.

2. Descompacte o conteúdo da pasta **identity-management-samples-master\Privileged-Access-Management-Portal\src** em uma nova pasta **C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3. Crie um novo site no IIS com um nome de site Portal de Exemplo do Privileged Access Management no MIM, com o caminho físico C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal e a porta 8090.  Isso pode ser feito usando o seguinte comando do PowerShell:

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. Configure o aplicativo Web de exemplo para poder redirecionar os usuários para a API REST do PAM no MIM. Usando um editor de texto como o Bloco de Notas, edite o arquivo **C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. Na seção **<system.webServer>**, adicione as seguintes linhas:

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. Configure aplicativo Web de exemplo. Usando um editor de texto como o Bloco de Notas, edite o arquivo **C:\Arquivos de Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Defina o valor de **pamRespApiUrl** como *http://pamsrv.priv.contoso.local:8086/api/pamresources/*.

6. Reinicie o IIS com o seguinte comando para que as alterações tenham efeito.

   ```cmd
   iisreset
   ```

7. (Opcional) Verifique se o usuário pode se autenticar na API REST. Abra um navegador da Web como o administrador em PAMSRV.  Navegue até a URL do site http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, autentique se necessário e garanta que o download ocorra.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>Instalar os cmdlets do solicitante do PAM no MIM

Instale os cmdlets do solicitante do PAM no MIM na estação de trabalho configurada na Etapa 1.

1.  Entre em CORPWKSTN como administrador.

2.  Baixe os **Suplementos e extensões** para o computador CORPWKSTN, se ainda não estiverem presentes.

3.  Desempacote do arquivo-morto a pasta **Suplementos e extensões** em uma nova pasta.

4.  Execute o instalador **setup.exe**.

5.  Na instalação personalizada, especifique o **Cliente do PAM** a ser instalado, mas não o **Suplemento do MIM para o Outlook** nem a **Senha e as Extensões de Autenticação do MIM**.

6.  No endereço do Servidor PAM, especifique *pamsrv.priv.contoso.local* como o nome do host do servidor PRIV do MIM.

Depois que a instalação for concluída, reinicie o CORPWKSTN para concluir o registro do novo módulo do PowerShell.

Na próxima etapa, você vai estabelecer a relação de confiança entre as florestas PRIV e CORP.

> [!div class="step-by-step"]
> [« Etapa 3](step-3-prepare-pam-server.md)
> [Etapa 5 »](step-5-establish-trust-between-priv-corp-forests.md)
