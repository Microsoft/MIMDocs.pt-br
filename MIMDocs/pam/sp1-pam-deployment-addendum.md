---
title: Adendo
description: Esse é o adendo aos documentos que abordam a implantação de PAM com scripts. Ele explica como configurar os domínios PRIV e CORP, bem como configurar um cliente para que ele faça a validação, e fornece informações sobre como solicitar assistência.
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
ms.openlocfilehash: 79b3547564fd5dd7874ffc53a7df50cb50ad3d49
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010669"
---
# <a name="pam-deployment-scripts-addendum"></a>Adendo dos scripts de implantação do PAM:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Adendo 1 Configuração do domínio PRIV

Depois de descompactar o arquivo compactado na pasta $env:SYSTEMDRIVE\PAM, edite PAMDeploymentConfig.xml para fornecer detalhes da floresta PRIV. Atualize o DNSName, o NetBiosName, o nome do DC, o caminho do banco de dados/log & caminho da pasta SYSVOL. Atualize também o domínio e o ForestMode. Se você estiver usando o Windows Server 2016 ou posterior, defina o DomainMode & Florestamode como Windows Server 2016 (WinThreshold).

1. Faça logon no DC do domínio PRIV como administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a opção de menu 9 (configuração de floresta PRIV)


O controlador de domínio será reinicializado automaticamente após a conclusão. A senha de administrador do DSRM (Modo de Restauração dos Serviços de Diretório) deve corresponder aos critérios a seguir:

  * O comprimento da senha deve ter no mínimo 15 caracteres
  * A senha contém pelo menos um caractere minúsculo
  * A senha contém pelo menos um caractere MAIÚSCULO
  * A senha contém pelo menos um digito ou caractere especial

## <a name="addendum-2-setting-up-the-corp-domain"></a>Adendo 2 Configuração do domínio CORP

Se você estiver apenas começando com o PAM e quiser configurar um ambiente de teste, o script também permitirá a configuração de um domínio CORP. Depois de descompactar o arquivo compactado na pasta $env:SYSTEMDRIVE\PAM, edite PAMDeploymentConfig.xml adicionando detalhes da floresta CORP. Atualize o DNSName, o NetBiosName, o nome do DC, o caminho do banco de dados/log e o caminho da pasta SYSVOL. O nível funcional deve ser pelo menos Windows Server 2012 R2.

1. Faça logon no controlador de domínio CORP
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a opção 10 do menu (Configuração de Floresta CORP)

O controlador de domínio reiniciará automaticamente após a conclusão

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Adendo 3 Configurar um cliente CORP para fazer a validação

O ClientBinaryLocation no arquivo de configuração deve apontar para o local onde setup.exe está localizado.
Faça logon no cliente como administrador local e execute os seguintes comandos em uma janela do PowerShell com privilégios elevados:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a opção 7 do menu (Configuração do Cliente PAM do MIM)


Se o computador não estiver ingressado no domínio, ele solicitará credenciais de administrador CORP para realizar o ingresso no domínio. O computador deve ser reinicializado após o ingresso no domínio. Faça logon no cliente novamente como administrador local e execute os seguintes comandos em uma janela do PowerShell com privilégios elevados:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a opção 7 do menu (Configuração do Cliente PAM do MIM)

Vá para a Etapa 8 fornecida acima.

## <a name="addendum-4-if-something-goes-wrong"></a>Adendo 4 Se algo der errado

Todos os logs de script foram salvos em % AppData%\MIMPAMInstall. Se necessário pelo suporte, compacte a pasta em um arquivo zip junto com os detalhes da operação e o erro.
