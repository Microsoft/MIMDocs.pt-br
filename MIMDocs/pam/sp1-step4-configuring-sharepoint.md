---
title: Etapa 4 Configurar o SharePoint
description: "Prepare o domínio CORP com identidades novas ou existentes para serem gerenciadas pelo Privileged Identity Manager usando scripts"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 76696f7dce3d79a845c2a8ba9ae8d284012a0df7


---

# <a name="step-4-configuring-sharepoint"></a>Etapa 4 Configurar o SharePoint

>[!div class="step-by-step"]
[« Etapa 3](sp1-step3-installing-configuring-sql.md)
[Etapa 5 »](sp1-step5-configuring-pam.md)

SharePoint deve ser SharePoint Foundation 2013 com SP1.

Para servidores ingressados no domínio, faça logon como MIMAdmin

1. Execute o PowerShell como administrador
2.  .\PAMDeployment.ps1
3.  Selecione a opção de menu 4 (Configuração do SharePoint)


Para servidores de grupo de trabalho

1. Execute o PowerShell como administrador
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. selecione a opção de menu 4 (Configuração do SharePoint)

O computador será reinicializado várias vezes enquanto instala o SharePoint. A cada reinicialização, a configuração do SharePoint deve ser executada novamente, certificando-se de fazer logon com a conta MIMAdmin.
Se o computador que estiver instalando o SharePoint não tiver conexão com a Internet para baixar os pré-requisitos, poderão ser baixados de forma independente e colocados em uma pasta local. **Esse caminho de pasta local precisa ser atualizado no arquivo PAMConfiguration.xml em <PrerequisitesBinaryLocation/>.** Consulte Adendo 5 para obter os links para baixar os arquivos.
Após a instalação, o GUI de Configuração do SharePoint será aberto e percorrerá as etapas para concluir a instalação do SharePoint. Selecione Completar Servidor e percorra o restante da interface de usuário. Após a instalação, ele solicitará a execução do Assistente de Configuração. Conclua as etapas conforme indicado abaixo.

1. Na guia **Conectar-se a um farm de servidores** , altere para **criar um novo farm de servidores.**
2. Especifique o **SQLServer** como o servidor de banco de dados para o banco de dados de configuração e a **ServiceAccount do SharePoint** como a conta de acesso ao banco de dados a ser usada pelo SharePoint.
3. Especifique uma senha como a frase secreta de segurança do farm **(ela não será usada posteriormente)**
4. Aceite o restante das configurações padrão do assistente de configuração do SharePoint para criar um farm de servidor único

Encontre os detalhes na seção **Configurar o SharePoint** na [Etapa 3: Preparar um servidor PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server) Após a conclusão, execute o script ".\PAMDeployment.ps1" novamente, selecionando a Opção 4 (Configuração do SharePoint) para concluir esta etapa.

>[!div class="step-by-step"]
[« Etapa 3](sp1-step3-installing-configuring-sql.md)
[Etapa 5 »](sp1-step5-configuring-pam.md)



<!--HONumber=Nov16_HO2-->


