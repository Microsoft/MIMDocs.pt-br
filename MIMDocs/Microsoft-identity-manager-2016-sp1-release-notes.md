---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft Docs
description: "Compreenda o funcionamento do MIM 2016 para criar uma experiência de gerenciamento de identidade mais segura e mais conveniente na nuvem e local."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 69d44af5eaef3665f3a55ea91f48d3658cd5e65c
ms.contentlocale: pt-br
ms.lasthandoff: 05/09/2017


---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>Novidades no Microsoft Identity Manager 2016 Service Pack 1 #

Como parte do ciclo de lançamento regular de manutenção e atualização do Microsoft Identity Manager, temos o prazer de anunciar o [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). Este documento descreve as atualizações, aprimoramentos, recursos e alterações incluídas nesta versão.

Se você encontrar problemas durante uma implantação de produção do MIM SP1, entre em contato com o Atendimento Microsoft.

Também queremos ouvir sua opinião! Envie um email com seus comentários, opiniões ou preocupações para a equipe de produto em [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## <a name="updates-in-this-service-pack"></a>Atualizações neste service pack #

### <a name="mim"></a>MIM

- **Compatibilidade de navegadores no Portal do MIM de autoatendimento de usuário final:** neste Service Pack, introduzimos o suporte para a maioria dos principais navegadores. Agora, os usuários podem acessar e interagir com o Portal do MIM para gerenciamento de autoatendimento de perfil e de grupo no Microsoft Edge, Chrome e Safari.

- **Suporte ao Serviço do MIM para Exchange Online:** há tempos o Serviço do MIM oferece suporte ao envio e recebimento de emails para aprovações e notificações. Antes do SP1, o MIM só fornecia suporte ao Exchange Server ou SMTP. Com o service pack 1, o Serviço do MIM pode enviar e receber solicitações, bem como notificações por email, usando uma conta online do Exchange do Office365.

- **Validação do formato de arquivo de imagem no carregamento:** o MIM agora é capaz de validar o formato de arquivo de imagens quando forem carregados no portal.

### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Suporte a floresta "PRIV" (bastião) PAM para o nível funcional do Windows Server 2016:** o Serviço MIM PAM podem ser configurado em um ambiente com controladores de domínio em execução no nível funcional de floresta dos Active Directory Domain Services do Windows Server 2016. Quando configurado, o tíquete do Kerberos do usuário terá uma limitação tempo para o tempo restante da ativação de sua função.

    >[!Note]
    Se você optar por manter o nível funcional de floresta do Windows Server 2012 R2 no domínio CORP, recomendamos a instalação do [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) e do [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) no controlador de domínio CORP.

- **Elevação de conta privilegiada em grupos exclusivos para a floresta "PRIV" (bastião):** agora, os administradores podem informar ao Serviço do MIM sobre grupos e usuários exclusivos à floresta "PRIV". Isso permite que esses usuários e grupos sejam incluídos em funções do PAM.  Eles podem ser ativados para uma função e receber a associação a grupos na floresta "PRIV".

- **Scripts de implantação do PAM:** os scripts de implantação do PAM permitem que os administradores agilizem a instalação do ambiente do PAM.

- **Cmdlets do PAM para configuração do Silo de política de autenticação:** o service pack 1 introduz novos Cmdlets para proteger a segurança de sua floresta de bastiões. Esses Cmdlets criam automaticamente um Silo de política de autenticação, associado a um Modelo de política de autenticação.

    >[!Note]
    Esses Cmdlets são executados automaticamente como parte dos scripts de implantações.


## <a name="platform-support"></a>Suporte de plataforma
Encontre informações atualizadas de suporte de plataforma no documento chamado [Plataformas com suporte para MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).  Novas plataformas com suporte neste service pack incluem o SQL Server 2016, SharePoint 2016

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>Problemas corrigidos nesta versão de disponibilidade geral do MIM 2016

### <a name="pam"></a>PAM
- New-PAMGroup não criou objetos MIM para grupos locais de domínio na floresta PRIV
- New-PAMDomainConfiguration falharia com uma mensagem de erro "netdom"
- Serviço de Monitoramento do PAM registrou avisos para grupos na floresta PRIV

## <a name="how-to-upgrade-to-service-pack-1"></a>Como atualizar para o Service Pack 1

Os clientes que estão atualizando para o Microsoft Identity Manager 2016 Service Pack 1 devem seguir as orientações abaixo sobre todos os serviços aplicáveis à implantação.

>[!Note]
>Os clientes que executam o Forefront Identity Manager 2010 R2 SP1 ou anterior devem atualizar primeiro seu ambiente para o Microsoft Identity Manager 2016 lançado em agosto de 2015 e, em seguida, executar as etapas abaixo.

Antes de começar

Você precisa atualizar o mecanismo de Sincronização do MIM antes de atualizar o serviço e o portal do MIM.
Você precisa fazer backup dos bancos de dados MIMService e MIM Sync.

  1. Desinstalar o componente do Microsoft Identity Manager que você está atualizando
  2. Após a conclusão da desinstalação, abra a página de abertura localizada na mídia de instalação "FIMSplash.htm"
  3. Selecione o componente MIM para atualizar
  4. Prossiga com a instalação seguindo as instruções
    * Instalação do Serviço e Portal do MIM: ao escolher o Exchange Online como a conta de email, insira o endereço de email e as credenciais da conta do Exchange Online na próxima tela.

