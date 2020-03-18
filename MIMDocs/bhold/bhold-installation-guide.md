---
title: Instalação do BHOLD SP1 | Microsoft Docs
description: Documentação da instalação do BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fb3cf6e5b00c1bd0c01d86aff474dc2ff28c2815
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042246"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guia de instalação do Microsoft BHOLD Suite SP1 (6.0)

O Microsoft® BHOLD Suite SP1 (Service Pack 1) é uma coleção de aplicativos que, quando usada com o MIM (Microsoft Identity Manager) 2016 SP1, adiciona gerenciamento de função, análise e atestado efetivos ao MIM. O Microsoft BHOLD Suite SP1 consiste dos seguintes módulos:

- BHOLD Core
- Conector de Gerenciamento de Acesso
- Integração de FIM/MIM com o BHOLD
- Gerador de Modelos do BHOLD
- Análise do BHOLD
- Relatório do BHOLD
- Atestado do BHOLD


> [!NOTE]
> **Aplica-se a**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>O que este documento abrange

Este documento explica como planejar sua implantação do BHOLD para atender às suas necessidades de negócios e instalar cada módulo BHOLD. Para cada módulo, são detalhados os requisitos relevantes de hardware, de infraestrutura e de software, bem como a configuração de rede de pré-instalação e também as informações necessárias durante a instalação e eventuais etapas de pós-instalação.

## <a name="pre-requisite-knowledge"></a>Conhecimento de pré-requisito

Este documento pressupõe que você tenha um entendimento básico de como instalar o software em computadores servidores. Ele também pressupõe que você tenha um conhecimento básico de software de banco de dados Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) e Microsoft SQL Server 2012. Uma descrição de como instalar e configurar tecnologias dependentes como o AD DS e o FIM está fora do escopo desta documentação. Para obter informações sobre as funções desempenhadas pelos módulos do Microsoft BHOLD, consulte [o guia de conceitos do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Público-alvo

Este documento destina-se a planejadores de TI, arquitetos de sistemas, responsáveis pelas decisões de tecnologia, consultores, planejadores de infraestrutura e pessoal de TI que pretendem implantar o Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Considerações sobre a infraestrutura do BHOLD

Geralmente, o FIM e BHOLD são usados em um ambiente de infraestrutura grande. Você pode personalizar a arquitetura do FIM e do BHOLD para atender às suas necessidades de negócios específicas. As seções a seguir fornecem algumas soluções de arquitetura possíveis. Esta visão geral não é uma lista abrangente de todas as opções possíveis, mas sugere maneiras que você pode implantar o BHOLD em sua rede.
 
Esta seção aborda os seguintes tópicos:

- Arquitetura de servidor único
- Arquitetura de servidor duplo
- Arquitetura de duas camadas
- Recomendações do SQL Server

### <a name="single-server-architecture"></a>Arquitetura de servidor único

Para implantação em empresas de pequenas porte ou para fins de desenvolvimento, você pode instalar o BHOLD e o FIM no mesmo servidor que o SQL Server e o AD DS, conforme mostrado na figura a seguir.
 
![Arquitetura de servidor único](media/bhold-installation-guide/single.png)

Quando o BHOLD Suite SP1 e o Portal do FIM são instalados juntos em um único servidor, você deve criar diferentes aliases de host (registros A ou CNAME) no DNS para o FIM e o BHOLD. Isso permite que SPNs (nomes da entidade de serviço) separados sejam criados para os serviços BHOLD e FIM. Para obter mais informações, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Para obter diretrizes sobre como instalar o FIM em uma configuração de servidor único, consulte [Configuração comum para guias de Introdução](https://technet.microsoft.com/library/ff575965.aspx) na Biblioteca do Microsoft TechNet.

### <a name="dual-server-architecture"></a>Arquitetura de servidor duplo

A instalação do BHOLD Core e do FIM em servidores separados fornece maior desempenho e flexibilidade para organizações de médio porte que não necessitam de uma implantação mais complexa, tal como aquela fornecida por arquiteturas multicamada. A figura a seguir mostra o BHOLD e o FIM instalados em seus próprios servidores; o servidor FIM também está executando o SQL Server para fornecer serviços de banco de dados para o BHOLD e o FIM. O Serviço de Sincronização do FIM em execução no servidor do FIM sincroniza as alterações entre os bancos de dados do FIM e do BHOLD. Observe que, se o autoatendimento de usuário final for necessário, o módulo Integração de FIM com o BHOLD deverá ser instalado no mesmo servidor que o Serviço FIM e o Portal do FIM. O módulo Integração de FIM com o BHOLD requer que o Serviço FIM e o módulo Integração de FIM com o BHOLD estejam instalados no mesmo servidor.

![Arquitetura de servidor duplo](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> O recurso de relatório do módulo Integração de FIM com o BHOLD requer que os bancos de dados BHOLD e FIM estejam instalados na mesma instância do SQL Server e que a conta de serviço do BHOLD tenha direitos de acesso ao banco de dados de serviço do FIM.

### <a name="two-tier-architecture"></a>Arquitetura de duas camadas

Na maioria dos ambientes, especialmente naqueles em que o desempenho é importante, você deve executar o BHOLD Suite SP1, o FIM e o SQL Server em servidores separados (arquitetura de duas camadas). Com uma arquitetura de duas camadas, os recursos de CPU e de memória são dedicados para cada camada. A ilustração a seguir mostra uma maneira possível de configurar uma arquitetura de duas camadas. O Serviço de Sincronização do FIM em execução no servidor do FIM sincroniza as alterações entre os bancos de dados do FIM e do BHOLD. Observe que, se o autoatendimento de usuário final for necessário, o módulo Integração de FIM com o BHOLD deverá ser instalado no mesmo servidor que o Serviço e o Portal do FIM.

![arquitetura de duas camadas](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendações do SQL Server

Se você está implantando o BHOLD em uma grande organização, é altamente recomendável que você siga estas diretrizes para configurar o banco de dados do Microsoft SQL Server:

- Implante o SQL Server em um servidor separado de quaisquer serviços FIM ou BHOLD.
- Isole o arquivo de log do arquivo de dados no nível do disco físico.
- Se você estiver usando RAID para fornecer redundância de armazenamento, use o nível de RAID 10 (1+0). Não use o RAID nível 5.
- Certifique-se de definir as configurações corretas ao usar mais de 2 GB de memória física para o servidor que executa o SQL Server.

Para obter mais informações sobre as práticas recomendadas do SQL Server, consulte [As 10 Melhores Práticas Recomendadas de Armazenamento](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) na Biblioteca do Microsoft TechNet.

### <a name="trusted-certificates-list-update"></a>Atualização de lista de certificados confiáveis

O Windows pode ser configurado para validar cadeias de certificados antes de iniciar um serviço. Em tais sistemas, um serviço não pode iniciar se o código executável do serviço foi assinado com um certificado que não está na TCL (lista de certificados confiáveis) do servidor. O código do software Microsoft BHOLD Suite SP1 é assinado usando uma cadeia de certificados de assinatura de código que é originada com o certificado da Autoridade de Certificação Raiz 2010 da Microsoft.
O Windows pode ser configurado para recuperar os certificados raiz da Microsoft por uma conexão de Internet. Em um sistema desconectado, no entanto, o Windows Server inclui somente os certificados que estavam presentes no programa raiz antes do lançamento do Windows. Em versões do Windows Server anteriores ao Windows Server 2010, esses certificados não incluirão o certificado raiz necessário para validar a cadeia de certificados de assinatura de código do BHOLD Suite SP1. Se você pretende instalar um ou mais módulos do Microsoft BHOLD Suite SP1 em um sistema que talvez não tenha uma TCL atualizada, você deve baixar e instalar o pacote de atualização raiz ou usar a Política de Grupo para instalar o pacote de atualização raiz, antes de instalar um módulo do SP1 BHOLD Suite. Para obter mais informações, consulte [Membros do programa de certificados raiz do Windows](https://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalando o BHOLD Suite SP1 no Windows Server 2012/2016 – etapa necessária 

![Instalação de IIS do BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Se você instalar o BHOLD Suite SP1 no Windows Server 2012 ou 2016, as páginas da Web do BHOLD não estarão disponíveis até que você modifique o arquivo applicationHost.config localizado em ```C:\Windows\System32\inetsrv\config```. Na seção ```<globalModules>```, adicione ```preCondition="bitness64``` à entrada que começa com ```<add name="SPNativeRequestModule"``` de modo que ela seja transcrita da seguinte maneira:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Depois de editar e salvar o arquivo, execute o comando iisreset para redefinir o servidor IIS.


## <a name="upgrading-bhold-suite"></a>Atualizando o BHOLD Suite

Você não pode atualizar uma instalação existente do BHOLD Suite. Em vez disso, você deve desinstalar uma instalação existente do BHOLD Suite antes de atualizar os módulos BHOLD. Se você tiver um modelo de função do BHOLD existente, você poderá atualizar o banco de dados do BHOLD e usá-lo quando você instalar o módulo BHOLD Core atualizado. Para obter mais informações, consulte [Substituindo o BHOLD Suite pelo BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Próximas etapas

- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)
