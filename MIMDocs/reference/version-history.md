---
title: Histórico de versão do Identity Manager | Microsoft Docs
description: Este artigo documenta as várias alterações feitas como parte das atualizações do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fa4e5ade14c1a3df94b868d149472c1b42d1c59c
ms.sourcegitcommit: d6178a67014d66d37056c13d10328ae03e3cd781
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "92757501"
---
# <a name="identity-manager-version-release-history"></a>Histórico de lançamento de versão do Identity Manager

A equipe do Microsoft Identity Manager lança atualizações regularmente. Este artigo destina-se para ajudar você a controlar as versões que foram lançadas. Em seguida, você pode determinar se já está na versão mais recente do service pack e da atualização do hotfix ou se precisa atualizar. 

>[!NOTE]
>Este artigo descreve apenas as versões dos componentes de sincronização, serviço e portal do MIM, cliente, CM e PAM.  Não tem certeza de quais componentes você precisa?  Consulte [Microsoft Identity Manager licenciamento e downloads](../microsoft-identity-manager-licensing.md).
>
>O histórico de versão para componentes do Microsoft BHOLD Suite pode ser encontrado em [módulos BHOLD módulo versão histórico](version-bhold-history.md).
>
>O histórico de versão para os conectores LDAP genérico, SQL genérico, serviços Web, PowerShell, Graph e Lotus Domino pode ser encontrado no [histórico de lançamento de versão do conector](microsoft-identity-manager-2016-connector-version-history.md).  

## <a name="mim-version-462630"></a>4.6.263.0 versão do MIM
- Status: 7 de agosto de 2020
- [Download do hotfix](https://www.microsoft.com/download/details.aspx?id=101612)
- [Artigo 4576473 da base de conhecimento](https://support.microsoft.com/help/4576473)

Este hotfix contém atualizações para o MIM CM, o Gerenciador de sincronização do MIM e os componentes do PAM.


## <a name="mim-version-462580"></a>4.6.258.0 versão do MIM
- Status: 22 de maio de 2020
- [Download do hotfix](https://www.microsoft.com/download/details.aspx?id=101305)
- [Artigo 4560335 da base de conhecimento](https://support.microsoft.com/help/4560335)

Este hotfix contém atualizações para o serviço do MIM, portal do MIM e componentes do PAM.


## <a name="mim-version-46340"></a>4.6.34.0 versão do MIM
* Status: MIM 2016 Service Pack 2 (SP2) de outubro, 2019
* Número de versão do BHOLD correspondente: 6.0.62.0
- [Download do hotfix](https://www.microsoft.com/download/details.aspx?id=100412)
- [Artigo 4512924 da base de conhecimento](https://support.microsoft.com/help/4512924)
- O arquivo ISO pode ser baixado [de VLSC](https://www.microsoft.com/Licensing/servicecenter/default.aspx) ou por meio de  [downloads do Visual Studio](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202)

> [!IMPORTANT]
>- .NET Framework 4,6 é necessário<br>
>- [Visual C++ pacote redistribuível 2013 (vcredist_x64.exe ou vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784) é necessário<br>

>[!NOTE]
> Leia o guia de [implantação do mim](../microsoft-identity-manager-deploy.md) para obter uma lista atualizada de pré-requisitos e limitações relacionadas a ambientes somente TLS 1,2, suporte a contas de serviço gerenciado de grupo e o [caminho de atualização das versões anteriores do mim e do fim](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md).

Um pacote de rollup do Service Pack 2 (SP2) (Build 4.6.34.0) está disponível para o MIM (Microsoft Identity Manager) 2016. É uma atualização cumulativa que substitui as atualizações do MIM 2016 SP1 anteriores 4.4.1302.0 por meio do Build 4.5.412.0.
### <a name="updates-in-mim-2016-service-pack-2"></a>Atualizações no MIM 2016 Service Pack 2

#### <a name="mim-client-addons"></a>Complementos de cliente MIM
- Suporte adicionado para que o complemento do Outlook do MIM seja carregado no Outlook para Microsoft 365 versão de clique para executar.

#### <a name="service-and-portal"></a>Serviço e Portal
- Suporte adicionado para o serviço e portal do MIM a ser instalado no Windows Server 2019 e use SQL Server 2017, Exchange Server 2019, SharePoint 2019 System Center Service Manager data warehouse 2019
- Habilitada a instalação do serviço e portal do MIM somente em ambientes TLS 1,2.
- Instalação habilitada para o serviço do MIM, redefinição de senha e sites de registro de senha, serviço de monitoramento do PAM, serviço de componente do PAM para usar contas de serviço gerenciado de grupo.
- Parâmetro do instalador ' keepSQLjobs ' adicionado.
- O MIM SQL Server Agent trabalhos temporais não são mais iniciados em réplicas do grupo de disponibilidade do SQL Always-On secundário.
- Os atributos virtuais ' ExplicitMember. Add ' e ' ExplicitMember. Remove ' estão habilitados para tipos de objeto personalizados em formulários do RCDC para trabalhar com alterações delta.

#### <a name="synchronization-service"></a>Serviço de Sincronização
- Suporte adicionado para que o serviço de sincronização do MIM seja instalado no Windows Server 2019 e use SQL Server 2017, Exchange Server 2019
- Habilitada a instalação do serviço de sincronização do MIM somente em ambientes TLS 1,2.
- Instalação habilitada para o serviço de sincronização do MIM usar uma conta de serviço gerenciado de grupo.
- Adicionada a opção ' usar conta de MIMSync ' para o agente de gerenciamento de serviço do MIM.
 
#### <a name="privilege-access-management"></a>Gerenciamento de acesso de privilégio 
- O cmdlet ' Get-PAMRequest ' do PowerShell retorna uma propriedade adicional ' FIMRequestID '.


## <a name="mim-version-454120"></a>4.5.412.0 versão do MIM
> [!IMPORTANT]
>- O formulário RCDC para objetos de grupo falha ao renderizar quando o valor do atributo ' displayedOwner ' não é populado. Você não poderá editar/exibir grupos quando esse erro ocorrer.<br>

- Data de disponibilidade: 10 de maio de 2019
- [Download do hotfix](https://www.microsoft.com/en-us/download/details.aspx?id=58213)
- [Artigo 4489646 da base de conhecimento](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

Este hotfix contém atualizações para sincronização do MIM, serviço do MIM, portal do MIM, MIM CM e componentes do PAM.  É uma atualização cumulativa que substitui as atualizações do MIM 2016 SP1 anteriores 4.4.1302.0 por meio do Build 4.5.286.0.

> [!IMPORTANT]
>- .NET Framework 4,6 também é necessário para o instalador <br>
>- [Visual C++ os pacotes redistribuíveis do 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) são necessários<br>

## <a name="mim-hybrid-reporting-agent-version-31410"></a>Agente de relatório híbrido do MIM versão 3.1.41.0
- Data de disponibilidade: 22 de abril de 2019
- [Download do agente de relatório híbrido](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

O agente de relatório híbrido do MIM versão 3.1.41.0 inclui atualizações para o TLS 1,2 e uma correção de bug.  O agente de relatório híbrido pode ser usado com as versões 4.4.1302.0 ou posteriores do MIM 2016 SP1 e uma assinatura Azure AD Premium.


## <a name="mim-version-452860"></a>4.5.286.0 versão do MIM
- Status: 19 de novembro de 2018
- [Download do hotfix](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [Artigo 4469694 da base de conhecimento](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


Este hotfix contém atualizações para o serviço do MIM, portal do MIM e componentes do PAM.  .NET Framework 4,6 é necessário para o instalador.


## <a name="version-452020"></a>4.5.202.0 da versão
- Status: 30 de agosto de 2018
- [KB versão KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- .NET Framework 4,6 também é necessário para o instalador <br>
>- [Visual C++ os pacotes redistribuíveis do 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) são necessários<br>
>- Localidades com suporte atualizadas para novos padrões ISO ([aqui](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Denota novo aprimoramento 

#### <a name="mim-service"></a>Serviço MIM
- Integração do servidor do Azure MFA – provedor de autenticação multifator alternativo

#### <a name="privilege-access-management"></a>Gerenciamento de acesso de privilégio 
- A API REST do PAM não pôde ser iniciada porque não pôde carregar o arquivo ou o assembly

#### <a name="microsoft-identity-portal"></a>Portal de identidade da Microsoft
- O portal é exibido com um comprimento de tabela incorreto
- Caixa de diálogo pesquisa avançada do portal, as barras de rolagem não são exibidas corretamente
- Falha na verificação da assinatura de nome forte do pacote de idiomas do pacote de idiomas

#### <a name="certificate-management"></a>Gerenciamento de certificado
- Instrução de redirecionamento de associação para a API REST

## <a name="version-45260"></a>4.5.26.0 da versão
- Status: 30 de junho de 2018
- [KB de versão KB4073679](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- .NET Framework 4,6 também é necessário para o instalador <br>
>- [Visual C++ os pacotes redistribuíveis do 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) são necessários<br>
>- Localidades com suporte atualizadas para novos padrões ISO ([aqui](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Denota novo aprimoramento 

#### <a name="synchronization-service"></a>Serviço de sincronização
- * Suporte para contas de serviço gerenciado de grupo
- * Suporte ao Visual Studio (Visual Studio 2013, Visual Studio 2015, Visual Studio 2017)
- Atualizações para MIISACTIVATE.EXE, suporte a gMSA adicionado 
    - Não gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService" *
    - gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService"
- Atualizações para MIISKMU.exe, suporte a gMSA adicionado 
    - non-gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "
    - gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "*
- As informações de partição atualizadas são salvas como esperado quando os botões atualizar e OK são clicados
- Ao indexar um atributo de cadeia de caracteres indexável é muito longo, um erro inesperado foi retornado; uma mensagem de erro mais descritiva agora é retornada
- Criando um agente de gerenciamento de arquivos de texto quando o serviço de sincronização do MIM está instalado no Windows Server 2016, algumas opções de codificação de texto, incluindo Unicode, não estavam disponíveis
- Serviço do MIM MA se uma mensagem de erro de exportação contiver um caractere inválido, isso causará corrupção nas entradas do histórico de execução. Essa compilação foi removida da mensagem de erro antes de ser salva no objeto de espaço do conector e no histórico de execução

#### <a name="mim-service"></a>Serviço MIM
- * Suporte para contas de serviço gerenciado de grupo
- * Suporte a idioma aprimorado para o novo padrão definido
- * FIMAutomation cmdlet do PowerShell Export-FIMConfig o argumento "-PamConfig" está disponível para forçar a exportação dos objetos de configuração do PAM
- * FIMAutomation Export-FIMConfig cmdlet do PowerShell o parâmetro "-Request" foi adicionado
- * Os atributos boolianos são sempre definidos como NULL na criação da associação, o booliano anterior antes do hotfix não será atualizado
> [!IMPORTANT]
>Isso pode ser uma alteração significativa se estiver preformando uma migração de configuração. A configuração deve ser avaliada e atualizada para o novo recurso, pois a migração de configuração é considerada uma nova 
    - Implementada a inicialização de novos atributos boolianos do MIM como false ao criar novo objeto
    - Implementada a inicialização de novos atributos boolianos do MIM como false na adição da nova associação de atributo booliano ao recurso
- Programa de Aperfeiçoamento da Experiência do Usuário configuração é mantida como false 
- Falha na instalação do serviço do MIM com erro de atualização do banco de dados: não é possível inserir o valor NULL na coluna ' name ' se não for usado o nome de banco de dados
- Em casos de hotfix, a configuração de Microsoft 365 seria desmarcada, a senha criptografada para a caixa de correio do Exchange Online do serviço do MIM não é alterada
- * Não havia nenhum limite para o arquivo de log do serviço do MIM criado, configuração padrão de log atualizado e funcionalidade de registro em log circular implementada

#### <a name="privileged-access-management"></a>Gerenciamento de Acesso Privilegiado 
- * Suporte para contas de serviço gerenciado de grupo
- * Suporte a idioma aprimorado para o novo padrão definido
- Os objetos que usam recursos não gerenciados não são limpos no tempo.  esses objetos serão limpos corretamente
- * Cmdlet New-PAMRole do PowerShell o "-disableAutoApproveIfOwner" negar a autoaprovação para a função
- * Get-PamRequest cmdlet do PowerShell, o "-CreatedFrom" permite a solicitação específica do PAM de filtragem de OD
- * Adições de módulo do PAM
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- O aviso (exceção: System. ObjectDisposedException: não é possível acessar um objeto Descartado) não será mais exibido no log de eventos do PAM
- Set-PAMUser cmdlet é capaz de alterar o PrivAccountName sem a exclusão
- Agora New-PamRole valida que a data "disponível para" é maior que a data "disponível de"
- Os valores "disponíveis de" e "disponíveis para" são retornados pelo cmdlet Get-PAMRole PowerShell
- O filtro de cmdlets Get-PamRequest agora está corretamente
- * O cmdlet Set-PamGroup agora é capaz de atualizar o objeto de grupo de entidades de sombra de Active Directory
- Remove-PamUser cmdlet do PowerShell falha com uma mensagem de erro não clara, se o usuário estiver vinculado a uma função como um candidato. Agora, a validação no lado do cliente foi adicionada ao cmdlet e a exceção retornada foi esclarecida
- As contas do PAM do modo de alteração não são expostas para configuração
    - Conta da API REST do PAM
    - Conta de serviço de componente do PAM
    - Conta de serviço de monitoramento do PAM

#### <a name="microsoft-identity-portal"></a>Portal de identidade da Microsoft
- * Suporte para contas de serviço gerenciado de grupo
- * Suporte a idioma aprimorado para o novo padrão definido
- Controle do seletor de identidade, o controle parece aumentar dinamicamente sua largura em vez de encapsular o texto
- O portal, as caixas de diálogo pop-up não são exibidas corretamente ao exibir no Internet Explorer (IE) 10
- Os símbolos cirílicos no texto da barra de título são exibidos corretamente
- As janelas pop-up não têm mais a exibição da barra de rolagem extra, quando exibidas no Internet Explorer
- Falha ao "importar definição de fluxo de trabalho" gera corretamente uma exceção e recupera, permitindo que uma atividade de regra de sincronização seja adicionada à definição de fluxo de trabalho
- <httpRuntime enableVersionHeader="false" /> adicionado ao web.config padrão
- Os caracteres especiais no distinguishedName não impedem que Self-Service redefinição de senha redefina a senha do usuário no Active Directory
- As melhorias nas frases estão localizadas corretamente na tela
- O suplemento do MIM para Outlook inclui uma cópia dos binários de interoperabilidade do Outlook ausentes

#### <a name="certificate-management"></a>Gerenciamento de certificado
- Renovando um cartão inteligente virtual por meio do MIM CM aplicativo moderno, o usuário recebe exceção proibida
- * Suporte a idioma aprimorado para o novo padrão definido
- O utilitário de fixação "CLM encontrou um erro ao tentar alterar o PIN do cartão inteligente.  Número incorreto de argumentos ou atribuição de propriedade inválida. "
- Atualizar para os módulos de autoridade de certificação do MIM do 4.4.1302.0 para uma compilação posterior a 4.4.1459, a instalação falha
- Aplicativo moderno para renovar, registrar e substituir operações, o histórico de solicitações não contém todos os itens de status de solicitação como são registrados
- A atualização online não é concluída e retorna a exceção "o registro foi atualizado ou excluído por outro usuário".  
- O link "baixar certificado" no Portal de Gerenciamento do certificado, o download do certificado (arquivo. cer) era muito grande
- O cliente em massa do gerenciamento de certificados do MIM funcionará com o TLS 1,1 e o TLS 1,2.  

 
## <a name="version-4417490"></a>4.4.1749.0 da versão

- Status: 30 de novembro de 2017
- [Download](https://www.microsoft.com/download/details.aspx?id=56271)

### <a name="fixed-issues"></a>Problemas corrigidos
Os problemas a seguir foram corrigidos na versão 4.4.1749.0 do MIM.

#### <a name="synchronization-service"></a>Serviço de sincronização

- A rotina de redefinição de senha falha quando o domínio do servidor de sincronização não tem relação de confiança com o domínio de destino.

#### <a name="mim-service"></a>Serviço MIM

- Falha na atualização do banco de dados. Uma exceção de violação de restrição de chave estrangeira é registrada no log de atualização do banco de dados.
- Quando você executa solicitações de redefinição de senha de autoatendimento, o serviço do MIM é interrompido aleatoriamente.
- Falha ao definir o cálculo para refletir a associação correta. Você não poderá excluir uma associação para um atributo se ele for referenciado em um conjunto dinâmico ou filtro de grupo.
- O serviço do MIM não funcionou para o cenário de aprovação de solicitação com o Exchange Online, em que as aprovações são respondidas por meio do suplemento do MIM para Outlook.
- O atributo msidmPhoneGatePhoneNumber sem um código de país não usa o valor DefaultCountryCode em MFASettings.xml.
- As associações definidas podem ser atualizadas dinamicamente sem a necessidade de contar com o FIM_TemporalEventsJob.
- As regras de sincronização não dão suporte à criação de regras de fluxo de atributo para atributos cujos nomes incluem o hash ou símbolo de libra (#).
  
#### <a name="privilege-access-management"></a>Gerenciamento de acesso de privilégio 

- New-PAMDomainConfiguration cmdlet do PowerShell define um valor incorreto para a configuração de confiança do domínio resultando em erro (esse parâmetro de solicitação desconhecido não pode ser processado).

#### <a name="microsoft-identity-portal"></a>Portal de identidade da Microsoft

- Uma exceção é exibida na tela principal do Portal de Gerenciamento de identidade e um botão fechar também é exibido.
- Botões exibidos incorretamente na janela Excluir item.Esse problema ocorreu no Internet Explorer, no Firefox e no Chrome. 
- O botão de pesquisa sobrepõe o botão do seletor de recursos em uma janela de atividade de aprovação no fluxo de trabalho de autorização. Esse problema ocorreu no Internet Explorer, no Firefox e no Chrome. 
- No pop-up Propriedades do grupo, a área do botão sobrepõe os controles de navegação ListView no controle excluir Membros.Esse problema ocorreu no Internet Explorer, no Firefox e no Chrome.
- Vários elementos da interface do usuário não são exibidos corretamente. Os seguintes elementos são fixos:

    - Setas para cima e para baixo em algumas folhas de propriedades.
    - Espaço vazio na parte inferior de algumas páginas e caixas de diálogo.
    - Sobreposições de Popup ausentes.

- Quando você usa o construtor de filtro em várias áreas do produto (como pesquisa avançada), o construtor de filtro fica "preso" se o botão OK em uma caixa de diálogo Selecionar valor for clicado sem um objeto selecionado na área adicionar instrução.
- O novo pop-up de fluxo de atributo em uma caixa de diálogo de edição de regra de sincronização não funcionou conforme o esperado no Chrome.
- Em uma tela de gerenciamento de objetos (como grupos de distribuição), se vários objetos forem selecionados usando a caixa de seleção e se os objetos tiverem nomes de exibição longos. Os tamanhos de caixa de diálogo agora são verticalmente para que o controle não ultrapasse o final da tela do navegador.
- Em uma tela de gerenciamento de objetos ou de lista (como grupos de distribuição), o controle itens selecionados pode mover a tela para que fique diretamente sob o último objeto listado nas listas de tabelas.
- O construtor de filtro (como pesquisa avançada) no navegador Safari não funciona.
- caixas de diálogo do portal que exibem valores de atributo, as palavras mais curtas são distribuídas em toda a célula com muito espaço em branco entre, em vez de serem alinhadas à esquerda. 
- Em algumas versões do navegador, os itens selecionados não são atualizados quando a seleção do item é alterada.
- As guias de caixa de diálogo e copiar para área de transferência realçam quando navegadas pelo usando a tecla Tab.  
- No Internet Explorer 10, quando você exibe uma exibição de grade de objetos (como grupos de distribuição), a parte "localizar os grupos de distribuição desejados usando a pesquisa acima" sobrepõe as sobreposições de faixa da faixa de opções do botão em vez de exibi-la no meio da caixa de diálogo.  
- Depois de instalar uma atualização no portal do MIM, a exibição do portal no Internet Explorer falhará.
- Quando você usa a pesquisa avançada no navegador Firefox, pressionar o campo Inserir chave em um valor de atributo retorna um erro.  

#### <a name="certificate-management"></a>Gerenciamento de certificado

- Um originador de solicitação (Gerenciador de certificados) não pode abandonar uma solicitação que está duplicada ou esquecida por um usuário que tem permissão de execução.
- Quando você tenta renovar o cartão inteligente virtual do TPM do aplicativo moderno, uma exceção proibida é retornada.
- Durante algumas atividades de cartão inteligente, as conexões existentes com o banco de dados CertificateManagement são deixadas abertas inesperadamente.  
- Se uma instalação de uma atualização para o gerenciamento de certificados do MIM (CM) for tentada antes da execução do assistente de configuração do MIM CM, a atualização falhará com uma exceção que parece não estar relacionada ao problema.
- O assistente de configuração de MIM CM tem informações de versão de produto incorretas exibidas e o logotipo não é exibido corretamente.  
- Os dados exportados para um relatório de gerenciamento de certificados do MIM diferem dos dados do relatório.Os dados da coluna nem sempre correspondem aos títulos das colunas.

## <a name="version-4416420"></a>4.4.1642.0 da versão

- Status: 29 de agosto de 2017
- [Download](https://www.microsoft.com/download/details.aspx?id=55794)

### <a name="fixed-issues"></a>Problemas corrigidos
Os problemas a seguir foram corrigidos na versão 4.4.1642.0 do MIM.

#### <a name="synchronization-service"></a>Serviço de Sincronização

- A rotina de redefinição de senha falha quando o domínio do servidor de sincronização não tem relação de confiança com o domínio de destino.
- O filtro de importação declarado (verificando o nome diferenciado) não funciona corretamente quando um objeto é movido de uma unidade organizacional onde deve ser filtrado para outro onde não deveria estar por usar o nome antigo de um objeto fantasma.
- O designer do agente de gerenciamento trava na página "configurar partições e hierarquias".
- A precedência não é transferida para o próximo objeto quando o anterior com precedência é desconectado.
- O Sun One Management Agent pesquisando contêineres filho no servidor LDAP usando a paginação causará um erro se o servidor não der suporte à paginação.
- O tipo de objeto do metaverso é alterado dinamicamente causando falha.

#### <a name="mim-service"></a>Serviço MIM

- A função Word não retornará uma cadeia de caracteres vazia se a cadeia de caracteres contiver menos do que o número de palavras.
- A interface do usuário exibir membros mostra a associação incorreta quando os critérios contêm cancelamento de referência e se a desreferência ocorre abaixo de outra parte dos critérios. 
- O fluxo de trabalho AuthZ nega a solicitação com a mensagem de erro ' fluxo de trabalho não encontrado no repositório de persistência de estado '.
- O fluxo de trabalho executa uma atividade enumerar recurso para consultar o MIM e ele está falhando intermitentemente.

#### <a name="privilege-access-management"></a>Gerenciamento de acesso de privilégio

- Get-PAMRequestToApprove commandlet não retornará candidatos se o aprovador não estiver na função para aprovar.
- O MPRs relacionado e o nó da barra de navegação do PAM são habilitados mesmo quando o PAM não está instalado.
- O serviço do MIM gera avisos de exceção e logs do sincronizador de configuração de domínio para o PAM.

#### <a name="microsoft-identity-portal"></a>Portal de identidade da Microsoft

- Ao usar o Firefox para acessar o construtor de filtro no portal do MIM, você não poderá usar os controles internos (caixas suspensas, caixas de texto e assim por diante). Eles não podem ser selecionados até você clicar com o botão direito do mouse (ou seja, clicar para exibir o menu de contexto).
- A pesquisa do portal é renderizada incorretamente em algumas resoluções de tela. 
- O controle de calendário na pesquisa avançada está truncado.
- A interface do usuário do construtor de filtro foi quebrada – no modo de edição (elementos no local incorreto).
- O Popup tinha tamanho fixo e os controles de edição não eram dimensionados adequadamente.
- A cadeia de caracteres é recortada para a Noruega e alguns outros idiomas no menu principal.
- A URL copiada do Popup não está funcionando.

#### <a name="certificate-management"></a>Gerenciamento de certificado

- Portais de senha de autoatendimento do MIM.

#### <a name="reporting"></a>Relatórios

- Import-FIMReportingSchemaDefinition cmdlet para relatórios falha com erro.

#### <a name="client-add-in"></a>Suplemento de cliente

- O idioma da interface do usuário não verifica a igualdade do código do país.

### <a name="new-features-and-improvements"></a>Novos recursos e aprimoramentos
Os seguintes recursos e aprimoramentos foram adicionados na versão 4.4.1642.0 do MIM.

#### <a name="mim-service"></a>Serviço MIM

- Adição de repetição nas operações de processamento de solicitação mais longas (estágio de validação). Ele não garante que o processamento da solicitação seja concluído, mas torna as solicitações mais estáveis. A correção está desabilitada por padrão. Para habilitar a correção, você adiciona alwaysOnRetryRequestProcessingTransaction = "true" na seção resourceManagementService do arquivo de configuração FIMService

#### <a name="certificate-management"></a>Gerenciamento de certificado

- As versões mais recentes do CM Server são capazes de trabalhar com BulkClient mais antigas (mas não com mais de 4.4. xxxx. 0). 

#### <a name="mim-self-service-password-portals"></a>Portais de senha de autoatendimento do MIM 

- Mostrar aviso para QAGate se usado fornecer uma resposta que contenha caractere de byte duplo
- Adicionar Propriedade configurável para permitir habilitar/desabilitar o uso do IME no formulário de registro do SSPR 
- SSPR mascara as perguntas de registro de senha no cliente do portal

## <a name="version-4415490"></a>4.4.1549.0 da versão
 
* Status: atualização do hotfix do MIM 2016 SP1 anterior, de 27 de março de 2017
* Número de versão do BHOLD correspondente: 6.0.36.0
* Leia mais em [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## <a name="version-4413020"></a>4.4.1302.0 da versão

* Status: MIM 2016 Service Pack 1 (SP1) de 9 de novembro de 2016
* Número de versão do BHOLD correspondente: 5.0.3355.0

### <a name="updates-in-this-service-pack"></a>Atualizações neste service pack


#### <a name="mim"></a>MIM

- **Compatibilidade de navegadores no Portal do MIM de autoatendimento de usuário final:** neste Service Pack, introduzimos o suporte para a maioria dos principais navegadores. Agora, os usuários podem acessar e interagir com o Portal do MIM para gerenciamento de autoatendimento de perfil e de grupo no Microsoft Edge, Chrome e Safari.

- **Suporte ao Serviço do MIM para Exchange Online:** há tempos o Serviço do MIM oferece suporte ao envio e recebimento de emails para aprovações e notificações. Antes do SP1, o MIM só fornecia suporte ao Exchange Server ou SMTP. Com o service pack 1, o serviço do MIM pode enviar e receber solicitações, bem como notificações por email usando uma conta do Microsoft 365 Exchange Online.

- **Validação do formato de arquivo de imagem no carregamento:** o MIM agora é capaz de validar o formato de arquivo de imagens quando forem carregados no portal.

#### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Suporte a floresta "PRIV" (bastião) PAM para o nível funcional do Windows Server 2016:** o Serviço MIM PAM podem ser configurado em um ambiente com controladores de domínio em execução no nível funcional de floresta dos Active Directory Domain Services do Windows Server 2016. Quando configurado, o tíquete do Kerberos do usuário terá uma limitação tempo para o tempo restante da ativação de sua função.

    >[!Note]
    Se você optar por manter o nível funcional de floresta do Windows Server 2012 R2 no domínio CORP, recomendamos a instalação do [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) e do [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) no controlador de domínio CORP.

- **Elevação de conta privilegiada em grupos exclusivos para a floresta "PRIV" (bastião):** agora, os administradores podem informar ao Serviço do MIM sobre grupos e usuários exclusivos à floresta "PRIV". Isso permite que esses usuários e grupos sejam incluídos em funções do PAM.  Eles podem ser ativados para uma função e receber a associação a grupos na floresta "PRIV".

- **Scripts de implantação do PAM:** os scripts de implantação do PAM permitem que os administradores agilizem a instalação do ambiente do PAM.

- **Cmdlets do PAM para configuração do Silo de política de autenticação:** o service pack 1 introduz novos Cmdlets para proteger a segurança de sua floresta de bastiões. Esses Cmdlets criam automaticamente um Silo de política de autenticação, associado a um Modelo de política de autenticação.

    >[!Note]
    Esses Cmdlets são executados automaticamente como parte dos scripts de implantações.



### <a name="platform-support"></a>Suporte a plataforma 

Encontre informações atualizadas de suporte de plataforma no documento chamado [Plataformas com suporte para MIM 2016](../microsoft-identity-manager-2016-supported-platforms.md).  As novas plataformas com suporte neste service pack incluem SQL Server 2016, SharePoint 2016.


### <a name="issues-fixed-in-this-release"></a>Problemas corrigidos nesta versão


#### <a name="pam"></a>PAM
- New-PAMGroup não criou objetos MIM para grupos locais de domínio na floresta PRIV
- New-PAMDomainConfiguration falharia com uma mensagem de erro "netdom"
- Serviço de Monitoramento do PAM registrou avisos para grupos na floresta PRIV


### <a name="how-to-upgrade"></a>Como atualizar

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

## <a name="version-4322660"></a>4.3.2266.0 da versão


* Status: hotfix do MIM 2016 de 15 de julho de 2016; Os clientes devem atualizar para uma versão mais recente para permanecer com suporte
* Leia mais em [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## <a name="version-4321950"></a>4.3.2195.0 da versão

* Status: hotfix do MIM 2016 de 20 de março de 2016, os clientes devem atualizar para uma versão mais recente para permanecer com suporte
* Número de versão do BHOLD correspondente: 5.0.3355.0
* Leia mais em [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
    
## <a name="version-4320640"></a>4.3.2064.0 da versão

* Status do hotfix MIM 2016 de 11 de dezembro de 2015; Os clientes devem atualizar para uma versão mais recente para permanecer com suporte
* Número de versão do BHOLD correspondente: 5.0.3176.0
* Leia mais em [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## <a name="version-4319350"></a>4.3.1935.0 da versão

* Status MIM 2016 GA de 6 de agosto de 2015; Os clientes devem atualizar para uma versão mais recente para permanecer com suporte
* Número de versão do BHOLD correspondente: 5.0.3079.0

