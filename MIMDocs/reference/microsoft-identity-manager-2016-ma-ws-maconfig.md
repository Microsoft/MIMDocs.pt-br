---
title: Opções de configuração do conector de serviço Web | Microsoft Docs
description: Este artigo aborda as etapas necessárias para instalar a ferramenta de configuração do serviço Web.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 3/27/2020
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.reviewer: markwahl-msft
ms.assetid: ''
ms.openlocfilehash: 34c83427b6dfb3084976aebf29c019d8228f8247
ms.sourcegitcommit: d21963c1fba6dc908bec5eaadc54e3395a8ef8c3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2020
ms.locfileid: "92757499"
---
# <a name="web-service-connector-configuration-options"></a>Opções de configuração do Conector do Serviço Web
Este artigo descreve as etapas para configurar um novo conector de serviço Web ou para fazer alterações em um conector de serviço da Web existente por meio da interface do usuário do serviço de sincronização Microsoft Identity Manager (MIM).

>[!IMPORTANT]
>Baixe e instale o [conector de serviço Web](https://www.microsoft.com/download/details.aspx?id=51495) antes de tentar as etapas neste artigo.

## <a name="configure-the-web-service-connector-in-the-synchronization-service"></a>Configurar o conector de serviço Web no serviço de sincronização

Você pode criar um novo conector de serviço Web usando o designer do agente de gerenciamento. Depois de criar o conector, você pode definir vários perfis de execução para executar tarefas diferentes. Ao configurar um conector existente, você pode alterar uma tarefa clicando na página apropriada no designer do agente de gerenciamento. Siga as etapas abaixo para configurar um novo conector de serviço Web.

1. Abra o serviço de sincronização Microsoft Identity Manager 2016. No menu **ferramentas** , selecione **agentes de gerenciamento** .

2. No menu **ações** , selecione **criar** . O designer do agente de gerenciamento é aberto.

3. Em **designer do agente de gerenciamento** , em **agente de gerenciamento para** , selecione **serviço Web (Microsoft)** . Em seguida, selecione **Avançar** .

    ![Criar um agente de gerenciamento](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma.png)

4. Na tela **conectividade** , selecione o projeto do **conector de serviço Web** padrão. Forneça valores para o **host** e a **porta** . Em seguida, selecione **Avançar** .

    ![Criar conectividade para o agente de gerenciamento](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-connectivity.png)

5. Defina os **parâmetros globais** . Use a credencial de logon adquirida do administrador do serviço Web para se conectar ao host. Em seguida, selecione **Avançar** .

    ![Definir parâmetros globais para o agente de gerenciamento](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-global-parameters.png)

    - Se o local da fonte de dados observar o horário de verão e a fonte de dados estiver configurada para se ajustar automaticamente às configurações de salvamento de horário de verão, verifique **se a fonte de dados está configurada para ajustar automaticamente o relógio para a opção de horário de verão** .
    - Se você quiser disparar o fluxo de trabalho de conexão de teste deste conector, verifique a opção **testar conexão** .

6. Na próxima tela, selecione **padrão** para **Selecionar partições de diretório** . Em seguida, selecione **Avançar** .

    ![Criar partições para o agente de gerenciamento](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-partitions.png)

7. Na tela **Selecionar tipos de objeto** , selecione o tipo de objeto com o qual você deseja trabalhar. Por padrão, o conector de serviço Web dá suporte a dois tipos de objeto: **funcionário** e **usuário** . Em seguida, selecione **Avançar** .

    ![Selecione o tipo de objeto](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-object-types.png)

8. Na página **Selecionar atributos** , selecione todos os atributos obrigatórios para os objetos e atributos selecionados com os quais você precisa trabalhar. Em seguida, selecione **Avançar** .

    ![Selecionar os atributos para os objetos](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-attributes.png)

9. Na página **Configurar âncoras** , especifique os atributos de âncora. Em seguida, selecione **Avançar** .

    ![Configurar as âncoras](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-anchors.png)

10. Na página **Configurar filtro do conector** , especifique o **filtro do conector** . Em seguida, selecione **Avançar** .

    ![Especificar o filtro do conector](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-connector-filter.png)

11. Na página **configurar regras de junção e projeção** , especifique as regras de junção e de projeção. Você pode criar uma nova regra de junção e uma regra de projeção selecionando nova regra de **junção** e **nova regra de projeção** , respectivamente. Em seguida, selecione **Avançar** .

    ![Especificar as regras de junção e projeção](media/microsoft-identity-manager-2016-ma-ws-maconfig/join-projection.png)

12. Na próxima página, configure o fluxo de atributos. Você deve especificar o **tipo de mapeamento** e a **direção do fluxo** para os atributos para os tipos de objeto selecionados. Em seguida, selecione **Avançar** .

    ![Configurar o fluxo de atributos](media/microsoft-identity-manager-2016-ma-ws-maconfig/attribute-flow.png)

13. Especifique o tipo de desprovisionamento a ser aplicado aos objetos. Em seguida, selecione **Avançar** .

    ![Especifique o tipo de desprovisionamento](media/microsoft-identity-manager-2016-ma-ws-maconfig/deprovisioning.png)

14. No caso de um fluxo de importação, a página **Configurar extensões** é desabilitada. Você pode configurar extensões para exportar fluxos selecionando primeiro o tipo de mapeamento **avançado** na página **Configurar fluxo de atributos** .

    ![Configurar extensões](media/microsoft-identity-manager-2016-ma-ws-maconfig/extensions.png)

15. Clique em **Concluir** .

Seu conector agora está configurado:

![Configuração do conector concluída](media/microsoft-identity-manager-2016-ma-ws-maconfig/sync-manager.png)

Depois que um conector é configurado, você pode configurar os perfis de execução selecionando **Configurar perfis de execução** .

## <a name="additional-steps"></a>Etapas adicionais

Quando a autenticação baseada em certificado é usada, uma alteração adicional é necessária após a ferramenta de configuração do serviço Web gerar um arquivo WSConfig, antes que esse arquivo possa ser importado em um projeto do conector de serviço Web no serviço de sincronização do MIM.

Para habilitar a autenticação baseada em certificado:

- Configurar seu projeto para usar a autenticação básica na ferramenta de configuração do serviço Web
- Crie uma cópia do arquivo my_project. wsConfig e renomeie-a para my_project.zip
- Abra este arquivo morto e modifique generated.config arquivos para substituir a autenticação básica pela autenticação baseada em certificado (um exemplo fornecido abaixo)
- Substitua generated.config arquivo em my_project.zip e renomeie-o como my_project_updated. wsConfig
- Selecione my_project_updated. wsConfig ao criar um agente de gerenciamento no servidor de sincronização do MIM

Encontre generated.config arquivo de exemplo com a autenticação baseada em certificado abaixo:

```xml
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="SoapAuthenticationType" value="Certificate"/>
        </appSettings>
        <system.serviceModel>
            <bindings>
                <wsHttpBinding>
                    <binding name="binding">
                        <security mode="Transport">
                            <transport clientCredentialType="Certificate"/>
                        </security>
                    </binding>
                </wsHttpBinding>
            </bindings>
            <client>
                <endpoint address="https://myserver.local.net:8011/sap/bc/srt/scs/sap/zsapconnect?sap-client=800"
                    binding="wsHttpBinding" bindingConfiguration="binding"
                    contract="SAPCONNECTOR.ZSAPConnect" name="binding"/>
            </client>
            <behaviors>
                <endpointBehaviors>
                    <behavior name="endpointCredentialBehavior">
                        <clientCredentials>
                            <clientCertificate findValue="my.certificate.name.local.net"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName"/>
                        </clientCredentials>
                    </behavior>
                </endpointBehaviors>
            </behaviors>
        </system.serviceModel>
    </configuration>
```

## <a name="next-steps"></a>Próximas etapas

- [Instalar a ferramenta de configuração do serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implantação de SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração de MA do serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
