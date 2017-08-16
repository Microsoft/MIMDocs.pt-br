---
title: Exemplo passo a passo de registro | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Instruções passo a passo de registro de exemplo
Este tópico mostra as etapas necessárias para executar um registro de autoatendimento de Cartão Inteligente Virtual. Mostra uma solicitação de aprovação automática com um PIN definido pelo usuário.
1.  O cliente autentica o usuário e solicita uma lista de modelos de perfil para os quais o usuário autenticado pode se registrar:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  O cliente exibe a lista resultante ao usuário. O usuário seleciona um modelo de perfil vSC (Cartão Inteligente Virtual) com o nome "VPN de Cartão Inteligente Virtual" e o UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  O cliente recupera a política de registro do modelo de perfil selecionado usando o UUID retornado na etapa anterior:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  O cliente analisa o campo "DataCollection" na política retornada, observando que um único item da coleção de dados intitulado "Razão da Solicitação" aparece. O cliente também observa que o sinalizador "collectComments" é definido como *false*, de modo que não solicita que o usuário inserir nenhum.

5.  Depois que o usuário tiver inserido o motivo para exigir os certificados, o cliente cria uma solicitação:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  O servidor cria com sucesso a solicitação e retorna o URI da solicitação ao cliente: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  O cliente recupera o objeto de solicitação chamando o URI retornado:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  O cliente verifica se a propriedade "estado" na solicitação está definida como "aprovada". A execução da solicitação pode começar.

9.  O cliente examina a solicitação analisando o conteúdo do parâmetro "newsmartcarduuid" para ver se já existe um cartão inteligente associado à solicitação.

10. Uma vez que ele contém apenas um GUID vazio, o cliente deve usar um cartão existente ainda não em uso pelo MIM CM ou criar um, caso o modelo de perfil seja configurado para cartões inteligentes virtuais.

11. Como este último foi indicado ao cliente por meio da consulta inicial para os modelos de perfil registráveis (etapa 1), agora ele deve criar um dispositivo de Cartão Inteligente Virtual.

12. O cliente recupera a política de cartão inteligente do modelo de perfil, usando o UUID do modelo selecionado na etapa 3:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. A política de cartão inteligente incluirá a chave de administração padrão do cartão na propriedade "DefaultAdminKeyHex". Quando criar o cartão inteligente, o cliente deve definir a chave de administração inicial do cartão inteligente para essa chave.  

14. Após a criação do dispositivo de cartão inteligente, ele deve atribuí-lo à solicitação:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. O servidor responderá ao cliente com um URI para o objeto de cartão inteligente recém-criado: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. O cliente recupera o objeto de cartão inteligente:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Quando marca o valor do sinalizador "diversifyadminkey" na política de cartão inteligente obtida na etapa 12, o cliente sabe que deve diversificar a chave de administração.

18. O cliente recupera a chave de administração proposta:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. O cliente deve autenticar ao cartão como administrador para definir a chave de administração. Para fazer isso, ele solicita um desafio de autenticação do cartão inteligente e o envia ao servidor:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    O servidor envia a resposta de desafio no corpo da resposta HTTP. Localize a cadeia de caracteres do desafio hexadecimal, converta em base64 e passe-a como um parâmetro na URL. A URL retornará outra resposta, que deve ser convertida para o formato hexadecimal.
<br/>
20. O servidor envia a resposta do desafio no corpo da resposta HTTP e o cliente o utiliza para diversificar a chave de administração.

21. O cliente observa que o usuário deve fornecer o PIN desejado inspecionando o campo "UserPinOption" na política de cartão inteligente.

22. Depois que o usuário tiver inserido o pin desejado em uma caixa de diálogo, o cliente executa a autenticação de desafio/resposta, conforme descrito na etapa 19, sendo a única diferença que o sinalizador diversificado agora deve estar definido para "true":

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. O cliente usa a resposta recebida do servidor para definir o pin do usuário desejado.

24. O cliente agora está pronto para gerar solicitações de certificado. Ele consulta os parâmetros de geração de certificado de modelo do perfil para determinar como as solicitações/chaves devem ser geradas.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. O servidor responde com um único objeto JSON serializado CertificateRequestGenerationOptions detalhando a capacidade de exportação da chave, o nome amigável do certificado, o algoritmo de hash, o algoritmo de chave, o tamanho da chave, etc. O cliente usa esses parâmetros para gerar uma solicitação de certificado.

26. A solicitação de certificado única é enviada ao servidor. O cliente também poderia especificar uma senha PFX que deveria ser usada para descriptografar todos os blobs do PFX no caso de o modelo de certificado especificar o arquivamento de certificados na autoridade de certificação, por exemplo, autoridade de certificação gera o par de chaves e as envia ao cliente. O cliente também pode optar por adicionar alguns comentários.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Depois de alguns segundos, o servidor responde com um objeto Microsoft.Clm.Shared.Certificate único serializado para JSON. Ao consultar o sinalizador "isPkcs7", o cliente aprende que essa resposta não é um blob do PFX. O cliente extrai o blob por base64 decodificando da cadeia de caracteres “pkcs7” e a instala.

28. O cliente marca a solicitação como concluída.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
