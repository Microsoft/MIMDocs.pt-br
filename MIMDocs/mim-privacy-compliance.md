---
title: Manipulação de dados do Microsoft Identity Manager | Microsoft Docs
description: Entenda a manipulação de dados do Microsoft Identity Manager para identificar e relatar dados dentro do ambiente e agir em determinado sistema com base em funções e requisitos operacionais.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: b89f7561869e154ed5639835d1233e19e356ee76
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176737"
---
# <a name="microsoft-identity-manager-data-handling"></a>Manipulação de dados do Microsoft Identity Manager 

Este artigo fornece orientações sobre como as organizações podem tomar decisões que possam ser aplicadas em diversas fontes de dados conectadas.  Isso pode ser obtido por meio das operações de pesquisa, exclusão, atualização e relatório.  Antes de decidir sobre a sua abordagem de excluir ou atualizar, é essencial entender o design e a configuração atuais do seu sistema de gerenciamento de identidades (MIM). 

Abaixo estão alguns cenários que os clientes precisarão considerar e responder às seguintes perguntas: 

- Quais dados você precisa para seu gerenciamento de identidade para ajudar no processo de negócios?
- Onde os dados atuais serão armazenados no MIM?
- Como você usará esses dados no sistema?
- Você está compartilhando esses dados com qualquer fonte de dados de parceiros externos (exportação)?
- Qual é a fonte autoritativa dos dados e do processamento dos dados?
- Qual será o plano de retenção de dados e exclusão de dados?
- Você identificou toda a tecnologia de que precisa para processar e gerenciar dados?

Para ajudar você a compreender um ambiente MIM atual, utilize a seguinte ferramenta para documentar seu ambiente MIM ou adiar para seus documentos de design de implementação.
- [Documentador do MIM - permite exportar a configuração atual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Pesquisando e identificando dados pessoais
A pesquisa de dados no MIM dependerá da instalação e configuração. A maioria dos ambientes é interconectada, mas, para maior clareza, nós os separamos por componentes de alto nível.

### <a name="synchronization-service"></a>Serviço de Sincronização

Todos os dados no MIM relacionados aos usuários são derivados de fontes de dados de RH e do Active Directory (AD). Ao pesquisar dados pessoais, o primeiro local que você deve considerar para a pesquisa é o AD ou fontes de dados conectadas. 

Se você não tiver certeza da fonte de autoridade, poderá rastrear esse usuário no console do Synchronization Service Manager do MIM, clique na barra Pesquisa de Metaverso para exibir os dados pessoais identificáveis ​​armazenados no banco de dados. Os usuários podem pesquisar por um usuário ou atributo específico.

- Para executar uma revisão ou pesquisa de dados de objetos do usuário
    - Abra o cliente do serviço de sincronização
        - Usar o designer de metaverso permite ver importações e precedência de fluxo de atributo.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Usar o designer de metaverso permite pesquisar em qualquer objeto e atributo dentro do banco de dados ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Depois de localizar o objeto, clicar nele abrirá a página de perfil do usuário. Os dados do objeto fornecem os dados abrangentes sobre o objeto, seus atributos, a última modificação e a fonte de autoridade, e a fonte de dados conectada relacionada derivada do exemplo de configuração do agente de gerenciamento abaixo.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM
Se você tem uma instância do Serviço e Portal ou PAM instalado, a capacidade de pesquisar usuários é importante. 

Se você instalou o Portal, poderá usar a interface do usuário para pesquisar em qualquer atributo ou consulta por um usuário específico.

Se você tiver apenas o servidor de serviços (sem a interface do usuário do Portal) instalado, poderá executar uma sintaxe de pesquisa com base no [PSSnapin FIMAutomation], exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

O PAM pode usar a mesma sintaxe acima ou você pode usar o [Módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), especificamente o cmdlet get-pamuser, para pesquisar o usuário dentro do ambiente PAM.

Outras opções de relatório para pesquisar os dados disponíveis estão no serviço e no portal.
- [Relatório híbrido](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Relatório com SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
O serviço BHOLD Core tem uma interface de usuário que permite que você pesquise por um usuário ou atributos. 

![pesquisa BHOLD](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se você estiver sincronizando o BHOLD com o [conector de gerenciamento de acesso](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) para o serviço de sincronização, poderá ver os objetos de usuário conectados e os atributos enviados para o BHOLD Core.

Também é possível carregar o módulo BHOLD Reporting.

- [BHOLD Reporting](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gerenciamento de certificado
A pesquisa do serviço de gerenciamento de certificados é integrada à interface do usuário. O administrador iniciará e selecionará o ‘Localizar usuário e exibir ou gerenciar suas informações’  

![pesquisa cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportando dados pessoais
Como os dados relacionados às entidades no MIM são derivados de várias origens, a maioria dos dados é armazenada no banco de dados do Serviço de Sincronização. Por esse motivo, você deve exportar dados relacionados a objetos do MIM Sync ou pode determinar o proprietário desses dados.

### <a name="synchronization-service"></a>Serviço de Sincronização
Os serviços de sincronização para exportar dados simplesmente selecionam os dados da interface de pesquisa e copiam e colam em csv ou no formato de sua preferência. Outra maneira de exportar esses dados é criar um MA baseado em arquivo para descartar os dados atuais necessários sobre um usuário sinalizado de interesse. Um exemplo do uso do MA baseado em arquivo pode ser encontrado [aqui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM
Com o Serviço e portal juntamente com o PAM, você pode exportar esses dados ao executar uma sintaxe de pesquisa baseada no [PSSnapin FIMAutomation], exemplo encontrado [aqui ](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) e redirecioná-la para [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

O PAM pode usar a mesma sintaxe acima ou você pode usar o [Módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), especificamente o cmdlet get-pamuser, para pesquisar o usuário dentro do ambiente PAM e redirecioná-lo para csv.

- [Exemplo de consulta do serviço MIM usando o PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Os dados do Bhold podem ser exportados para seu formato preferido usando o módulo de relatório do Bhold.

### <a name="certificate-management"></a>Gerenciamento de certificado
Dados de gerenciamento de certificados relacionados a dados pessoais estão conectados ao diretório ativo. Um administrador pode exportar esses dados usando o Active Directory PowerShell.

## <a name="updating-personal-data"></a>Atualizando dados pessoais

Os dados pessoais sobre usuários ou objetos em soluções MIM geralmente são derivados do objeto do usuário nas fontes de dados conectadas da sua organização. Porque quaisquer alterações feitas no perfil de usuário na origem de RH ou em outro sistema autoritativo de registro, como o AD, são refletidas no Serviço de Sincronização MIM.

### <a name="synchronization-service"></a>Serviço de Sincronização

Para executar operações de gerenciamento, os administradores devem fazer parte das operações de sincronização ou do administrador definido [aqui](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

A atualização de dados é feita definindo regras da fonte de autoridade. O console de gerenciamento ajuda a identificar a fonte de autoridade para atualizá-la na origem. Outra opção é criar uma regra de sincronização ou extensão de regra para controlar a atualização de dados se a origem, como os dados de RH, ainda precisar permanecer. Essas são as opções com suporte disponíveis.

Para obter mais informações sobre diferentes maneiras de atualizar o atributo, confira abaixo. 

- [Usar extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Noções básicas sobre a Sincronização de Dados com Sistemas Externos](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM

O Serviço e portal para incluir dados do PAM podem ser atualizados usando os cmdlets FIMAutomation ou PAM. Se você tiver o Portal, também poderá atualizar diretamente pesquisando e modificando o objeto. É importante observar que, dependendo da configuração, atualizar a partir do portal não bastará para garantir a permanência. A fonte de autoridade é altamente dependente da configuração geral.

### <a name="bhold"></a>BHOLD

Os usuários podem ser atualizados diretamente com a interface de usuário do BHOLD Core ou com o conector de gerenciamento de acesso.

### <a name="certificate-management"></a>Gerenciamento de certificado

Os usuários no serviço de gerenciamento de certificados são todos um reflexo do diretório ativo. Para atualizar, use o Active Directory para alterar os detalhes do objeto.

## <a name="deleting-personal-data"></a>Excluindo dados pessoais

>[!Note] 
> Este artigo fornece diretrizes sobre maneiras de excluir dados pessoais do Microsoft Identity Manager e pode ser usado para oferecer suporte às suas obrigações no âmbito do RGPD. Se você estiver procurando informações gerais sobre o RGPD, consulte a seção [RGPD do Portal de Confiança do Serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Os dados no MIM são sincronizados e sempre atualizados a partir de sua fonte de dados conectada. Quando um objeto é excluído no destino, os dados do objeto no MIM podem ser mantidos para fins de investigação de segurança. A Exclusão de Objeto é configurada por regras de origem de dados conectadas ou extensões (código) e/ou regras de exclusão de objetos.

### <a name="synchronization-service"></a>Serviço de Sincronização
O Serviço de Sincronização apresenta várias maneiras de manipular dados ou excluir dados, dependendo dos processos de negócios. Para ajudar a entender, abaixo estão alguns artigos para ajudar a entender as opções de exclusão e atualização de atributos: 

- [Noções Básicas sobre Desprovisionamento](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Usar extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Práticas recomendadas do MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM

É recomendado para o Serviço & Portal que você mantenha a configuração padrão de retenção de recursos do sistema por 30 dias. Isso informa ao serviço quando ele excluirá, não apenas os dados da solicitação, mas também qualquer objeto que precise ser limpo do sistema. Quando o processo ocorre, todos os dados vinculados a esse objeto são excluídos, incluindo todos os dados de registro do SSPR. Isso funciona na configuração de exclusão do objeto acima. Nós temos uma tabela onde armazenamos o guid dos objetos. Para reduzir o tamanho geral da tabela no build 4.4.1459, adicionamos um processo chamado FIM_DeleteExpiredSystemObjectsJob, detalhes sobre este processo podem ser encontrados [aqui](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Como a maioria dos sistemas conectados ao serviço de sincronização, o Bhold pode ser configurado para excluir uma vez que o objeto de origem como RH é removido. Isso é configurado no agente de gerenciamento, e controlado pelas regras de exclusão de objeto, conforme descrito em recursos do serviço de sincronizações.

Outra opção é remover o objeto de usuário diretamente na interface do usuário de núcleo do BHOLD. Dependendo da configuração, isso pode funcionar bem, mas observe que a lógica de provisionamento pode recriar esse usuário se não for excluída na origem.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Gerenciamento de certificado
Para remover um usuário do gerenciamento de certificado, exclua o usuário no diretório ativo.

O gerenciamento de certificados só armazenará o perfil de serviços de certificado com o domínio sAMAccountName. Depois que o usuário é excluído do AD, o cache de usuário só fica presente para os certificados para os quais ele se inscreveu. Não recomendamos excluir nada no banco de dados, pois isso pode causar danos gerais à operação do ambiente.

## <a name="opt-out-of-telemetry"></a>Recusa de telemetria
Os builds FIM/MIM anteriores coletavam telemetria anônima sobre cada implantação e transmitiam esses dados por HTTPS para servidores da Microsoft. No passado, esses dados foram usados ​​pela Microsoft para ajudar a aprimorar versões futuras do FIM/MIM.

>[!Note] 
> Em versões posteriores de 4.5.x.x ou superiores, a coleta de dados será desativada.

Para desativar a coleta de dados na versão anterior, execute o modo de alteração e desmarque o seguinte prompt:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

ou edite o registro e defina o valor como 0: (Component)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Próximas etapas 
- [Para Diretrizes de privacidade relacionadas ao SQL](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Seção RGPD do Portal de Confiança do Serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Arquivamento do FIM 2010: Aumento – Implementação do Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
