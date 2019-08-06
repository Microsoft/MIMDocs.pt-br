---
title: Downloads e licenciamento do Microsoft Identity Manager | Microsoft Docs
description: Este artigo descreve as abordagens de licenciamento do Microsoft Identity Manager (MIM) 2016, com ponteiros sobre onde baixar o software.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: f8c2ec9c0fbdf797acca4a699fec6d358b63f5c8
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701436"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Downloads e licenciamento do Microsoft Identity Manager 2016

Este artigo descreve as abordagens de licenciamento do Microsoft Identity Manager (MIM) 2016, com ponteiros sobre onde baixar o software.

## <a name="licensing-mim-for-your-organization"></a>Licenciamento do MIM para sua organização

O Microsoft Identity Manager 2016 é licenciado por usuário.  Os detalhes sobre o licenciamento estão incluídos nos termos do produto e documentos relacionados, que podem ser baixados na página dos [termos de licenciamento](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx).

### <a name="licensing-for-azure-ad-premium-customers"></a>Licenciamento para os clientes do Azure AD Premium

O Microsoft Identity Manager 2016 é incluído com o Azure Active Directory Premium (P1 e P2), que faz parte do Enterprise Mobility + Security.

O Azure AD Premium fica disponível por meio de um [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), do [Programa de Licença de Volume Aberto](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)e do programa [Provedores de Soluções na Nuvem](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409). Os assinantes do Azure e do Office 365 também podem comprar o Active Directory Premium P1 e P2 online.  Leia mais em [preços do Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>CALs do MIM

Se você não tiver assinaturas do Azure Active Directory Premium para os seus usuários e estiver usando mais recursos do MIM além da sincronização, então será necessária ter uma [CAL (Licença de Acesso de Cliente)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) para cada usuário cuja identidade seja gerenciada pelo MIM. Se você quiser que os usuários externos — como clientes, prestadores de serviços externos ou parceiros de negócios — possam acessar o MIM, você pode adquirir CALs para cada um dos seus usuários externos ou adquirir ECs (Licenças de Conector Externo). As CALs do Microsoft Identity Manager 2016 não são necessárias para usuários cuja identidade esteja apenas no serviço de sincronização do Microsoft Identity Manager e não seja gerenciada por qualquer outro componente do MIM.

### <a name="licenses-for-platform-components"></a>Licenças de componentes da plataforma

Uma licença do Windows Server é necessária para usar o software do servidor do Microsoft Identity Manager 2016 como um suplemento do Windows Server. E uma implantação do MIM também requer uma instalação do SQL Server.  As licenças do Windows Server e do SQL Server não estão incluídas no MIM.

## <a name="obtaining-mim-software"></a>Como obter o software MIM

Antes de iniciar uma nova instalação do MIM ou uma atualização a partir de uma versão anterior, verifique se você tem as versões mais recentes.

Se você começar com uma nova instalação, precisará baixar os arquivos de instalação de cada componente do MIM que seja relevante para o seu cenário. Em seguida, baixe as atualizações desses arquivos e todos os componentes adicionais separados oferecidos no Centro de Download.


| Cenário | Componente | Exigido para o cenário? | Nome da pasta ISO do DVD | Comentários |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Sincronização| Serviço de Sincronização (incluindo o conector AD) | Sim | `Synchronization Service` | |
| Sincronização | PCNS | Não | `Password Change Notification Service` |  A ser instalado nos controladores de domínio |
| Sincronização | Conectores para LDAP, SQL, serviços Web, PowerShell, Lotus Domino, do Graph | Não | N/D | Distribuídos por meio do Centro de Download |
| Gerenciamento de Acesso Privilegiado | Serviço MIM | Sim | `Service and Portal` | |
| Autoatendimento | Serviço e Portal do MIM | Sim | `Service and Portal` | |
| Autoatendimento | Suplementos e extensões | Não | `Add-ins and extensions` | A ser instalado em computadores de usuário final |
| Autoatendimento | Relatório SCSM | Não | `Data Warehouse Support Scripts` | |
| Autoatendimento | Agente de relatório híbrido | Não | N/D | Distribuídos por meio do Centro de Download |
| Autoatendimento | Pacotes de idiomas | Não | `LANGUAGE Packs` | |
| Gerenciamento de certificado | CM | Sim | `Certificate Management` | |
| Gerenciamento de certificado | Cliente em massa do CM | Não | `CM Bulk Client` | |
| Gerenciamento de certificado | Cliente CM | Não | `CM Client`  | |
| Gerenciamento de certificado | Aplicativo CM para Windows | Não | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Obtenção de pacotes do Windows Installer

Para uma nova instalação, a maioria das organizações baixa os pacotes de instalação do MIM no [Centro de Serviços de Licenciamento por Volume](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


O arquivo ISO do DVD contém uma pasta para cada componente do MIM: Serviço de sincronização, Serviço e Portal, etc. Se você pretende instalar o software em um computador diferente do qual baixou, certifique-se de copiar o arquivo ISO ou a pasta inteiros para o componente: não copie simplesmente só o arquivo MSI de uma pasta sem o restante dos arquivos e subpastas.

Se você não tiver acesso ao Centro de Serviços de Licenciamento por Volume, os clientes com uma assinatura de desenvolvedor apropriada também podem baixar o MIM 2016 SP1 como um arquivo ISO em [Downloads dos Meus Benefícios do Visual Studio](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Procurar o "Microsoft Identity Manager 2016 com Service Pack 1".  

Se você não tiver acesso ao Centro de Serviços de Licenciamento por Volume e simplesmente quiser experimentar o software MIM por um período limitado, é possível baixar uma [versão de avaliação do MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Este software não se destina para uso em produção e deixa de funcionar 180 dias após a primeira instalação, não podendo ser atualizado. A versão de avaliação requer o Windows Server 2008 R2, o Windows Server 2012 ou o Windows Server 2012 R2 para instalação.  Se o MIM for novidade e você estiver aprendendo essa tecnologia, lembre-se de que todos os cenários do MIM exigem um domínio do Active Directory, um Windows Server e o SQL Server. Se você não tiver o Windows Server ou SQL Server já instalados, poderá tentar o [provisionamento de uma VM com o SQL Server 2016 e o Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Obtenção de atualizações

Depois de instalar o MIM a partir do MSI, instale os hotfixes necessários.

No [histórico de lançamento das versão do Identity Manager](./reference/version-history.md), verifique a versão de atualização mais recente, cujo link para o site de download está nos arquivos de patch do instalador.

Para determinar quais arquivos de atualização são necessários, esta tabela lista os componentes e o nome do arquivo de patch (MSP) correspondente em uma atualização.

| Cenário | Componente | Nome da pasta ISO do DVD | Nome do arquivo de patch de atualização correspondente |
|----------|-----------|-   |-------------------|----------|--------------|
|Sincronização| Serviço de sincronização | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Autoatendimento | Serviço e Portal do MIM | `Service and Portal` | `FIMService_x64*msp` |
| Autoatendimento | Suplementos e extensões | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Autoatendimento | Pacotes de idiomas | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Gerenciamento de acesso (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Gerenciamento de certificado | CM |  `Certificate Management` | `FIMCM*.msp` |
| Gerenciamento de certificado | Cliente em massa do CM |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Gerenciamento de certificado | Cliente CM | Cliente CM |`FIMCMClient*msp` |

Leia todas as notas de versão associado com a atualização antes de instalar o arquivo MSP.

As atualizações de [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) não são distribuídas como arquivos MSP, apenas como instaladores do MSI.

### <a name="additional-downloads"></a>Downloads adicionais

Os downloads a seguir também podem ser relevantes:

- [Agente de Relatório Híbrido de MIM](https://www.microsoft.com/download/details.aspx?id=55112)

- [Conector LDAP genérico, conector de SQL genérico, conector do Graph, conector do Lotus Domino, conector do PowerShell, conector de serviços Web](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Conector do Repositório de Perfis de Usuários do SharePoint](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Se você ainda não tiver um domínio do Active Directory e estiver configurando um cenário de PAM para experimentação, consulte os [scripts de implantação do PAM no MIM 2016 SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre os cenários fornecidos no [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Leia o [guia de planejamento de capacidade](capacity-planning-guide.md).
- Implante o MIM para um [cenário de sincronização](microsoft-identity-manager-deploy.md) ou para o [cenário de gerenciamento de acesso privilegiado](./pam/privileged-identity-management-for-active-directory-domain-services.md).

