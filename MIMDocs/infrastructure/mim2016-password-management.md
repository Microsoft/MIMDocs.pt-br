---
title: Gerenciamento de Senha do Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/01/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 45b46ed10f7eda506fe1fc1af94c4be06a1a37b9
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516590"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Gerenciamento de Senha do Microsoft Identity Manager 2016

O gerenciamento de senhas para várias contas de usuário é uma das dificuldades de gerenciar um ambiente corporativo com diversas fontes de dados. O Microsoft Identity Manager 2016 (MIM) fornece duas soluções de gerenciamento de senha:

-   Sincronização de senha – Utiliza o serviço de notificação de alteração de senha (PCNS) para capturar alterações de senha do Active Directory e propagá-las para outras fontes de dados conectadas.

-   Gerenciamento de alterações de senha baseado no usuário – Utiliza a Instrumentação de Gerenciamento do Windows (WMI) por meio de suporte técnico virtual e aplicativos de autoatendimento de redefinição de senha.

Usando a sincronização de senha e o gerenciamento de alterações de senha baseado no usuário, é possível:

-   Reduzir o número de senhas diferentes que os usuários precisam lembrar.

-   Definir ou alterar simultaneamente as senhas em várias contas de usuário para a mesma senha.

-   Permitir que os usuários alterem suas próprias senhas no Active Directory e enviar por push a alteração de senha para outros sistemas.

-   Eliminar o risco de criar outro repositório de senha ou credencial.

-   Sincronizar senhas entre várias fontes de dados usando o Active Directory como fonte autoritativa.

-   Realizar operações de gerenciamento de senha em tempo real, independentes de operações do MIM.

## <a name="password-extensions"></a>Extensões de senha

Os agentes de gerenciamento de servidores de diretório oferecem suporte para a alteração de senha e operações de definição por padrão. Para agentes de gerenciamento baseados em arquivo, banco de dados e de conectividade extensível, que não oferecem suporte à alteração de senhas e operações de definição por padrão, é possível criar uma biblioteca de vínculo dinâmico (DLL) de extensões de senha do .NET.
A DDL de extensões de senha do .NET será chamada sempre que uma alteração ou definição de senha for invocada para qualquer um desses agentes de gerenciamento. As configurações de extensão de senha são configuradas para esses agentes de gerenciamento no Synchronization Service Manager. Para obter mais informações sobre como configurar extensões de senha, consulte a Referência do Desenvolvedor do FIM.

| Há suporte para o gerenciamento de senhas por padrão nos agentes de gerenciamento para: | Ao usar uma extensão de senha, o gerenciamento de senhas também terá suporte nos agentes de gerenciamento do: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Arquivos de texto do par atributo-valor                                                                    |
| ADLDS (Active Directory Lightweight Directory Services)                   | Arquivos de texto delimitado                                                                               |
| Servidor de diretório IBM                                                      | Linguagem DSML (Linguagem de Marcação de Serviços de Diretório)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Arquivos de texto de largura fixa                                                                             |
| Servidores de diretório Sun e Netscape                                        | Banco de dados Universal do IBM DB2                                                                         |
|                                                                           | LDIF (Formato de Troca de Dados LDAP)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Banco de dados Oracle                                                                                    |

## <a name="password-synchronization"></a>Sincronização de senha


A sincronização de senha funciona com o serviço de notificação de alteração de senha (PCNS) em um domínio do Active Directory e permite que as alterações de senha que se originam do Active Directory sejam propagadas automaticamente para outras fontes de dados conectadas. O MIM faz isso executando como um servidor de Chamada de Procedimento Remoto (RPC) que detecta uma notificação de alteração de senha de um controlador de domínio do Active Directory. Quando a solicitação de alteração de senha for recebida e autenticada, será processada pelo MIM e propagada para os agentes de gerenciamento apropriados.

> [!IMPORTANT]
> O MIM não dá suporte para a sincronização de senha bidirecional. A configuração de uma sincronização de senha bidirecional pode criar um loop, o que consumirá os recursos do servidor e terá um efeito potencialmente negativo no Active Directory e no MIM.

O PCNS é executado em cada controlador de domínio do Active Directory. Os sistemas que recebem as notificações de senha são conhecidos como destinos. O servidor MIM deve ser configurado como um destino PCNS no Active Directory antes de as notificações de senha serem enviadas. A configuração do PCNS deve definir um grupo de inclusão e, opcionalmente, um grupo de exclusão. Esses grupos são usados para restringir o fluxo de senhas confidenciais do domínio. Por exemplo, para enviar as senhas para todos os usuários, mas não enviar senhas administrativas, é possível optar por usar Usuários de Domínio como o grupo de inclusão e Admins. do Domínio como grupo de exclusão. Para obter mais informações sobre como configurar o serviço de notificação de alteração de senha, consulte [Usando a Sincronização de Senha](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx)

Os componentes envolvidos no processo de sincronização de senha são:

-   **Serviço de notificação de alteração de senha (Pcnssvc.exe)** – O serviço de notificação de alteração de senha é executado em um controlador de domínio e é responsável por receber as notificações de alteração de senha do filtro de senhas local, enfileirando-as para o servidor de destino que executa o MIM e usando o RPC para enviar as notificações. O serviço criptografa a senha e garante que a ela permaneça segura até que seja entregue com êxito ao servidor de destino que executa o MIM.

-   **Nome da entidade de serviço (SPN)** – O SPN é uma propriedade no objeto de conta no Active Directory usada pelo protocolo Kerberos para autenticar mutuamente o PCNS e o destino. O SPN garante que o PCNS autenticará o servidor que executa o MIM correto e que nenhum outro serviço receba as notificações de alteração de senha. O SPN é criado e atribuído usando a ferramenta setspn.exe. Para obter mais informações sobre como configurar o SPN, consulte Usando a Sincronização de Senha.

-   **Filtro de notificação de alteração de senha (Pcnsflt.dll)** – O filtro de senhas é usado para obter as senhas com texto sem formatação do Active Directory. Esse filtro é carregado pela Autoridade de Segurança Local (LSA) de cada controlador de domínio do Windows Server que participa da distribuição de senha para um servidor de destino que executa o MIM. Após a instalação do filtro e a reinicialização do domínio, o filtro começará a receber notificações de alterações de senha originadas nesse controlador de domínio. O filtro de notificação de senha é executado simultaneamente com outros filtros que estão em execução no controlador de domínio.

-   **Utilitário de configuração de serviço de notificação de alteração de senha (Pcnscfg.exe)** – O utilitário pcnscfg.exe é usado para gerenciar e manter os parâmetros de configuração do serviço de notificação de alteração de senha armazenados no Active Directory. Esses parâmetros de configuração, como definir os servidores de destino, o intervalo de repetição da fila de senhas e habilitar ou desabilitar um servidor de destino, são usados ao autenticar e enviar notificações de senha ao servidor de destino que executa o MIM.
    A configuração do serviço é armazenada no Active Directory, portanto, só é necessário atualizar a configuração em um controlador de domínio. O Active Directory replica a alteração para todos os outros controladores de domínio.

-   **Servidor de Chamada de Procedimento Remoto (RPC) no servidor que executa o MIM** – Quando a sincronização de senha estiver habilitada, o servidor RPC no servidor que executa o MIM será inicializado, o que o habilitará a receber notificações do serviço de notificação de alteração de senha. O RPC seleciona dinamicamente um intervalo de portas a serem usadas. Se o MIM for necessário para se comunicar com a floresta do Active Directory por meio de um firewall, abra um intervalo de portas.

-   **DLL de extensão de senha** – A DLL de extensão de senha fornece uma maneira de implementar operações de definição ou alteração de senhas por meio de uma extensão de regras para qualquer banco de dados, conectividade extensível ou agente de gerenciamento baseado em arquivo.
    Isso é feito criando um atributo criptografado somente para exportação denominado "export_password" que, na verdade, não existe no diretório conectado, mas pode ser acessado e definido em extensões de regras de provisionamento ou ser usado durante o fluxo de atributo de exportação. Para obter mais informações sobre como configurar extensões de senha, consulte a [Referência do Desenvolvedor do FIM](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Preparação para a sincronização de senha

Antes de configurar a sincronização de senha para o ambiente do MIM e do Active Directory, verifique o seguinte:

-   Se o MIM está instalado de acordo com as instruções de instalação.

-   Se os agentes de gerenciamento das fontes de dados conectados a serem gerenciadas para a sincronização de senha já foram criados e se os objetos estão sendo associados e sincronizados com êxito.

Configurar a sincronização de senha:

-   Estenda o esquema do Active Directory para adicionar as classes e os atributos necessários para instalar e executar o serviço de notificação de alteração de senha (PCNS).

-   Instale o PCNS em cada controlador de domínio.

-   Configure o nome da entidade de serviço (SPN) no Active Directory para a conta de serviço do MIM.

-   Configure o PCNS para se comunicar com o serviço MIM de destino.

-   Configure os agentes de gerenciamento para as fontes de dados conectadas a serem gerenciadas para sincronização de senha.

-   Habilite sincronização de senha no MIM.

Para obter mais informações sobre como configurar a sincronização de senha, consulte Usando a Sincronização de Senha.

## <a name="password-synchronization-process"></a>Processo de sincronização de senha

O processo de sincronização de uma solicitação de alteração de senha de um controlador de domínio do Active Directory para outras fontes de dados conectadas é mostrado no diagrama a seguir:

1.  O usuário inicia a solicitação de alteração de senha pressionando Ctrl + Alt + Del. A solicitação de alteração de senha, incluindo a nova senha, é enviada para o controlador de domínio mais próximo.

2.  O controlador de domínio registra a solicitação de alteração de senha e notifica o filtro de notificação de alteração de senha (Pcnsflt.dll).

3.  O filtro de notificação de alteração de senha encaminha a solicitação para o serviço de notificação de alteração de senha (PCNS).

4.  O PCNS verifica a solicitação de alteração de senha e, em seguida, autentica o nome da entidade de serviço (SPN) usando o Kerberos e encaminha a solicitação de alteração de senha no RPC criptografado para o servidor de destino MIM.

5.  O MIM valida o controlador de domínio de origem e, em seguida, usa o nome de domínio para localizar o agente de gerenciamento que usa esse domínio e utiliza as informações de conta do usuário da solicitação de alteração de senha para localizar o objeto correspondente no espaço conector.

6.  Usando as informações da tabela de associação, o MIM determina os agentes de gerenciamento que recebem a alteração de senha e envia a alteração de senha para eles.

## <a name="password-synchronization-security"></a>Segurança de sincronização de senha

As seguintes preocupações de segurança de sincronização de senha foram abordadas:

-   Autenticação da origem da senha – Quando a notificação de alteração de senha é recebida, a autenticação Kerberos é feita pelo MIM, bem como pelo controlador de domínio de origem para garantir que o destinatário e o remetente são válidos. Ao receber uma notificação de alteração de senha, o MIM garante que o chamador tem uma conta no contêiner de Controladores de Domínio do domínio ao qual pertence.

-   Falha na sincronização de senha para uma fonte de dados de destino em virtude de uma conexão não segura – Se o agente de gerenciamento tiver sido configurado para exigir uma conexão segura e esta não for detectada, a sincronização falhará.
    Sincronização ainda ocorrerá se o agente de gerenciamento tiver sido configurado para permitir conexões não seguras. Habilite conexões não segura apenas após examinar e entender os riscos envolvidos.

-   Armazenamento seguro de senhas – O MIM armazena senhas criptografadas apenas temporariamente. Todas as senhas recebidas pelo MIM durante uma operação de notificação de alteração de senha são criptografadas assim que entram no processo do MIM.
    No momento em que são enviadas com êxito à fonte de dados de destino conectada, elas são descriptografados e a memória que armazena a senha será desmarcada imediatamente. Caso não seja possível gravar na fonte de dados conectada, a senha criptografada será armazenada até que todas as tentativas de repetição sejam realizadas e, então, será apagada da memória.

-   Filas de senha seguras – As senhas armazenadas nas filas de senha do PCNS são criptografadas até que sejam entregues.

## <a name="password-synchronization-error-recovery-scenarios"></a>Cenários de recuperação de erro de sincronização de senha

Idealmente, sempre que um usuário alterar uma senha, a alteração será sincronizada sem erros. Os cenários a seguir descrevem como o MIM se recupera de erros comuns de sincronização:

-   **Falha na notificação de senha do Active Directory para o MIM** – Isso poderá ocorrer se a rede estiver inoperante ou se o servidor que executa o MIM não estiver disponível. A notificação de alteração de senha permanece na fila localmente no controlador de domínio pelo PCNS. O PCNS tenta novamente a notificação de acordo com a configuração de intervalo de repetição.

-   **Falha na sincronização de senha para uma fonte de dados de destino** – Isso também poderá ocorrer se a rede estiver inoperante ou se a fonte de dados de destino não estiver disponível.
    A notificação de alteração de senha será enfileirada e repetida de acordo com a configuração do agente de gerenciamento para tentativa e intervalo de repetição. Todas as senhas são criptografadas enquanto estiverem armazenadas para repetição e excluídas quando a operação for bem-sucedida ou os limites de repetição sejam atingidos.

-   **Ativar um servidor em espera passiva que executa o MIM após uma falha** – No caso de o servidor primário que executa o MIM falhar, é possível configurar um servidor em espera passiva para sincronização de senha e ativá-lo sem perda de alterações de senha. Para obter mais informações, confira [MIISactivate: ferramenta de ativação de servidor](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx)

Algumas falhas são tão graves que nenhuma quantidade de tentativas é suficiente para que a operação seja bem-sucedida. Nesses casos, um evento de erro é registrado e o processo é interrompido. Os eventos a seguir não serão repetidos:

| Evento | Severidade    | Descrição                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Informações do | Uma operação de definição de sincronização de senha não foi executada porque o carimbo de data/hora estava desatualizado.                                                                      |
| 6921  | Erro do       | A operação de definição de sincronização de senha não foi processada porque o gerenciamento de senhas não está habilitado no agente de gerenciamento de destino.                                |
| 6922  | Erro do       | A operação de definição de sincronização de senha não foi processada porque o gerenciamento de senhas não está configurado no agente de gerenciamento de destino.                             |
| 6923  | Aviso     | A operação de definição de sincronização de senha não foi processada porque o objeto do espaço conector de destino não pôde ser encontrado no diretório conectado.                  |
| 6927  | Erro do       | A operação de definição de sincronização de senha falhou porque a senha não está de acordo com a política de senha do sistema de destino.                                      |
| 6928  | Erro do       | A operação de definição de sincronização de senha falhou porque a extensão de senha do agente de gerenciamento de destino não está configurada para oferecer suporte a operações de definição de senha. |

## <a name="user-based-password-change-management"></a>Gerenciamento de alterações de senha baseado no usuário

O MIM fornece dois aplicativos Web que usam o Instrumentação de Gerenciamento do Windows (WMI) para redefinir senhas. Assim como na sincronização de senha, o gerenciamento de senhas será ativado quando o agente de gerenciamento for configurado no Designer de Agente de Gerenciamento. Para obter informações sobre gerenciamento de senhas e o WMI, consulte a Referência do Desenvolvedor do MIM.

O MIM cria dois grupos de segurança durante a instalação, que oferecem suporte específico a operações de gerenciamento de senha:

-   FIMSyncBrowse — Os membros desse grupo têm permissão para coletar informações sobre contas do usuário ao realizar operações de pesquisa com consultas WMI.

-   FIMSyncPasswordSet — Os membros desse grupo têm permissão para executar operações de pesquisa na conta, definição e alteração de senhas usando as interfaces de gerenciamento de senha com o WMI.
