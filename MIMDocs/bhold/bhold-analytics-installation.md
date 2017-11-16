---
title: "Instalação da Análise do BHOLD | Microsoft Docs"
description: "O módulo Análise do BHOLD fornece teste de acesso a dados baseado em regras"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 631e08667e5d1535d8f63cc297aad360080f8b20
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-analytics-installation"></a>Instalação da Análise do BHOLD

O módulo Análise do BHOLD fornece teste de acesso a dados baseado em regras para assegurar que sua organização possa controlar efetivamente o acesso a seus dados e esteja em conformidade com os requisitos de acesso interno e externo. A análise de impacto automatizada gerada pelo módulo Análise do BHOLD fornece uma visão geral que mostra o número de usuários que seriam afetados pela imposição de uma regra proposta, abrangendo tanto aqueles que estariam em conformidade com a regra quanto aqueles que a violariam. O módulo Análise do BHOLD também pode fornecer uma lista detalhada dos usuários que estariam em conformidade com a regra e daqueles que a violariam.

## <a name="bhold-analytics-installation-requirements"></a>Requisitos para instalação do módulo Análise do BHOLD

Antes de instalar o módulo Análise do BHOLD, você deve instalar o módulo BHOLD Core no servidor no qual você planeja instalar o módulo Análise do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Antes de começar

Antes de você começar a instalar o módulo Análise do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de Configuração da Análise do BHOLD requer para concluir a instalação. A planilha a seguir ajudará você a registrar essas informações, de modo que você estará pronto para fornecê-las quando necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar um provedor de segurança em um Domínio/Computador** | Quando selecionado, especifica que a segurança do Active Directory Domain Services controlará o acesso ao BHOLD Core.                                                                                                                | Marque a caixa de seleção. **Importante:** a instalação falhará se essa caixa de seleção não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio é fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Usuário**                                    | Especifica o nome de logon da conta de usuário do serviço BHOLD Core.                                                                                                                                                          | Grave o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Senha**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Grave a senha aqui: **Importante:** certifique-se de manter essa senha em um local seguro, oculto.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalação da Análise do BHOLD

Para instalar o módulo Análise do BHOLD, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo Análise do BHOLD:

- BholdAnalytics*\<Versão\>*\_Release.msi

Substitua *\<Versão\>* pelo número de versão da Análise do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

# <a name="next-steps"></a>Próximas etapas

- [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)