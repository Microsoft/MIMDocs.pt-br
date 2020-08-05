---
title: Recursos preteridos do MIM e planejamento para o futuro | Microsoft Docs
description: Este artigo documenta os recursos preteridos do MIM Identity Manager 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 7/28/2020
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: cb0159ec70e161dc47140712bd2ac039786e034d
ms.sourcegitcommit: 50cee02a7146445bd6fa361349099c7b294792d8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87443553"
---
# <a name="deprecated-features"></a>Recursos preteridos

Este artigo descreve os recursos preteridos do Microsoft Identity Manager 2016 SP2. O recurso terá suporte até o momento nos locais em que ainda estiver presente no Microsoft Identity Manager. Os recursos não são recomendados para as novas implantações, uma vez que eles podem ser removidos em um hotfix futuro ou uma versão do service pack.  Para os desenvolvedores, é recomendável não utilizar recursos preteridos em novos aplicativos ou em novas soluções.

> [!NOTE]
>
> Para saber mais sobre as novas opções de suporte ao MIM, confira as [opções de suporte para os clientes do Azure AD Premium](support-update-for-azure-active-directory-premium-customers.md).

## <a name="bhold"></a>BHOLD

A Microsoft não recomenda que os clientes iniciem novas implantações dos componentes do Microsoft BHOLD Suite. As implantações existentes do BHOLD continuarão a receber suporte. Agora o Azure AD oferece [revisão de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), uma função que substitui os recursos de campanha de atestado BHOLD. Além disso, fornece o gerenciamento de direitos, que substitui os recursos de atribuição de acesso.

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Recurso preterido**              | **Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática da sincronização | Interface de configuração do serviço Web (ma-data e mv-data) | A capacidade de configurar o serviço de sincronização MIM por meio do serviço Web do MIM pode ser removida em um hotfix ou service pack futuro.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

O MAs a seguir foi removido do MIM 2016: </br> 1.  MA para o gerenciamento de certificado FIM </br>2.  MA para o Lotus Notes</br> 3.  MA para o SAP R/3 </br> Os MAs do Lotus Notes e do SAP R/3 foram substituídos com novos conectores. Para saber mais, confira [Histórico de lançamento de versão do conector mais recente e download](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).

A estrutura de extensibilidade ECMA1/XMA foi substituída pelo ECMA 2.0. É necessário atualizar os agentes de gerenciamento ECMA1 com os conectores ECMA2.0.

| **Categoria**                | **Recurso preterido**              | **Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de gerenciamento           | Conectores fora do processo em execução      | O serviço de sincronização sempre chamará o conector no mesmo processo. É responsabilidade do conector iniciar e gerenciar os outros processos. |
| Agentes de gerenciamento           | Configurar o nome de exibição da partição    | Esta opção só foi utilizada para fornecer um nome alternativo para uma partição em interfaces do WMI.                                                                                                                                                                       |
| Perfis de execução                | Perfis combinados                   | Os perfis combinados importação/sincronização delta, importação completa/sincronização delta e importação/sincronização completas podem ser removidos. Como alternativa, use os perfis de execução com duas etapas.

> [!NOTE]
> Mantenha os perfis de execução combinados apenas em ambientes em que o desempenho será afetado por um grande número de desconectores existentes.

| **Categoria**                | **Recurso preterido**              | **Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Precedência de atributo | Vários domínios/precedência igual                       | A precedência igual pode ser removida. Como alternativa, configure a precedência manual. Você poderá continuar a usar este recurso se o seu ambiente tiver um agente de gerenciamento de serviço FIM implantado. Este agente de gerenciamento não fornece precedência manual para evitar a exportação sem precedentes para o provisionamento declarativo. |
| Regras de união           | Ingressar em “Qualquer” tipo de objeto                             | Todas as regras de associação devem definir explicitamente o tipo de objeto do metaverso no qual estão tentando ingressar.       |
| Fluxos de atributos      | Cancelar a seleção da opção “permitir valores nulos” para os valores exportados            | "Permitir valores nulos" sempre estará selecionado. Confira se essa opção está selecionada no ambiente atual.  |
| Fluxos de atributos      | “Não recuperar atributos”                            | Os atributos sempre serão recuperados, o que é a prática recomendada.  |
| Extensão das regras      | Executar o metaverso e a extensão das regras MA fora do processo | As regras de fluxo de atributo e o metaverso serão executados no mesmo processo que o mecanismo de sincronização.       |
| Extensão das regras      | Propriedades da transação                                | Evite transmitir dados entre a sincronização de entrada, de provisionamento e de saída usando essa classe de utilitário.  |
| Extensão das regras      | ExchangeUtils: Métodos \*Create55                     | Os métodos para criar objetos para os servidores Exchange 5.5 podem ser removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros da classe ClmUtils podem ser removidos em um hotfix ou service pack futuro.   |

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre:

- O Microsoft Identity Manager ainda está muito relacionado ao seu predecessor, o Forefront Identity Manager. Se você ainda usa o FIM ou deseja consultar a documentação adicional, veja o [Roteiro da documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações de topologia para implantação do MIM](topology-considerations.md). Este artigo apresenta várias topologias de implantação que você pode considerar.
- [Guia de planejamento de capacidade](capacity-planning-guide.md) Use este guia em ambientes de teste para entender o escopo apropriado da sua implantação.

