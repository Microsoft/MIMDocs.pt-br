---
title: Adendo
description: "Esse é o adendo aos documentos que abordam a implantação de PAM com scripts. Ele explica como configurar os domínios PRIV e CORP, bem como configurar um cliente para que ele faça a validação, e fornece informações sobre como solicitar assistência."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: f69fe68dc63323c0945a4902e34ea8153f938c02


---
# <a name="pam-deployment-scripts-addendum"></a>Adendo dos scripts de implantação do PAM:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Adendo 1 Configuração do domínio PRIV

Depois de descompactar o arquivo compactado na pasta $env:SYSTEMDRIVE\PAM, edite PAMDeploymentConfig.xml para fornecer detalhes da floresta PRIV. Atualize o DNSName, o NetbiosName, o nome do controlador de domínio, o Caminho do Banco de Dados/Log e o Caminho Sysvol. Atualize também o domínio e o ForestMode. Se você estiver testando o Windows Server Technical Preview 5, defina o DomainMode & ForestMode como WinThreshold.

1. Faça logon no DC do domínio PRIV como Administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a opção 9 do menu (Configuração de Floresta Priv)


O controlador de domínio será reinicializado automaticamente após a conclusão. A senha de administrador do DSRM (Modo de Restauração dos Serviços de Diretório) deve corresponder aos critérios a seguir:

  * O comprimento da senha deve ter no mínimo 15 caracteres
  * A senha contém pelo menos um caractere minúsculo
  * A senha contém pelo menos um caractere MAIÚSCULO
  * A senha contém pelo menos um digito ou caractere especial

## <a name="addendum-2-setting-up-the-corp-domain"></a>Adendo 2 Configuração do domínio CORP

Se você estiver apenas começando a usar o PAM e quiser configurar um ambiente de teste, o script também permitirá a configuração de um Domínio CORP. Depois de descompactar o arquivo compactado na pasta $env:SYSTEMDRIVE\PAM, edite PAMDeploymentConfig.xml adicionando detalhes da floresta CORP. Atualize DNSName, NetbiosName, nome do controlador de domínio, Caminho do banco de dados/Log e Caminho Sysvol. O nível funcional deve ser pelo menos Windows Server 2012 R2.

1. Faça logon no controlador de domínio do domínio CORP como Administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a opção 10 do menu (Configuração de Floresta CORP)

O controlador de domínio reiniciará automaticamente após a conclusão

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Adendo 3 Configurar um cliente CORP para fazer a validação

O ClientBinaryLocation no arquivo de configuração deve apontar para o local onde setup.exe está localizado.
Faça logon no cliente como administrador local e execute os seguintes comandos em uma janela elevada do PowerShell:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a opção 7 do menu (Configuração do Cliente PAM do MIM)


Se o computador não estiver ingressado no domínio, ele solicitará credenciais de administrador CORP para realizar o ingresso no domínio. O computador deve ser reinicializado após o ingresso no domínio. Faça logon novamente no cliente como administrador local e execute os seguintes comandos em uma janela elevada do PowerShell:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a opção 7 do menu (Configuração do Cliente PAM do MIM)

Vá para a Etapa 8 fornecida acima.

## <a name="addendum-4-if-something-goes-wrong"></a>Adendo 4 Se algo der errado

Todos os logs de script foram salvos em % AppData%\MIMPAMInstall. Compacte a pasta em um arquivo Zip e a envie por email para [mim2016@microsoft.com](mailto:mim2016@microsoft.com), juntamente com detalhes da operação e do erro.



<!--HONumber=Jan17_HO2-->


