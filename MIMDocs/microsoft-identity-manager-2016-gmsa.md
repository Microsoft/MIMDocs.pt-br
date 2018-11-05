---
title: Conversão de serviços específicos do MIM em gMSA | Microsoft Docs
description: Tópico que descreve as etapas básicas para configurar o gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 63f2509d35355a8fe3a59b173756257298079a92
ms.sourcegitcommit: 6374aa4f7d58b7218626d36d0fc2dc4b38cb8332
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237223"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversão de serviços específicos do MIM em gMSA

Este guia percorrerá as etapas básicas para configurar gMSA para serviços com suporte. O processo de converter em gMSA é fácil depois que você configura previamente seu ambiente.

Hotfix necessário: \<link para KB mais recente\>

Com suporte:

-   Serviço de Sincronização do MIM (FIMSynchronizationService)
-   Serviço MIM (FIMService)
-   Registro da Senha do MIM
-   Redefinição de Senha do MIM
-   Serviço de Monitoramento do PAM (PamMonitoringService)
-   Serviço de componente do PAM (PrivilegeManagementComponentService)

Sem suporte:

-   Não há suporte para o Portal do MIM. Ele faz parte do ambiente do SharePoint, e você precisará implantar no modo de farm e [Configurar a alteração de senha automática no SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Todos os Agentes de Gerenciamento
-   Gerenciamento de Certificados da Microsoft
-   BHOLD

<a name="general-information"></a>Informações gerais 
--------------------

Leitura necessária para concluir a instalação e entender

-   [Visão geral das contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Primeira etapa em seu controlador de domínio do Windows

1.  Crie a Chave Raiz de Serviços de Distribuição de Chave (KDS) (apenas uma vez por domínio), se necessário. A Chave Raiz é usada pelo serviço KDS em controladores de domínio (juntamente com outras informações) para gerar senhas.

    -   Add-KDSRootKey –EffectiveImmediately

    -   "–EffectiveImmediately" significa aguardar até \~10 horas/precisa ser replicada para todos os controladores de domínio. Isso era aproximadamente uma hora para os dois controladores de domínio.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Serviço de Sincronização
-----------------------

1.  Cria um grupo chamado "MIMSync_Servers" e adiciona todos os servidores de sincronização a esse grupo.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  No Windows PowerShell execute o comando abaixo como administrador de domínio com uma conta de computador já está associada ao domínio

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Obter detalhes do GSMA para sincronização:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Se estiver executando o serviço PCNS, você precisará atualizar a delegação

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Em seguida, nos serviços de sincronização, não se esqueça de fazer backup da chave de criptografia, pois ela será solicitada após a instalação de modo de alteração

    -   No servidor em que o Serviço de Sincronização está instalado, localize a ferramenta de Gerenciamento de Chave de Serviço de Sincronização

    -   Por padrão, o item **Exportar conjunto de chaves**  já está selecionado

    -   Clique em  **Avançar**

    -   Agora você será solicitado a inserir as informações de conta de sincronização existente

    -   Insira e verifique as informações de conta de sincronização do FIM

        -   Nome da conta - nome da conta de Serviço de Sincronização usada durante a instalação inicial

        -   Senha - senha da conta de serviço de sincronização

        -   Domínio – domínio do qual a conta de serviço de sincronização faz parte

    -   Clique em  **Avançar**

    -   Se tiver digitado algo incorretamente, você receberá o erro a seguir

    -   Agora que você inseriu as informações de conta com êxito, verá uma opção para alterar o destino (local do arquivo de exportação) da chave de criptografia de backup

        -   Por padrão, o local do arquivo de exportação é  **C:\\Windows\\system32**\\miiskeys-1.bin.

4. Instalação do Serviço de Sincronização do Microsoft Identity Manager SP1 build 4.4.1302.0. Você pode ser encontrado no Site de Download do MSDN ou no Centro de Download de Licença por Volume. Depois que você concluir a instalação, salve o conjunto de chaves miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Instale o [hotfix 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) mais recente ou posterior.

- Quando os patches forem aplicados, interrompa o serviço Sincronização do FIM.
- Programas e recursos do Microsoft Identity Manager no Painel de Controle
- Serviço de Sincronização Alterar -\> Avançar -\> Configurar -\> Avançar

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Limpar o nome da conta
-  Digite o nome de conta de serviço **MIMSyncGMSA** com o símbolo \$ como na
- captura de tela. Deixe Senha em branco.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Next-\> Next-\> Instalar
- Restaure o conjunto de chaves do arquivo. bin salvo.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> A permissão de SQL adicionada à conta é criada para o logon. Portanto, você deve permitir ao usuário aplicar permissões de modo de alteração para adicionar a conta e dbo no banco de dados de serviço de sincronização

## <a name="mim-service"></a>Serviço MIM
-----------

>[!IMPORTANT]
>O processo a seguir deve ser usado ao converter pela primeira vez contas relacionadas ao serviço do MIM para serem contas gMSA. Os cmdlets do PowerShell observados no Apêndice só podem ser usados para alterar as informações de conta depois que a configuração inicial é concluída.*

1.  Criar contras gerenciadas de grupo para Serviço do MIM, API Rest do PAM, Serviço de Monitoramento do PAM, Serviço de Componente do PAM, Portal de Registro do SSPR, Portal de Redefinição do SSPR.

    -   Atualize a delegação de gMSA e SPN
        -   Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames \@{Add="\<SPN\>"}
        -   Delegação
            -   Set-ADServiceAccount -Identity \<gsmaaccount\> -TrustedForDelegation \$true
        -   Delegação restrita
            -   \$delspns = 'http/mim', 'http/mim.contoso.com'
            -   New-ADServiceAccount -Name \<gsmaaccount\> -DNSHostName \<gsmaaccount\>.contoso.com -PrincipalsAllowedToRetrieveManagedPassword \<group\> -ServicePrincipalNames \$spns -OtherAttributes \@{'msDS-AllowedToDelegateTo'=\$delspns }

2.  Adicione a conta de serviço do MIM em Grupos de Sincronização. É necessário para SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **OBSERVAÇÃO**.  Problema conhecido em que os serviços que usam a conta gerenciada travam depois de reiniciar o servidor porque o Serviço de Distribuição de Chaves da Microsoft não foi iniciado depois de reiniciar o Windows. Não foi possível iniciar o serviço, e o Windows também não pôde ser reiniciado. O problema é reproduzível pelo menos no Windows Server 2012 R2. A solução alternativa para esse problema é executar o comando 

-   **sc triggerinfo kdssvc start/networkon**

    para iniciar o Serviço de Distribuição de Chaves da Microsoft quando a rede está ativada (normalmente, no início do ciclo de inicialização).

    Confira a discussão sobre um problema semelhante: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Execute o MSI com privilégios elevados do Serviço do MIM e selecione a alteração.

5.  Em "Configurar página de conexão do servidor principal", marque a caixa de seleção "Usar conta diferente para o Exchange (para contas gerenciadas)". Aqui, você terá uma opção para usar a conta antiga que tem uma caixa de correio ou usar a caixa de correio da nuvem.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na página "Conta de serviço do MIM", digite conta de serviço com o símbolo \$ no fim. Também digite a Senha da Conta do Serviço de Email. A Senha da Conta de Serviço deve ser desabilitada.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Como a função LogonUser não funciona para contas gerenciadas, a próxima página avisará "Verifique se a conta de serviço é segura em sua configuração atual".

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na página "Configurar API REST de Gerenciamento de Acesso Privilegiado", digite o nome da Conta do Pool de Aplicativos com o símbolo \$ no fim e deixe o campo de senha em branco.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Na página "Configurar o serviço de componente do PAM", digite o nome da conta de serviço com o símbolo \$ no fim e deixe o campo de senha em branco.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8.A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Na página "Configurar Serviço de Monitoramento de Gerenciamento de Acesso Privilegiado", digite o nome da conta de serviço com o símbolo \$ no fim e deixe o campo de senha em branco.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Na página "Configurar o Portal de Registro de Senha do MIM", digite o nome da conta com o símbolo \$ no fim e deixe o campo de senha em branco.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Na página "Configurar o Portal de redefinição de senha do MIM", digite o nome da conta com o símbolo \$ no fim e deixe o campo de senha em branco.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Concluir a instalação.

Observação:

-  Após a instalação, duas novas chaves são criadas no Registro pelo caminho
    - "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Serviço Gerenciador\\2010\\" para armazenar a senha criptografada do Exchange. Um para
    - o Exchange Online e outros para o Exchange no local (um deles deve ser
    - vazio).

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Para atualizar a senha, fornecemos um script [baixe-o aqui](microsoft-identity-manager-2016-gmsascript.md) para que o cliente não tenha que executar o modo de alteração

- Para criptografar a senha do Exchange, o instalador cria um serviço adicional e
    - é executado sob a conta gerenciada. As mensagens a seguir serão adicionadas ao
    - Log de Eventos do Aplicativo durante a instalação.

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
