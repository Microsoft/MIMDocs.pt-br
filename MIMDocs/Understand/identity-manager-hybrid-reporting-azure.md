---
title: "O que é o relatório híbrido | Microsoft Docs"
description: "O relatório de híbrido do Azure Active Directory permite criar relatórios personalizados que incluem eventos de nuvem e local."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f21c15fdaa5fba9176cfc60a3c49017fa97fa935


---

# <a name="hybrid-identity-management-reports-in-azure"></a>Relatórios híbridos de gerenciamento de identidades no Azure
Com o Azure AD (Active Directory), você pode criar um único relatório para monitorar a atividade de gerenciamento de identidades que acontece no local ou na nuvem. Esse recurso permite gerenciar todos os dados de identidade e acesso em um único local, poupando tempo e reduzindo os custos gerais.

## <a name="what-is-azure-ad-hybrid-reporting"></a>O que é relatório híbrido do Azure AD?
O relatório híbrido ajuda os profissionais de TI a resolver desafios comuns trazidos pelos relatórios de gerenciamento de identidades.

1. **Coletar atividades de gerenciamento de identidades entre sistemas diferentes.** Os relatórios híbridos mostram a atividade de gerenciamento de identidades do Azure AD e o Identity Manager.

2. **Exportar dados de relatório e criar relatórios personalizados.** Além de exibir os relatórios no portal do Azure, também é possível exportar os dados para gerar suas próprias exibições personalizadas.

3. **Reduzir o custo da infraestrutura do sistema de relatórios.** O relatório híbrido na nuvem significa que você pode eliminar a infraestrutura de data warehouse de relatórios no local.

## <a name="how-does-it-work"></a>Como isso funciona?

Para coletar dados no local, primeiramente é preciso instalar um agente de relatório no servidor do Identity Manager. O agente de relatório é baixado na página Configurar do diretório no [portal clássico do Azure](https://manage.windowsazure.com/).

O processo do relatório híbrido segue estas etapas:
1. Após a instalação do agente de relatório, os dados da atividade do Identity Manager são enviados ao Log de Eventos do Windows.
2. O agente de relatório processa os eventos no Log de Eventos do Windows e os carrega no Azure.
3. Os dados da atividade são armazenados por um mês.
4. Ao solicitar um relatório, os eventos da atividade são analisados e filtrados para os relatórios necessários.
5. O portal clássico do Azure recupera os dados de relatório e os renderiza como o relatório da atividade.

## <a name="see-also"></a>Consulte também
- Obtenha mais detalhes sobre [Working with Identity Manager Hybrid Reporting (Como trabalhar com o relatório híbrido do Identity Manager)](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Nov16_HO2-->


