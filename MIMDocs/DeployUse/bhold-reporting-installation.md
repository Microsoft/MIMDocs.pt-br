---
title: "Instalação de relatórios do BHOLD | Microsoft Docs"
description: "O módulo Relatório do BHOLD permite gerar relatórios sobre as funções e políticas de autorização"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-reporting-installation"></a>Instalação de relatório do BHOLD

O módulo Relatório do BHOLD fornece a capacidade de gerar relatórios sobre as funções e outras políticas de autorização no BHOLD. Esses relatórios costumam ser úteis para auditoria ou para demonstrar a conformidade com os requisitos regulamentares. Além disso, esse módulo estende sua capacidade de gerenciar autorizações em sua organização, fornecendo aos usuários as informações necessárias analisar a associação das respectivas funções. Os relatórios podem ter exibições restritas, que garantem que os usuários que criam relatórios veem apenas as informações que têm permissão para ver.

## <a name="bhold-reporting-installation-requirements"></a>Requisitos de instalação de Relatório do BHOLD

Antes de instalar o módulo Relatório do BHOLD, você deve instalar o módulo BHOLD Core no servidor no qual você planeja instalar o módulo Relatório do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Se você estiver instalando ambos o Relatório do BHOLD e o Atestado do BHOLD, você deverá instalar o Relatório do BHOLD antes de instalar o Atestado do BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de você começar a instalar o módulo Relatório do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de Configuração do Relatório do BHOLD requer para concluir a instalação. A planilha a seguir ajudará você a registrar essas informações, de modo que você estará pronto para fornecê-las quando necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar um provedor de segurança em um Domínio/Computador** | Quando selecionado, especifica que a segurança do Active Directory Domain Services controlará o acesso ao BHOLD Core.                                                                                                                | Marque a caixa de seleção. </br>**Importante:** a instalação falhará se essa caixa de seleção não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio é fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Usuário**                                    | Especifica o nome de logon da conta de usuário do serviço BHOLD Core.                                                                                                                                                          | Grave o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Senha**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Grave a senha aqui: </br>**Importante:** certifique-se de manter essa senha em um local seguro, oculto.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalação de Relatório do BHOLD

Para instalar o módulo Relatório do BHOLD, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo Relatório do BHOLD:

- BholdReporting*\<Versão\>*\_Release.msi

Substitua *\<Versão\>* pelo número de versão do Relatório do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximas etapas

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)