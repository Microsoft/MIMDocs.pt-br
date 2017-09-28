---
title: Conectores com suporte | Microsoft Docs
description: "Use conectores para gerenciar a transferência de dados entre o MIM e suas fontes de dados conectadas."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/26/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 99e98f3f9cb5e68fde0e3018856bf613c082325d
ms.sourcegitcommit: ba4cd133f7b49752c5470c9fc46e7e302cc99b49
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="connect-to-your-directories"></a>Conectar aos seus diretórios

Os conectores vinculam fontes de dados conectadas específicas ao MIM (Microsoft Identity Manager) SP1. Um conector move dados de uma fonte de dados conectada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a fonte de dados conectada para mantê-los sincronizados com o MIM. Geralmente, há pelo menos um conector para cada diretório conectado.

No Forefront Identity Manager, conectores eram conhecidos como agentes de gerenciamento. Esse termo ainda é usado em alguns artigos ou em algumas partes do produto, mas saiba que os dois termos referem-se o mesmo conceito.

Este artigo aborda os conectores que estão incluídos e têm suporte no MIM, mas o conector para Conectividade Extensível 2.0 possibilita conectar-se com ainda mais fontes de dados. Alguns parceiros criaram seus próprios conectores dessa forma e uma lista completa está disponível na wiki do [FIM 2010: agentes de gerenciamento de parceiros](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores com suporte no MIM 2016 SP1

| Nome | Versões com suporte da fonte de dados conectada |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory 2012, 2016 |
| ADLDS (Active Directory Lightweight Directory Services) | ADLDS (Active Directory Lightweight Directory Services) |
| GAL (Lista de Endereços Global) do Active Directory | GAL (Lista de endereços Global) do Active Directory – Exchange 2013, 2016 |
| Extensible Connectivity 2.0 | Qualquer fonte de dados baseada em chamada ou arquivo |
| Serviço FIM | O Agente de Gerenciamento de Serviço do FIM (Serviço de Sincronização) deve ser a mesma versão do "Forefront Identity Manager Service" instalado |
| Banco de dados Universal do IBM DB2 | IBM DB2 versão 9.5 ou 9.7; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 |
| Servidor de diretório IBM | IBM Tivoli Directory Server 6. x |
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 |
| Banco de dados Oracle | Banco de dados do Oracle 10g ou 11g; cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Servidores de diretório Oracle (anteriormente Sun e Netscape) | Sun Directory Server 6. x, 7. x e Oracle 11 |
| [Conector do Windows PowerShell para o FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Microsoft Azure Active Directory Connector para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Active Directory do Microsoft Azure |
| [Conector LDAP genérico para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Servidor do LDAP v3 (compatível com RFC 4510) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [Conector de Serviços do SharePoint UPA](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | O SharePoint server 2013 ou 2016 com o UPA (Aplicativo de Serviço de Perfil do Usuário) |
| [Conector de Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| [Arquivo de texto do Par Atributo-Valor](https://technet.microsoft.com/en-us/library/cc708644(v=ws.10).aspx) | Arquivos de texto do par atributo-valor |
| [Arquivo de texto delimitado](https://technet.microsoft.com/en-us/library/cc720612(v=ws.10).aspx) | Arquivos de texto delimitado |
| [DSML (Linguagem de Marcação de Serviços de Diretório)](https://technet.microsoft.com/en-us/library/cc720660(v=ws.10).aspx) | DSML (Linguagem de Marcação de Serviços de Diretório) 2.0 |
| [Arquivo de texto de largura fixa](https://technet.microsoft.com/en-us/library/cc720633(v=ws.10).aspx) | Arquivos de texto de largura fixa |
| [LDIF (Formato de Troca de Dados LDAP)](https://technet.microsoft.com/en-us/library/cc708662(v=ws.10).aspx) | LDIF (Formato de Troca de Dados LDAP) |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gerenciamento no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
