---
title: Recursos preteridos do MIM e planejamento para o futuro | Microsoft Docs
description: Este artigo documenta os recursos preteridos do MIM (Microsoft Identity Manager) 2016 SP1.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: a4239f1d69d8a43d70dd38af16e9ef8be62bd33c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288904"
---
# <a name="deprecated-features"></a>Recursos preteridos

Este artigo descreve os recursos preteridos do Microsoft Identity Manager 2016 SP1. O recurso terá suporte até o momento nos locais em que ainda estiver presente no Microsoft Identity Manager. Os recursos não são recomendados para as novas implantações, uma vez que eles poderão ser removidos em uma versão do recurso.  Para os desenvolvedores, é recomendável não utilizar recursos preteridos em novos aplicativos ou em novas soluções.

> [!NOTE]
> As funcionalidades e os recursos removidos do Microsoft Identity Manager SP1 são identificados com **. <br>
> Para obter mais informações sobre o [ciclo de vida de suporte para o Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft não recomenda que os clientes iniciem novas implantações dos componentes do Microsoft BHOLD Suite. As implantações existentes do BHOLD continuarão a receber suporte. O Azure AD agora fornece [revisões de acesso](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), que substituem alguns dos recursos de campanha de atestado BHOLD.

## <a name="certificate-management"></a>Gerenciamento de certificado 

| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de gerenciamento | **Gerenciamento de certificado FIM | O agente de gerenciamento de certificado FIM foi removido no MIM 2016.                                                             |

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração do serviço Web (ma-data e mv-data) | A capacidade de configurar o serviço de sincronização FIM por meio do serviço Web FIM será removida em uma próxima versão.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração do serviço Web | A capacidade de configurar o serviço de sincronização FIM por meio do serviço FIM será removida em uma próxima versão.                                                          |
| Agentes de gerenciamento           | MAs interno                        | O MAs a seguir foi removido do MIM 2016: </br> 1.  **MA para o gerenciamento de certificado FIM </br>2.  **MA para o Lotus Notes</br> 3.  **MA para o SAP r/3 </br> Os MAs do Lotus Notes e do SAP r/3 foram substituídos por novas versões. Para obter mais informações, consulte [Latest Connector Version Release History & Download](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history) (Histórico de lançamento de versão do conector mais recente e Download)                                                                                                                                                                                                                                              |
| Agentes de gerenciamento           | ECMA1                               | A estrutura de extensibilidade ECMA1/XMA foi substituída pelo ECMA 2.0. É necessário atualizar os agentes de gerenciamento ECMA1 com os conectores ECMA2.0.                                                                                                                                          |
| Agentes de gerenciamento           | Conectores fora do processo em execução      | Este recurso não será substituído. O serviço de sincronização sempre chamará o conector no mesmo processo. É responsabilidade do conector iniciar e gerenciar os outros processos. |
| Agentes de gerenciamento           | Configurar o nome de exibição da partição    | Este recurso não será substituído. Esta opção só foi utilizada para fornecer um nome alternativo para uma partição em interfaces do WMI.                                                                                                                                                                       |
| Perfis de execução                | Perfis combinados                   | Os perfis combinados importação/sincronização delta, importação completa/sincronização delta e importação/sincronização completas serão removidos. Como alternativa, utilize os perfis de execução com duas etapas. 

> [!NOTE]
> Mantenha os perfis de execução combinados apenas em ambientes em que o desempenho será afetado por um grande número de desconectores existentes.


| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|--------|-------|---|    
| Prioridade de atributo | Vários domínios/precedência igual                       | A precedência igual será removida. Não há nenhuma substituição para este recurso. Como alternativa, configure a precedência manual. Você poderá continuar a usar este recurso se o seu ambiente tiver um agente de gerenciamento de serviço FIM implantado. Este agente de gerenciamento não fornece precedência manual para evitar a exportação sem precedentes para o provisionamento declarativo. |
| Regras de união           | Ingressar em “Qualquer” tipo de objeto                             | Este recurso não será substituído. Todas as regras de associação devem definir explicitamente o tipo de objeto do metaverso no qual estão tentando ingressar.       |
| Fluxos de atributos      | Cancelar a seleção da opção “permitir valores nulos” para os valores exportados            | Este recurso não será substituído. A opção “Permitir valores nulos” sempre será selecionada. Confira se a opção “Permitir valores nulos” foi selecionada em seu ambiente atual.  |
| Fluxos de atributos      | “Não recuperar atributos”                            | Este recurso não será substituído. Os atributos sempre serão recuperados, o que é a prática recomendada.  |
| Extensão das regras      | Executar o metaverso e a extensão das regras MA fora do processo | Este recurso não será substituído. As regras de fluxo de atributo e o metaverso serão executados no mesmo processo que o mecanismo de sincronização.       |
| Extensão das regras      | Propriedades da transação                                | Este recurso não será substituído. Evite transmitir dados entre a sincronização de entrada, de provisionamento e de saída usando essa classe de utilitário.  |
| Extensão das regras      | Métodos ExchangeUtils: Create55\*                     | Os métodos para criar objetos para os servidores Exchange 5.5 serão removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros da classe ClmUtils serão removidos em uma próxima versão.   |

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre:

- O Microsoft Identity Manager ainda está muito relacionado ao seu predecessor, o Forefront Identity Manager. Se você ainda usa o FIM ou deseja consultar a documentação adicional, veja o [Roteiro da documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações de topologia para implantação do MIM](topology-considerations.md). Este artigo apresenta várias topologias de implantação que você pode considerar.
- [Guia de planejamento de capacidade](capacity-planning-guide.md) Use este guia em ambientes de teste para entender o escopo apropriado da sua implantação.
