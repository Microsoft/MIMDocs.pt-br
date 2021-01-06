---
title: Histórico de versão do BHOLD do Identity Manager | Microsoft Docs
description: Este artigo documenta as várias alterações feitas como parte das atualizações para BHOLD no MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/1/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4e3c247370c6c78e7814d894e9f0a6166d2e397a
ms.sourcegitcommit: 8f81767ec92e1b80658aaebb9463aa4d62396d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927601"
---
# <a name="bhold-modules-version-release-history"></a>Histórico de lançamento de versão dos módulos BHOLD

A equipe de Microsoft Identity Manager lança regularmente as atualizações listadas no histórico de [versão](version-history.md) . Este artigo ajuda você a acompanhar as versões que foram lançadas para BHOLD, um subcomponente da Microsoft Identity Manager. Em seguida, você pode decidir se precisa atualizar para a versão mais recente ou não.

## <a name="version-60620"></a>6.0.62.0 da versão

- Status: outubro de 2018
- [Download](https://www.microsoft.com/download/details.aspx?id=55950)

## <a name="version-60360"></a>6.0.36.0 da versão

- Status: 7 de setembro de 2017

### <a name="enhancements"></a>Aprimoramentos 
O BHOLD versão 6.0.36.0 inclui os seguintes aprimoramentos:

- Atualização da plataforma x86 para x64.

### <a name="fixed-issues"></a>Problemas corrigidos
Os problemas a seguir foram corrigidos no BHOLD versão 6.0.36.0.

#### <a name="bhold-core"></a>BHOLD Core

- Instalador atualizado para x86 para x64.
- A interface da Web não mostra todas as permissões.
- Correções para a biblioteca B1Common.
- O usuário não pode atribuir a função a OrgUnit quando a função tem poucas permissões com cardinalidade.
- A cardinalidade de permissão não funciona para o máximo. números de usuários.

#### <a name="access-management-connector"></a>Conector de Gerenciamento de Acesso

- n/a

#### <a name="bhold-integration-module"></a>Módulo de integração do BHOLD

- Instalador atualizado para x86 para x64.
- Local incorreto da pasta de LAYOUT.

#### <a name="bhold-model-generator"></a>Gerador de modelo de BHOLD

- As funções de modelo dos departamentos pai não são herdadas.

#### <a name="bhold-analytics"></a>BHOLD Analytics

- n/a

#### <a name="bhold-attestation"></a>Atestado de BHOLD

- n/a

#### <a name="bhold-reporting"></a>Relatórios do BHOLD

- O atributo personalizado ' employeeID ' não está disponível.
- Não é possível carregar corretamente dados grandes usando um conjunto de 3 arquivos.

## <a name="next-steps"></a>Próximas etapas

- [Guia de conceitos do BHOLD](../bhold/bhold-concepts-guide.md)
- [Guia de instalação do BHOLD](../bhold/bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](mim2016-bhold-developer-reference.md)
- [Histórico de versão do MIM](version-history.md)

