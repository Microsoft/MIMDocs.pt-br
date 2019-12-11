---
title: Instalação do gerador de modelos do BHOLD | Microsoft Docs
description: O modelo do BHOLD permite que você estruture dados de várias fontes
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d2510d6ea604dd88e56436812ed8bc975bc5c2b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516742"
---
# <a name="bhold-model-generator-installation"></a>Instalação do Gerador de Modelos do BHOLD

Usando o módulo Gerador de Modelos do BHOLD, você pode estruturar de dados de fontes autoritativas que contêm informações de usuário e organizacionais junto com ACLs (listas de controle de acesso) em um modelo que pode ser usado ao administrar o BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Requisitos para instalação do Gerador de Modelos do BHOLD 

Antes de instalar o módulo Gerador de Modelos do BHOLD, você deve instalar o seguinte:

1. Módulo BHOLD Core no servidor em que você planeja instalar o módulo Gerador de Modelos do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. O Provedor Microsoft OLE DB para Microsoft Jet deve estar instalado. Para obter mais informações, consulte [este artigo](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Não instale o Gerador de Modelos do BHOLD em sua rede de produção. O Gerador de Modelos do BHOLD destina-se a ser usado offline em um ambiente de preparo para criar um modelo de função normalizado que você pode importar para seu modelo de função empresarial. Executar o Gerador de Modelos do BHOLD em sua rede de produção pode resultar na perda de seu modelo de função existente.

## <a name="before-you-begin"></a>Antes de começar

Antes de instalar o módulo Gerador de Modelos do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de Configuração do Gerador de Modelos do BHOLD requer para concluir a instalação. A planilha a seguir ajudará você a registrar essas informações, de modo que você estará pronto para fornecê-las quando necessário. Você também precisará verificar se

Pacotes redistribuíveis do Mecanismo de Banco de Dados do Microsoft Access 2010

 

*De \<* <http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems> *\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Configurações de conta**

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar um provedor de segurança em um Domínio/Computador** | Quando selecionado, especifica que a segurança do Active Directory Domain Services controlará o acesso ao BHOLD Core.                                                                                                                | Marque a caixa de seleção. **Importante:** a instalação falhará se essa caixa de seleção não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio é fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Usuário**                                    | Especifica o nome de logon da conta de usuário do serviço BHOLD Core.                                                                                                                                                          | Grave o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Senha**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Grave a senha aqui: **Importante:** mantenha essa senha em um local seguro e oculto.                                                                                                                                                                                                                  |

**Configurações do banco de dados de backup**

| Item                                        | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                  | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar segurança integrada**                 | Especifica que a Autenticação do Windows é usada para acessar o banco de dados.                                                                                                                                                                                                                                                                                                                                                        | Marque a caixa de seleção se a autenticação do Windows for usada para se conectar ao SQL Server. Desmarque a caixa de seleção se a autenticação do SQL Server for usada. O banco de dados deverá ter sido criado antes de executar a Instalação do BHOLD Core se a Autenticação do SQL Server for usada. **Observação:** se a Autenticação do Windows for usada, você deverá fazer logon com uma conta que tenha a função de servidor sysadmin no servidor de banco de dados. **Importante:** use a autenticação do SQL Server somente em ambientes de teste. A Microsoft recomenda expressamente o uso da Autenticação do Windows em implantações de produção. |
| **Usuário de Banco de Dados** e **Senha de Banco de Dados** | Especifica o nome de usuário e a senha de um usuário com a função de servidor sysadmin no servidor de banco de dados. Esses valores são fornecidos somente quando a Autenticação do SQL Server é usada.                                                                                                                                                                                                                                                  | Escreva o nome de usuário do SQL Server aqui:  Escreva a senha do usuário do SQL Server aqui: </br></br> **Importante:** mantenha essa senha em um local seguro e oculto.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Servidor de Banco de Dados** e **Nome do Banco de Dados**   | Especifica o nome NetBIOS do servidor de banco de dados e o nome do banco de dados de backup que será criado pela Instalação do Gerador de Modelos do BHOLD. Se você não estiver usando a instância padrão do servidor de banco de dados, especifique a instância do servidor de banco de dados no formato *\<servidor\>* \\ *\<instância\>* .  A Microsoft recomenda que você nomeie o banco de dados de backup usando o nome do banco de dados do BHOLD Core seguido de \_BACKUP, por exemplo, B1_BACKUP. | Grave o nome de servidor (ou servidor e instância) aqui: </br> Grave o nome do banco de dados aqui:

## <a name="bhold-model-generator-setup"></a>Instalação do Gerador de Modelos do BHOLD

Para instalar o módulo Gerador de Modelos do BHOLD, faça logon como um membro do grupo Administradores do Domínio, baixe o arquivo a seguir e execute-o como administrador no servidor em que você pretende instalar o módulo BHOLD Core:

- BholdModelGenerator *\<Versão\>* \_Release.msi

Substitua *\<Versão\>* pelo número de versão do Gerador de Modelos do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximas etapas

- Para obter informações sobre como criar arquivos de entrada, consulte a [Referência técnica do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência do BHOLD para desenvolvedores](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versão do BHOLD](../reference/version-bhold-history.md)
