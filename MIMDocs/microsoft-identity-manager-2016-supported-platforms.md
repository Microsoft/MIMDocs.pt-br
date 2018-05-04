---
title: Plataformas de software com suporte | Microsoft Docs
description: Localize os produtos e versões compatíveis com cada um dos componentes do MIM 2016
keywords: ''
author: fimguy
ms.author: davidste
manager: davidste
ms.date: 04/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 3ff2ca7b9a268d4723861fcdec7f2f11932a6026
ms.sourcegitcommit: 3502d636687e442f7d436ee56218b9b95f5056cf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas com suporte para o MIM 2016

Esta tabela descreve as plataformas com suporte, e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só têm suporte no MIM 2016 service pack 1 com o hotfix mais recente.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|--------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Nível funcional do Active Directory para provisionamento de usuário, PCNS e sincronização de GAL | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Banco de dados da Sincronização do MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory para provisionamento de usuário, PCNS e sincronização de GAL (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange para provisionamento de caixa de correio e Sincronização GAL (opcional)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Sistema conectado adicional (opcional) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Outros produtos de terceiros |
| **Serviço e Portal do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário do PAM: Windows Server | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário do PAM: Active Directory para a floresta do PAM no ambiente de bastiões | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário do PAM: Active Directory para florestas (CORP) existentes em cenários do PAM | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Banco de dados do Serviço do MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Servidor de email para aprovação do Serviço do MIM e emails de gerenciamento de grupo (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (somente notificação antes da compilação [4.4.1749.0](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Navegador | Todos os principais navegadores com suporte * (com limitação para dispositivos móveis)|
| **Relatórios do Serviço do MIM** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Data Warehouse | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (Com 4.4.1459)<br/> [Compatibilidade de Versão do SQL Server para o System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **Portais de Registro e Redefinição de Senha do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Navegador da Web | Todos os principais navegadores com suporte |
| **Suplementos e extensões do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integração com o Outlook (opcional) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (no Windows 10) * |
| | Cmdlets do solicitante do PowerShell no PAM (opcional) | Windows 8.1<br/>Windows 10 |
| **Gerenciamento de certificados do MIM** (Integração de servidor e a AC (Autoridade de Certificação)) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Autoridade de certificação | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Banco de dados do MIM CM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Gerenciamento de certificados do MIM** (Aplicativo) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (cliente em massa) | Windows | Windows 7 |
| **MIM Certificate Management** (cartão inteligente baseado no ActiveX do cliente) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **Suite MIM BHOLD** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Banco de dados BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Servidor de email (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Navegador da Web | Navegadores com suporte do Internet Explorer com o Silverlight |
