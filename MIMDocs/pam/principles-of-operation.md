---
title: Entender os componentes do PAM | Microsoft Docs
description: O Privileged Access Management compartilha alguns componentes com o MIM e tem alguns próprios. Saiba como eles funcionam juntos.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e3e248092f674a031e255eb368681ab1b6dc21df
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010686"
---
# <a name="understand-the-components-of-mim-pam"></a>Entender os componentes do PAM do MIM

Privileged Access Management mantém o acesso administrativo separado das contas de usuário do dia a dia usando uma floresta separada.

> [!NOTE]
> A abordagem do PAM fornecida pelo MIM destina-se a ser usada em uma arquitetura personalizada para ambientes isolados em que o acesso à Internet não está disponível, em que essa configuração é exigida pela regulamentação ou em ambientes isolados de alto impacto, como laboratórios de pesquisa offline e tecnologia desconectada ou controle de supervisão e ambientes de aquisição de dados. Se seu Active Directory fizer parte de um ambiente conectado à Internet, consulte [protegendo o acesso privilegiado](/security/compass/overview) para obter mais informações sobre onde começar.

 Essa solução se baseia em florestas paralelas:

- *CORP*: sua floresta corporativa de uso geral que inclui um ou mais domínios. Embora você possa ter várias florestas CORP, para simplificar, os exemplos apresentados neste artigo presumem uma única floresta com um único domínio.  
- *PRIV*: uma floresta dedicada criada especialmente para esse cenário de PAM. Esta floresta inclui um domínio para acomodar grupos privilegiados e contas sombreadas por meio de um ou mais domínios CORP.

A solução MIM conforme configurada para o PAM inclui os seguintes componentes:  

- **Serviço MIM**: implementa a lógica de negócios para realizar operações de gerenciamento de identidade e acesso, incluindo gerenciamento de conta privilegiada e tratamento de solicitações de elevação.
- **Portal do mim**: um portal baseado no SharePoint, hospedado pelo SharePoint 2013 ou posterior, que fornece uma interface do usuário de gerenciamento e configuração do administrador.
- **Banco** de dados de serviço do mim: armazenado no SQL Server 2012 ou posterior, e mantém metadados de identidade e meta-dados necessários para o serviço do mim.
- **Serviço de Monitoramento do PAM** e **Serviço de Componente do PAM**: dois serviços que gerenciam o ciclo de vida de contas privilegiadas e que auxiliam o AD PRIV no ciclo de vida da associação a um grupo.
- **Cmdlets do PowerShell**: para popular, no Serviço MIM e no AD PRIV, usuários e grupos correspondentes aos usuários e grupos na floresta CORP para administradores do PAM, bem como para usuários finais que solicitam o uso JIT (Just-In-Time) de privilégios em uma conta administrativa.
- **API REST do PAM e portal de exemplo**: para desenvolvedores que integram o MIM no cenário de PAM com clientes personalizados para elevação, sem a necessidade de usar o PowerShell ou SOAP. O uso da API REST é demonstrado com um aplicativo Web de exemplo.

Uma vez instalado e configurado, cada grupo criado pelo procedimento de migração na floresta PRIV é um grupo de entidades estrangeiras que espelha o grupo na floresta CORP original. O grupo de entidades estrangeiras fornece os usuários que são membros desse grupo com o mesmo SID em seu token Kerberos como o SID do grupo na floresta CORP. Além disso, quando o serviço do MIM adiciona membros a esses grupos na floresta de privilégios, essas associações serão por tempo limitado.

Como resultado, quando um usuário solicita elevação usando os cmdlets do PowerShell e a solicitação é aprovada, o serviço do MIM adiciona sua conta na floresta PRIV a um grupo na floresta PRIV. Quando o usuário se conectar com suas contas privilegiadas, seu token Kerberos conterá um identificador SID (segurança) idêntico ao SID do grupo na floresta CORP. Como a floresta CORP é configurada para confiar na floresta PRIV, a conta elevada que está sendo usada para acessar um recurso na floresta CORP é exibida para um recurso de verificação de associações de grupo do Kerberos ser um membro dos grupos de segurança daquele recurso. Isso é fornecido através da autenticação entre florestas do Kerberos.

Além disso, essas associações são por tempo limitado, assim, depois de um intervalo pré-configurado, a conta do usuário administrativo não será mais parte do grupo na floresta PRIV. Como resultado, essa conta não poderá ser usada para acessar recursos adicionais.
