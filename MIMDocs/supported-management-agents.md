---
title: Conectores com suporte | Microsoft Docs
description: Use conectores para gerenciar a transferência de dados entre o MIM e suas fontes de dados conectadas.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: daveba
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/2/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b691fb5dd324a4202ab3f0f02344c5b43102c63a
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492473"
---
# <a name="connect-to-your-directories"></a>Conectar aos seus diretórios

Conectores vinculam fontes de dados conectadas específicas ao MIM (Microsoft Identity Manager). Um conector move dados de uma fonte de dados conectada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a fonte de dados conectada para mantê-los sincronizados com o MIM. Geralmente, há pelo menos um conector para cada diretório conectado.

No Forefront Identity Manager, conectores eram conhecidos como agentes de gerenciamento. Esse termo ainda é usado em alguns artigos ou em algumas partes do produto, mas saiba que os dois termos referem-se o mesmo conceito.

Este artigo aborda os conectores que estão incluídos e têm suporte no MIM, mas o conector para Conectividade Extensível 2.0 possibilita conectar-se com ainda mais fontes de dados. Alguns parceiros criaram seus próprios conectores dessa forma e uma lista completa está disponível na wiki do [FIM 2010: agentes de gerenciamento de parceiros](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp2"></a>Conectores com suporte no MIM 2016 SP2

| Nome do conector | Versões com suporte da fonte de dados conectada e links técnicos |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory no Windows Server 2012 – 2019 |
| ADLDS (Active Directory Lightweight Directory Services) | ADLDS (Active Directory Lightweight Directory Services) |
| GAL (Lista de Endereços Global) do Active Directory | GAL (lista de endereços global) Active Directory no Exchange 2013-2019 |
| Extensible Connectivity 2.0 | Qualquer fonte de dados baseada em chamada ou arquivo |
| Serviço FIM | Serviço MIM. Observe que o Serviço de Sincronização de MIM e o Serviço de MIM devem ser da mesma versão. |
| Banco de dados Universal do IBM DB2 | IBM DB2 versão 9.5 ou 9.7; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 <br/> Usar o conector SQL genérico para versões posteriores|
| Servidor de diretório IBM | IBM Tivoli Directory Server 6. x <br/> Usar o conector LDAP genérico para versões posteriores|
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 <br/> Usar o conector LDAP genérico para versões posteriores|
| Banco de dados Oracle | Banco de dados do Oracle 10g ou 11g; cliente de 64 bits <br/> Usar o conector SQL genérico para versões posteriores|
| Microsoft SQL Server | SQL Server 2012-2017 <br/> Usar o conector SQL genérico para versões posteriores ou SQL Azure|
| Servidores de diretório Oracle (anteriormente Sun e Netscape) | Sun Directory Server 6. x, 7. x e Oracle 11<br/> Usar o conector LDAP genérico para versões posteriores |
| [Conector do Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector do Microsoft Azure Active Directory](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (não recomendado para novas implantações, use Azure AD Connect sincronização, Azure AD Connect o provisionamento de nuvem ou o conector do Graph em vez disso) |
| [Conector LDAP genérico](https://msdn.microsoft.com/library/dn510997.aspx) | [Servidor LDAP v3 (compatível com RFC 4510)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector), incluindo o servidor de diretório 389, servidor de diretório Apache, IBM Tivoli DS, Isode Directory, NetIQ eDirectory, Novell eDirectory, Open DJ, Open DS, Open LDAP, Oracle Directory Server Enterprise Edition, RadiantOne virtual Directory Server, Sun One Directory Server |
| [Conector SQL genérico](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Há suporte para o conector com todos os drivers ODBC de 64 bits](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) , incluindo Microsoft SQL Server & SQL Azure, IBM DB2 10. x, IBM DB2 9. x, oracle 10 & 11g, oracle 12c & 18C, MySQL 5. x|
| [Conector para Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes versão v 8.5. x, v 9.0. x |
| [Conector de Serviços do SharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013-2019 com o aplicativo de serviço de perfil de usuário (UPA) |
| [Conector de Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 e outras APIs REST e SOAP](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Arquivo de texto do par atributo-valor](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Arquivos de texto do par atributo-valor |
| [Arquivo de texto delimitado](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Arquivos de texto delimitado |
| [DSML (Linguagem de Marcação de Serviços de Diretório)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | DSML (Linguagem de Marcação de Serviços de Diretório) 2.0 |
| [Arquivo de texto de largura fixa](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Arquivos de texto de largura fixa |
| [LDIF (Formato de Troca de Dados LDAP)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDIF (Formato de Troca de Dados LDAP) |
| [Conector do Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gerenciamento no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
