---
title: "O que é o relatório híbrido | Microsoft Docs"
description: "Os relatórios da Atividade de Auditoria no Azure Active Directory permitem que você exiba os eventos de auditoria locais e na nuvem."
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 678626e7c32659570de88d8178c16821cceaf7ee
ms.contentlocale: pt-br
ms.lasthandoff: 07/10/2017


---

<a id="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh" class="xliff"></a>
# Relatórios de auditoria de gerenciamento de identidade híbrida no Azure Active Directory – Visualização Pública (atualização)
Com os relatórios de Atividade de Auditoria do AD (Azure Active Directory), você pode exibir um único relatório para monitorar a atividade de gerenciamento de identidades que acontece localmente ou na nuvem. Esse recurso permite gerenciar todos os dados de identidade e acesso em um único local, poupando tempo e reduzindo os custos gerais.

<a id="what-is-azure-active-directory-hybrid-reporting" class="xliff"></a>
## O que é o relatório híbrido do Azure Active Directory?
O relatório de auditoria híbrido ajuda os profissionais de TI a resolver desafios comuns trazidos pelos relatórios de gerenciamento de identidades.

1. **Coletar atividades de gerenciamento de identidades entre sistemas diferentes.** Os relatórios híbridos mostram a atividade de gerenciamento de identidades do Azure AD e o Identity Manager.

2. **Exportar dados de relatório e criar relatórios personalizados.** Além de exibir os relatórios no portal do Azure, também é possível exportar os dados para gerar suas próprias exibições personalizadas.

3. **Reduzir o custo da infraestrutura do sistema de relatórios.** O relatório híbrido na nuvem significa que você pode eliminar a infraestrutura do data warehouse de relatórios locais.

<a id="how-does-it-work" class="xliff"></a>
## Como isso funciona?

Para coletar dados locais, primeiramente é preciso instalar um agente de relatório no servidor do Identity Manager 2016. O agente de relatório é baixado da página de Download da Microsoft [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=55112).

O processo do relatório híbrido segue estas etapas:
1. Após a instalação do agente de relatório, os dados da atividade do Identity Manager são enviados ao Log de Eventos do Windows.
2. O agente de relatório processa os eventos delta a cada dez minutos ou reinicia o serviço no Log de Eventos do Windows e os carrega no Portal do Azure.
3. O Portal do Azure processa os dados no prazo de uma hora após o recebimento
4. Os dados da atividade são armazenados por um mês.
5. O Portal do Azure recupera os dados do relatório de auditoria e os renderiza como auditoria na Folha de relatório de auditoria do Microsoft Azure.

<a id="see-also" class="xliff"></a>
## Consulte também
- Obtenha mais detalhes sobre [Working with Identity Manager Hybrid Reporting (Como trabalhar com o relatório híbrido do Identity Manager)](working-with-identity-manager-hybrid-reporting.md)
- Obtenha mais detalhes sobre os [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)
