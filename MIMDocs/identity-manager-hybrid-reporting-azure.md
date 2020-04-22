---
title: O que é um relatório híbrido no Azure AD? | Microsoft Docs
description: Os relatórios de atividade de auditoria híbrida no Azure Active Directory permitem visualizar os eventos auditados na nuvem e no local.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 6f4f2aea998fc5682d1fb21d77e4d4f1c582d770
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042144"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Relatórios de auditoria de gerenciamento de identidades híbridas no Azure Active Directory
Com os relatórios de Atividade de Auditoria do Azure Active Directory (Azure AD), você pode monitorar a atividade de gerenciamento de identidades localmente ou na nuvem. Ao gerenciar todas as suas identidades e acessar os dados em um único relatório, é possível economizar tempo e reduzir os custos gerais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é o relatório híbrido do Azure Active Directory?
O relatório de auditoria híbrido ajuda os profissionais de TI a resolver desafios comuns trazidos pelos relatórios de gerenciamento de identidades, como:

* **Coletar atividades de gerenciamento de identidades entre sistemas diferentes**. Os relatórios híbridos mostram a atividade de gerenciamento de identidades do Azure AD e o Identity Manager.

* **Exportar dados de relatório e criar relatórios personalizados**. Além de exibir os relatórios no portal do Azure, é possível exportar os dados para gerar suas próprias exibições personalizadas.

* **Reduzir o custo da infraestrutura do sistema de relatórios**. O relatório híbrido na nuvem significa que você pode ajudar a eliminar os custos associados à sua infraestrutura de data warehouse local.

## <a name="how-does-it-work"></a>Como isso funciona?

Para coletar dados locais, primeiramente é preciso instalar um agente de relatório no servidor do Identity Manager 2016. [Baixar o agente de relatório híbrido do Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

O relatório híbrido passa pelo seguinte processo:
1. Após a instalação do agente de relatório, os dados da atividade do Identity Manager são enviados ao Log de Eventos do Windows.
2. O agente de relatório processa os eventos delta a cada dez minutos ou quando o serviço de Log de Eventos do Windows reinicia. Em seguida, o agente carrega os eventos para o portal do Azure.
3. O portal do Azure processa os dados recebidos em até uma hora após o recebimento.
4. Os dados da atividade são armazenados por um mês.
5. O Portal do Azure recupera os dados do relatório de auditoria e os exibe na janela de Relatório de Auditoria do Microsoft Azure.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre:
- [Trabalhar com o Relatório híbrido do Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Políticas de retenção de emissão de relatórios](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integração de logs do Microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [API de relatórios do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
