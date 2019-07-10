---
title: Conectores com suporte | Microsoft Docs
description: Use conectores para gerenciar a transferência de dados entre o MIM e suas fontes de dados conectadas.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1f7d4150ce7012cd4726126ba50b1ab0f94f474c
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690682"
---
# <a name="connect-to-your-directories"></a>Conectar aos seus diretórios

Os conectores vinculam fontes de dados conectadas específicas ao MIM (Microsoft Identity Manager) SP1. Um conector move dados de uma fonte de dados conectada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a fonte de dados conectada para mantê-los sincronizados com o MIM. Geralmente, há pelo menos um conector para cada diretório conectado.

No Forefront Identity Manager, conectores eram conhecidos como agentes de gerenciamento. Esse termo ainda é usado em alguns artigos ou em algumas partes do produto, mas saiba que os dois termos referem-se o mesmo conceito.

Este artigo aborda os conectores que estão incluídos e têm suporte no MIM, mas o conector para Conectividade Extensível 2.0 possibilita conectar-se com ainda mais fontes de dados. Alguns parceiros criaram seus próprios conectores dessa forma e uma lista completa está disponível na wiki do [FIM 2010: agentes de gerenciamento de parceiros](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores com suporte no MIM 2016 SP1

| Nome | Versões com suporte da fonte de dados conectada e links técnicos |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory Domain Services no Windows Server 2012 e no Windows Server 2016 |
| ADLDS (Active Directory Lightweight Directory Services) | ADLDS (Active Directory Lightweight Directory Services) |
| GAL (Lista de Endereços Global) do Active Directory | GAL (Lista de Endereços Global) do Active Directory Domain Services – Exchange 2013, Exchange 2016 |
| Extensible Connectivity 2.0 | Qualquer fonte de dados baseada em chamada ou arquivo |
| Serviço FIM | Serviço MIM. Observe que o Serviço de Sincronização de MIM e o Serviço de MIM devem ser da mesma versão. |
| Banco de dados Universal do IBM DB2 | IBM DB2 versão 9.5 ou 9.7; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 |
| Servidor de diretório IBM | IBM Tivoli Directory Server 6. x |
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 |
| Banco de dados Oracle | Banco de dados do Oracle 10g ou 11g; cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Servidores de diretório Oracle (anteriormente Sun e Netscape) | Sun Directory Server 6. x, 7. x e Oracle 11 |
| [Conector do Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector do Microsoft Azure Active Directory](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (não recomendado para novas implantações) |
| [Conector LDAP genérico](https://msdn.microsoft.com/library/dn510997.aspx) | [Servidor do LDAP v3 (em conformidade com RFC 4510)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Conector do SQL Genérico](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [O conector é compatível com todos os drivers ODBC de 64 bits](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [Conector de Serviços do SharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | O SharePoint server 2013 ou 2016 com o UPA (Aplicativo de Serviço de Perfil do Usuário) |
| [Conector de Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 e outras APIs REST e SOAP](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Arquivo de texto do Par Atributo-Valor](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Arquivos de texto do par atributo-valor |
| [Arquivo de texto delimitado](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Arquivos de texto delimitado |
| [DSML (Linguagem de Marcação de Serviços de Diretório)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | DSML (Linguagem de Marcação de Serviços de Diretório) 2.0 |
| [Arquivo de texto de largura fixa](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Arquivos de texto de largura fixa |
| [LDIF (Formato de Troca de Dados LDAP)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDIF (Formato de Troca de Dados LDAP) |
| [Conector do Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gerenciamento no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
