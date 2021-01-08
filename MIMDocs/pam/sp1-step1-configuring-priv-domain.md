---
title: Etapa 1 Configurando o domínio PRIV
description: Preparar o domínio PRIV com identidades novas ou existentes a serem gerenciadas pelo Microsoft Identity Manager usando scripts
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: a4d7c8ed4f5d7a9d6b4653caf03de69f1d18e509
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010618"
---
# <a name="step-1-configuring-the-priv-domain"></a>Etapa 1 Configurando o domínio PRIV

> [!div class="step-by-step"]
> [Etapa 2 »](sp1-step2-configuring-corp-domain.md)

1. Faça logon no PRIVDC como administrador
   * Se essa implantação do PAM for um ambiente PRIV-Only, faça logon no CORPDC
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 1 do Menu (Configuração de Floresta PRIV)


As contas de serviço necessárias para gerenciar o SQL, o SharePoint e o MIM são criadas automaticamente, caso ainda não estejam presentes no domínio. Você precisará inserir as senhas para a criação dessas contas de serviço durante a execução do script.

Se o domínio PRIV for o Windows Server 2016, com o nível funcional definido como Windows Server 2016, o script solicitará a habilitação do Active Directory opcional ' Privileged Access Management recurso ' exigido pelo PAM. Escolha ‘Sim’ para continuar. Para níveis funcionais abaixo do Windows Server 2016, ignore o aviso que outras configurações não serão executadas. Você precisará executar novamente a configuração da floresta PAMDeployment.ps1 e do PAM, uma vez que o administrador gere o nível funcional para o Windows Server 2016.

>[!NOTE]
>As etapas a seguir não são necessárias para as configurações do PRIVOnly

7. Copie o SIDs.txt que é gerado no $env:SYSTEMDRIVE\PAM para a pasta semelhante no CORPDC.
   * Você precisará dessa lista de SIDs no CORPDC, para configurar permissões em uma etapa subsequente para que os usuários PRIV possam ler as propriedades de usuário CORP.
8. Quando o script for concluído, ele solicitará a reinicialização do computador para que as alterações entrem em vigor.

> [!div class="step-by-step"]
> [Etapa 2 »](sp1-step2-configuring-corp-domain.md)
