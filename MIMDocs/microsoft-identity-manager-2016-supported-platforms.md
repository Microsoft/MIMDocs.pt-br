---
title: Plataformas de software com suporte | Microsoft Docs
description: Localize os produtos e versões compatíveis com cada um dos componentes do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 4ef6c5aa5ba3814ab20dff445729da4acf96ecf7
ms.sourcegitcommit: d6178a67014d66d37056c13d10328ae03e3cd781
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970389"
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas com suporte para o MIM 2016

Esta tabela descreve as plataformas com suporte, e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só têm suporte no MIM 2016 service pack 1. As versões marcadas com ** só têm suporte no MIM 2016 service pack 2 ou em um hotfix posterior. As versões marcadas com "NR", que significa não recomendada, são compatíveis, embora não sejam recomendadas ao iniciar uma nova implantação daquela plataforma para MIM.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|--------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 ** |
| | Nível funcional do Active Directory para provisionamento de usuário, PCNS e sincronização de GAL | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Banco de dados da Sincronização do MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/>SQL Server 2016 SP2 * <br/>SQL Server 2017 ** <br/> SQL Server 2019 ** |
| | Active Directory para provisionamento de usuário, PCNS e sincronização de GAL (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Exchange para provisionamento de caixa de correio e Sincronização GAL (opcional)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Exchange Server 2019 ** |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Sistema conectado adicional (opcional) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> SharePoint Server 2019 ** <br/> Outros produtos de terceiros |
| **Serviço e Portal do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| |Cenário do PAM: Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * <br/> Windows Server 2019 **|
| |Cenário do PAM: Active Directory para a floresta do PAM no ambiente de bastiões | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * <br/> Windows Server 2019 ** |
| |Cenário do PAM: Active Directory para florestas (CORP) existentes em cenários do PAM | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * <br/> Windows Server 2019 ** |
| | Banco de dados do Serviço do MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** <br/> SQL Server 2019 ** |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 *<br/> SharePoint 2019 ** |
| | Servidor de email para aprovação do Serviço do MIM e emails de gerenciamento de grupo (opcional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Server 2019 ** <br/> Exchange Online * (somente notificação antes do build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Navegador | Todos os principais navegadores com suporte * (com limitação para dispositivos móveis)|
| **Relatórios do Serviço do MIM** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Data Warehouse | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (Com 4.4.1459)<br/> System Center 2019 Service Manager ** |
| **Portais de Registro e Redefinição de Senha do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Navegador da Web | Todos os principais navegadores com suporte |
| **Suplementos e extensões do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integração com o Outlook (opcional) | Outlook 2010 (no Windows, exceto Clique para Executar)<br/>Outlook 2013 (no Windows, exceto Clique para Executar) <br/> Outlook 2016 (no Windows 10, exceto Clique para Executar) *<br/>Outlook para Microsoft 365 (no Windows 10, incluindo o Clique para Executar) ** |
| | Cmdlets do solicitante do PowerShell no PAM (opcional) | Windows 8.1<br/>Windows 10 |
| **Gerenciamento de certificados do MIM** (Integração de servidor e a AC (Autoridade de Certificação)) | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Autoridade de certificação | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Banco de dados do MIM CM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** |
| **Gerenciamento de certificados do MIM** (Aplicativo) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (cliente em massa) | Windows | Windows 7 |
| **MIM Certificate Management** (cartão inteligente baseado no ActiveX do cliente) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **Suite MIM BHOLD** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Banco de dados BHOLD | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Servidor de email (opcional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Navegador da Web | Navegadores com suporte do Internet Explorer com o Silverlight |
