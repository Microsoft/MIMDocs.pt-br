---
title: Implantar o Privileged Access Management do MIM com o Windows Server 2016 | Microsoft Docs
description: Saiba mais sobre a implantação do Privileged Access Management com o Windows Server 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 347eda5872792872a9bb30357c45835303f92e01
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518531"
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>Implantar o PAM do MIM com o Windows Server 2016


Esse cenário permite que o MIM 2016 SP1 aproveite os recursos do Windows Server 2016 como o controlador de domínio para a floresta "PRIV".  Quando esse cenário está configurado, o tíquete do Kerberos do usuário terá uma limitação de tempo para o tempo restante da ativação de sua função. 

> [!Note]
> Visualizações técnicas do Windows Server 2016 anteriores à Visualização Técnica 5 não podem ser usadas com esta versão do MIM.

## <a name="preparation"></a>Preparação

No mínimo duas VMs são necessárias para o ambiente de laboratório:

-   A VM hospeda o controlador de domínio PRIV que executa o Windows Server 2016

-   A VM hospeda o serviço de MIM, executando o Windows Server 2016 (recomendado) ou o Windows Server 2012 R2

> [!NOTE]
> Se você ainda não tiver um domínio "CORP" no seu ambiente de laboratório, um controlador de domínio adicional para esse domínio será necessário. O controlador de domínio "CORP" pode executar o Windows Server 2016 ou o Windows Server 2012 R2.


Execute a instalação, conforme descrito no [Guia de Introdução](privileged-identity-management-for-active-directory-domain-services.md), **exceto conforme indicado a seguir**:

- Se você estiver criando um novo domínio CORP, ao seguir as instruções em [Etapa 1 – Preparar o controlador de domínio CORP](step-1-prepare-corp-domain.md), poderá optar por configurar opcionalmente o domínio CORP para estar no nível funcional do Windows Server 2016. **Se você escolher essa opção, faça os seguintes ajustes**:

  - Se você estiver usando a mídia do Windows Server 2016, a opção de instalação será chamada de Windows Server 2016 (Servidor com a Experiência Desktop).

  - Você pode especificar o nível funcional do Windows Server 2016 para o domínio e a floresta CORP fornecendo 7 como o número de versão de domínio e floresta no argumento para o comando Install-ADDSForest, da seguinte maneira:
    ```
    Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
    ```
  - Em "Criar novos usuários e grupos", o comando final (New-ADGroup -name 'CONTOSO\$\$\$' …) **não é necessário quando os controladores de domínio CORP e PRIV são o nível funcional de domínio do Windows Server 2016**.

  - As alterações descritas em "Configurar auditoria"(item 8) e "Definir configurações do Registro" (item 10) são **recomendadas, mas não necessárias** quando controladores de domínio CORP e PRIV são o nível funcional de domínio do Windows Server 2016.

- Se você optar por usar o Windows Server 2012 R2 como o sistema operacional para CORPDC, deverá instalar hotfixes 2919442, 2919355 [e atualizar 3155495](http://support.microsoft.com/kb/3156418) no CORPDC.

- Siga as instruções em [Etapa 2 – Preparar o controlador de domínio PRIV](step-2-prepare-priv-domain-controller.md), exceto para esses ajustes:

  -   Instale usando a mídia do Windows Server 2016. A opção de instalação será chamada de Windows Server 2016 (Servidor com a Experiência Desktop).

  -   Em instruções de "Adicionar funções" (item 4), **você deve especificar os números de versão de domínio e floresta na quarta linha dos comandos do PowerShell para ser 7**, para permitir que os recursos do Windows Server AD descritos posteriormente sejam ativados.

      ```
      Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
      ```  

  -   Ao configurar os direitos de logon e auditoria, observe que o programa de Gerenciamento de Política de Grupo será localizado na pasta Ferramentas Administrativas do Windows.

  -   Definir as configurações do Registro necessárias para a migração do histórico SID (item 8) **não é necessário quando o domínio PRIV é o nível funcional de domínio do Windows Server 2016**.

  -   Depois de configurar a delegação e antes de reiniciar o servidor, habilite os recursos do Privileged Access Management no Active Directory do Windows Server 2016 abrindo uma janela do PowerShell como administrador e digitando os seguintes comandos.

  ```
  $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
  Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
  ```

  - Depois de configurar a delegação e antes de reiniciar o servidor, autorize os administradores MIM e a conta de serviço de MIM para criar e atualizar objetos de sombra.

    a. Inicie uma janela do PowerShell e digite ADSIEdit.

    b. Abra o menu Ações, clique em “Conectar a”. Na configuração de Ponto de conexão, altere o contexto de nomenclatura de "Contexto de nomenclatura padrão" para "Configuração" e clique em OK.

    c. Após se conectar, no lado esquerdo da janela abaixo de "Editar ADSI", expanda o nó Configuração para ver “CN=Configuration,DC=priv,....”. Expanda CN=Configuration e, em seguida, expanda CN=Services.

    d. Clique com o botão direito em “CN=Shadow Principal Configuration” e clique em Propriedades. Quando a caixa de diálogo Propriedades for exibida, altere para a guia Segurança.

    e. Clique em Adicionar. Especifique as contas "MIMService", além de outros administradores de MIM que mais tarde estarão executando New-PAMGroup para criar grupos de PAM adicionais. Para cada usuário, na lista de permissões permitidas, adicione "Gravação", "Criar todos os objetos filho" e "Excluir todos os objetos filho". Adicione as permissões.

    f. Altere para as configurações avançadas de segurança. Na linha que permite o acesso de MIMService, clique em Editar. Altere a configuração de "Aplica-se a" para "para este objeto e todos os objetos descendentes". Atualize essa configuração de permissão e feche a caixa de diálogo de segurança.

    g. Feche o ADSI Editar.

  - Depois de configurar a delegação e antes de reiniciar o servidor, autorize os administradores MIM para criar e atualizar a política de autenticação.

    a.  Inicie um **prompt de comandos** com privilégios elevados e digite os seguintes comandos, substituindo o nome da sua conta de administrador MIM para “mimadmin” em cada uma das quatro linhas:
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


- Siga as instruções em [Etapa 3 – Preparar um servidor PAM](step-3-prepare-pam-server.md), com esses ajustes.

  -   Se estiver instalando o Windows Server 2016, observe que a função "ApplicationServer" não está disponível.

  -   Se estiver instalando o MIM no Windows Server 2016, **não será possível instalar o SharePoint 2013**.

- Siga as instruções em [Etapa 4 – Instalar componentes MIM no servidor e estação de trabalho PAM](step-4-install-mim-components-on-pam-server.md), com esses ajustes.

  -   O usuário que está instalando os componentes de serviço e PAM do MIM **deve ter acesso de gravação ao domínio PRIV no AD**, porque a instalação de MIM cria uma nova UO "Objetos do PAM" do AD.

  -   Se o SharePoint não estiver instalado, não instale o Portal do MIM.

- Siga as instruções em [Etapa 5 – Estabelecer confiança](step-5-establish-trust-between-priv-corp-forests.md) com estes ajustes:

  - Ao estabelecer relação de confiança unidirecional, basta executar os dois primeiros comandos do PowerShell (get-credential e New-PAMTrust), **não execute o comando New-PAMDomainConfiguration**.

  - Depois de estabelecer a relação de confiança, faça logon no PRIVDC como administrador \\do PRIV, inicie o PowerShell e digite os seguintes comandos:
    ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
    /usero:contoso\administrator /passwordo:Pass@word1

    netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
    /usero:contoso\administrator /passwordo:Pass@word1  

    netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
    /usero:contoso\administrator /passwordo:Pass@word1
    ```

- Item 5 (verificação de confiança) **não é necessário quando os domínios CORP e PRIV estão no nível funcional de domínio do Windows Server 2016**.

## <a name="more-information"></a>Mais informações

- [Privileged Access Management para Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md)
- [Configure o ambiente do MIM para o Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Configurar o PAM usando scripts](sp1-pam-configure-using-scripts.md)
