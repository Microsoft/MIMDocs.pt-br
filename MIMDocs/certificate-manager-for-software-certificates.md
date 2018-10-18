---
title: Solicitar certificados no Gerenciador de Certificados usando modelos | Microsoft Docs
description: Saiba como usar o Gerenciador de certificados para criar e renovar certificados de software com os modelos de perfil.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e9454643f425a6bd306c2828479129312d5a5d4f
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358356"
---
# <a name="create-software-certificates-with-certificate-manager"></a>Criar certificados de software com o Gerenciador de certificados
Para registrar e renovar certificados de software, você não precisa ser um administrador e não precisa de cartão inteligente virtual. Vale a pena observar que, em algum momento, você será solicitado a permitir uma operação de certificado, e isso é normal.

## <a name="create-a-software-certificate-profile-template-in-mim-2016-certificate-manager"></a>Criação de modelo de perfil de certificado de software no Gerenciador de certificados do MIM 2016

1.  Crie um modelo para o certificado que você vai solicitar para o cartão inteligente virtual. Abra o mmc.

2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-in**.

3.  Na lista de snap-ins disponíveis, clique duas vezes em **Modelos de Certificado** e, em seguida, clique em **Adicionar**.

4.  **Modelos de Certificado** agora estão localizados na Raiz do Console no MMC. Clique duas vezes para exibir todos os modelos de certificado disponíveis.

5.  Clique com o botão direito do mouse no **Modelo de usuário** e clique em **Duplicar Modelo**.

6.  Na guia **Compatibilidade**, em Autoridade de Certificação, selecione Windows Server 2008 e, em Destinatário do Certificado, selecione Windows 8.1/Windows Server 2012 R2.

    1.  Na guia **Geral** , no campo de nome de exibição, digite **Modelo de Certificado Arquivado**.

    2.  b.  Na guia **Tratamento de Solicitação**

        1.  Defina a **Finalidade** da assinatura e criptografia.

        2.  Marque **Incluir algoritmos simétricos permitidos pela entidade**.

        3.  Se desejar arquivar a chave, marque **Arquivar chave privada de criptografia da entidade**.

        4.  Em seguida, faça o seguinte... selecione **Avisar o usuário durante o registro**.

    3.  Na guia **Criptografia**

        1.  Em Categoria de Provedor, selecione **Provedor de Armazenamento de Chave**

        2.  Selecione **Solicitações podem usar qualquer provedor disponível no computador da entidade**.

    4.  Na guia **Segurança** , adicione o grupo de segurança ao qual deseja conceder acesso de **Registro** . Por exemplo, se você quiser conceder acesso a todos os usuários, selecione o grupo de usuários **Autenticado** e, em seguida, selecione permissões de **Registro** para eles.

    5.  Na guia **Nome da Entidade** .

        1.  Desmarque **Incluir nome de email no nome da entidade**.

        2.  Em **Incluir esta informação no nome de entidade alternativo**, desmarque **Nome de email**.

    6.  Clique em **OK** para finalizar suas alterações e criar o novo modelo. O novo modelo agora deve aparecer na lista Modelos de Certificado.

    7.  Selecione **Arquivo** e, em seguida, clique em **Adicionar/Remover Snap-in** para adicionar o snap-in da Autoridade de Certificação ao console do MMC. Quando for perguntado qual computador você deseja gerenciar, selecione **Computador Local**.

    8.  No painel esquerdo do MMC, expanda **Autoridade de Certificação (Local)** e, em seguida, expanda a CA dentro da lista Autoridade de Certificação.

    9. Clique com o botão direito do mouse em **Modelos de Certificados**, clique em **Novo** e, em seguida, clique em **Modelo de Certificado** a Ser Emitido.

    10. Na lista, selecione o novo modelo que você acabou de criar (**Modelo de Certificado Arquivado**) e, em seguida, clique em **OK**.

## <a name="create-the-profile-template"></a>Criar o modelo de perfil

1.  Faça logon no portal do CM como um usuário com privilégios administrativos.

2.  Vá para **Administração &gt; Gerenciar Modelos de Perfil** e verifique se a caixa está marcada ao lado do **Modelo de Perfil de Logon do Cartão Inteligente de Exemplo do MIM CM** e clique em **Copiar um modelo de perfil selecionado**.

3.  Digite o nome do modelo de perfil e clique em **OK**.

4.  Na próxima tela, clique em **Adicionar novo modelo de certificado** e certifique-se de marcar a caixa ao lado do nome da autoridade de certificação.

5.  Marque a caixa ao lado do nome do Certificado de Software Arquivado e clique em **Adicionar**.

6.  Remova o modelo de usuário marcando a caixa ao lado dele e, em seguida, clicando em **Excluir modelos de certificado selecionados** e **OK**.

7.  Clique em **Alterar as configurações gerais**.

8.  Marque as caixas à esquerda de **Gerar chaves de criptografia no servidor** e clique em **OK**. No painel esquerdo, clique em **Recuperar Política**.

9. Clique em **Alterar as configurações gerais**.

10. Se quiser emitir novamente os certificados arquivados, marque as caixas à esquerda de **Reemitir certificados arquivados** e clique em **OK**.

11. Se você estiver usando o CM Do Cartão Inteligente Virtual, precisará desabilitar itens de coleta de dados porque eles não funcionam com a coleta de dados ativada. Desabilite a coleta de dados para cada política clicando na política no painel esquerdo e, em seguida, desmarcando a caixa ao lado de **Item de dados de exemplo** e, em seguida, em **Excluir itens de coleta de dados**. Em seguida, clique em **OK**.
