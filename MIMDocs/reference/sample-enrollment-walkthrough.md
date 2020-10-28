---
title: Explicação do registro de exemplo | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a560e68ea820211caa1dd0a40a0143f44d5f7fb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "92757212"
---
# <a name="sample-enrollment-walkthrough"></a>Explicação do registro de exemplo
Este artigo mostra as etapas necessárias para executar um registro de autoatendimento de cartão inteligente virtual. Mostra uma solicitação de aprovação automática com um PIN definido pelo usuário.

1. O cliente autentica o usuário e solicita uma lista de modelos de perfil para os quais o usuário autenticado pode se registrar:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. O cliente exibe a lista resultante ao usuário. O usuário seleciona um modelo de perfil de vSC (cartão inteligente virtual) com o nome "VPN de cartões inteligentes virtuais" e UUID `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1` .

3. O cliente recupera a política de registro do modelo de perfil selecionado usando o UUID retornado na etapa anterior:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. O cliente analisa o campo **DataCollection** na política retornada, observando que um único item de coleta de dados intitulado "razão da solicitação" é exibido. O cliente também observa que o sinalizador **collectComments** está definido como false, portanto, ele não solicita que o usuário insira nenhum.

5. Depois que o usuário tiver inserido o motivo para exigir os certificados, o cliente cria uma solicitação:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. O servidor cria a solicitação com êxito e retorna o URI da solicitação para o cliente:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5` .

7. O cliente recupera o objeto de solicitação chamando o URI retornado:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. O cliente verifica se a propriedade **State** na solicitação está definida como aprovada. A execução da solicitação pode começar.

9. O cliente examina a solicitação para ver se há um cartão inteligente já associado à solicitação analisando o conteúdo do parâmetro **newsmartcarduuid** .

10. Como ele contém apenas um GUID vazio, o cliente deve usar um cartão existente que ainda não esteja em uso pelo MIM CM ou criar um se o modelo de perfil estiver sendo configurado para cartões inteligentes virtuais.

11. Como este último foi indicado ao cliente por meio da consulta inicial para os modelos de perfil registráveis (etapa 1), agora ele deve criar um dispositivo de Cartão Inteligente Virtual.

12. O cliente recupera a política de cartão inteligente do modelo de perfil. Ele usa o UUID do modelo que foi selecionado na etapa 3:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13. A política de cartão inteligente contém a chave de administração padrão para o cartão na propriedade **DefaultAdminKeyHex** . Quando criar o cartão inteligente, o cliente deve definir a chave de administração inicial do cartão inteligente para essa chave.  
14. Após a criação do dispositivo de cartão inteligente, ele deve atribuí-lo à solicitação:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. O servidor responde ao cliente com um URI para o objeto de cartão inteligente recém-criado: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644` .

16. O cliente recupera o objeto de cartão inteligente:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17. Ao verificar o valor do sinalizador **diversifyadminkey** na política de cartão inteligente obtida na etapa 12, o cliente sabe que ele tem que diversificar a chave de administração.

18. O cliente recupera a chave de administração proposta:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19. O cliente deve autenticar ao cartão como administrador para definir a chave de administração. Para fazer isso, ele solicita um desafio de autenticação do cartão inteligente e o envia ao servidor:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    O servidor envia a resposta de desafio no corpo da resposta HTTP. Localize a cadeia de caracteres do desafio hexadecimal, converta em base64 e passe-a como um parâmetro na URL. A URL retorna outra resposta, que deve ser convertida novamente no formato hexadecimal.

20. O servidor envia a resposta do desafio no corpo da resposta HTTP e o cliente o utiliza para diversificar a chave de administração.

21. O cliente observa que o usuário deve fornecer o PIN desejado inspecionando o campo **UserPinOption** na política de cartão inteligente.

22. Depois que o usuário tiver inserido o PIN desejado em uma caixa de diálogo, o cliente executará uma autenticação de desafio/resposta, conforme descrito na etapa 19, com a única diferença de que o sinalizador diversificado agora deve ser definido como true:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23. O cliente usa a resposta recebida do servidor para definir o pin do usuário desejado.

24. O cliente agora está pronto para gerar solicitações de certificado. Ele consulta os parâmetros de geração de certificado de modelo do perfil para determinar como as solicitações/chaves devem ser geradas.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25. O servidor responde com um único objeto **CertificateRequestGenerationOptions** SERIALIZADO em JSON. O objeto inclui parâmetros para a exportabilidade da chave, o nome amigável do certificado, o algoritmo de hash, o algoritmo de chave, o tamanho da chave e assim por diante. O cliente usa esses parâmetros para gerar uma solicitação de certificado.

26. A solicitação de certificado única é enviada ao servidor. O cliente também poderia especificar uma senha PFX que deve ser usada para descriptografar quaisquer BLOBs PFX quando o modelo de certificado especificar o arquivamento dos certificados na autoridade de certificação, ou seja, a CA gerará o par de chaves e, em seguida, o enviará para o cliente. O cliente também pode optar por adicionar alguns comentários.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27. Depois de alguns segundos, o servidor responde com um único objeto **Microsoft. CLM. Shared. Certificate** SERIALIZADO em JSON. Ao verificar o sinalizador **isPkcs7** , o cliente aprende que essa resposta não é um blob PFX. O cliente extrai o blob por Base64 decodificando a cadeia de caracteres PKCS7 e a instala.

28. O cliente marca a solicitação como concluída.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
