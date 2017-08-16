---
title: "Referência de função para o Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Referência de funções para o Microsoft Identity Manager 2016


No MIM (Microsoft Identity Manager) 2016, as funções permitem que você modifique os valores de atributos antes de encaminhá-los para um destino em uma atividade de função ou provisionamento declarativo. O objetivo deste documento é fornecer uma visão geral das funções disponíveis e uma descrição de como você pode usá-las.

A configuração de mapeamentos de fluxo de atributos é uma tarefa fundamental ao configurar regras de sincronização. A forma mais simples de mapeamento de fluxo de atributo é um mapeamento direto. Conforme indicado pelo nome, um mapeamento direto pega o valor de um atributo de origem e o aplica ao atributo de destino configurado. Há casos nos quais é necessário modificar os valores de atributo existentes ou calcular novos valores de atributo antes de o sistema aplicá-los a um destino.

As funções são um método interno usado para definir qual é o tipo de modificação que o mecanismo de sincronização precisa aplicar ao gerar um valor de atributo para um destino.

No MIM, você pode agrupar as funções existentes nas seguintes categorias:

-   **Funções de manipulação de dados**. Funções para executar várias operações de manipulação em cadeias de caracteres.

-   **Funções de recuperação de dados**. Funções para extrair dados de valores de atributo.

-   **Funções de geração de dados**. Funções para gerar valores.

-   **Funções lógicas**. Funções para executar operações com base em condições.

As seções a seguir fornecem mais detalhes sobre as funções em cada categoria.

## <a name="data-manipulation-functions"></a>Funções de manipulação de dados

As funções de manipulação de dados são usadas para executar várias operações de manipulação em cadeias de caracteres.

| Concatenate        |   |
|--------------------|-------------------------|
| Descrição        | A função Concatenate é usada para concatenar duas ou mais cadeias de caracteres.                                                                                                       |
| Assinatura da função | string1 + string2...                                                                                                                                                     |
| Entradas             | Duas ou mais cadeias de caracteres                                                                                                                                                        |
| Operações         | Todos os parâmetros de cadeia de caracteres de entrada são concatenados uns aos outros.                                                                                                              |
| Saída             | Uma cadeia de caracteres        |


| UpperCase         |         |
|-------------------|---------|
| Descrição        | A função UpperCase converte todos os caracteres de uma cadeia em letras maiúsculas.         |
| Assinatura da função | String UpperCase(string)                                                                                                                                                   |
| Entradas             | Uma cadeia de caracteres                                                                                                                                                                 |
| Operações         | Todos os caracteres minúsculos do parâmetro de entrada são convertidos em letras maiúsculas. Exemplo: UpperCase("teste") resulta em "TESTE".                                     |
| Saída             | Uma cadeia de caracteres                                                              |


| LowerCase          |                                 |
|--------------------|---------------------------------|
| Descrição        | A função LowerCase converte todos os caracteres em uma cadeia de caracteres em letras minúsculas.                                                                                                  |
| Assinatura da função | String LowerCase(string)                                                                                                                                                   |
| Entradas             | Uma cadeia de caracteres                                                                                                                                                                 |
| Operações         | Todos os caracteres maiúsculos do parâmetro de entrada são convertidos em letras minúsculas. Exemplo: LowerCase("TeSte") resulta em "teste".                                     |
| Saída             | Uma cadeia de caracteres               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Descrição        | A função ProperCase converte o primeiro caractere de cada palavra delimitada por espaço em uma cadeia de caracteres em letras maiúsculas, e todos os outros caracteres são convertidos em letras minúsculas.           |
| Assinatura da função | String ProperCase(string)                                                                                                                                                  |
| Entradas             | Uma cadeia de caracteres                                                                                                                                                                 |
| Operações         | O primeiro caractere de cada palavra delimitada por espaço em um parâmetro de entrada é convertido em letras maiúsculas, e todos os caracteres maiúsculos são convertidos em caracteres minúsculos. Se uma palavra no parâmetro de entrada começar com um caractere que não faz parte do alfabeto, o primeiro caractere da palavra não será convertido em letra maiúscula. <br/> Exemplos: <br/> – ProperCase("TEsTe") resulta em "Teste". <br/> – ProperCase("brenda fernandes") resulta em "Brenda Fernandes". <br/>– ProperCase(" TEsTe") resulta em " Teste". <br/> – ProperCase("\$TEsT") resulta em "\$Teste".|
| Saída             | Uma cadeia de caracteres      |


| LTrim              |      |
|--------------------|------|
| Descrição        | A função LTrim remove o entrelinhamento dos espaços em branco a partir de uma cadeia de caracteres.                                                                                                             |
| Assinatura da função | String LTrim(string)                                                                                                                                                       |
| Entradas             | Uma cadeia de caracteres                                                                                                                                                                 |
| Operações         | Os caracteres de espaço em branco à esquerda contidos no parâmetro de entrada são removidos. <br/><br/>Exemplo: LTrim(" Test ") resulta em "Teste ".                                              |
| Saída             | Uma cadeia de caracteres      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função RTrim remove espaços em branco à direita de uma cadeia de caracteres.                                                                 |
| Assinatura da função | String RTrim(string)                                                                                                            |
| Entradas             | Uma cadeia de caracteres                                                                                                                      |
| Operações         | Os caracteres de espaço em branco à direita contidos no parâmetro de entrada são removidos. Exemplo: RTrim(" Test ") resulta em " Teste".  |
| Saída             | Uma cadeia de caracteres                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função Trim remove espaços à direita e em branco a partir de uma cadeia de caracteres.                                                      |
| Assinatura da função | String Trim(string)                                                                                                             |
| Entradas             | Uma cadeia de caracteres                                                                                                                      |
| Operações         | Os caracteres de espaço em branco à esquerda e à direita contidos na cadeia de caracteres são removidos. Exemplo: Trim(" Test ") resulta em "Teste". |
| Saída             | Uma cadeia de caracteres                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função RightPad preenche à direita uma cadeia de caracteres para um comprimento especificado usando um caractere de preenchimento fornecido.                          |
| Assinatura da função | String RightPad(string, length, padCharacter)                                                                                   |
| Operações         | Se o comprimento da cadeia de caracteres for menor do que o comprimento, o Caractere de preenchimento será repetido até o final da cadeia de caracteres, até que atinja um tamanho igual ao comprimento. <br/> Exemplos: <br/> – RightPad("Usuário", 10, "0") resultaria em "Usuário000000". <br/> – RightPad(RandomNum(1,10), 5, "0") poderia resultar em "9000".   |
| Saída                                                                                                                                                          | Se a cadeia de caracteres tem um comprimento maior ou igual ao comprimento, uma cadeia de caracteres idêntica à cadeia de caracteres será retornada. Se o comprimento da cadeia de caracteres for menor do que o comprimento, então uma nova cadeia de caracteres do tamanho desejado é retornada, contendo a cadeia de caracteres preenchida com um padCharacter. Se a cadeia de caracteres for nula, a função retorna uma cadeia de caracteres vazia. |   |   |
>[!NOTE]
**padCharacter** pode ser um caractere de espaço, mas não pode ser um valor nulo. Se o comprimento da **string** for igual ou maior do que o **length**, a **string** retornará inalterada.


| LeftPad      |     |
|----|-------|
| Descrição  | A função LeftPad preenche à esquerda uma cadeia de caracteres com um comprimento especificado usando um caractere de preenchimento fornecido.    |
| Assinatura da função      | String LeftPad(string, length, padCharacter)     |
| Entradas |  - **String.** A cadeia de caracteres para preenchimento. <br/> - **length.** Um inteiro que representa o comprimento da cadeia de caracteres desejado. <br/> - **padCharacter.** Uma cadeia de caracteres composta por um único caractere que será usado como um caractere de preenchimento. |
| Operações  | Se o comprimento da cadeia de caracteres for menor que o comprimento, padCharacter é repetidamente anexado no início da cadeia de caracteres até que tenha um comprimento igual ao comprimento. <br/> Exemplos: <br/> – LeftPad("Usuário", 10, "0") resultaria em "000000Usuário". <br/> LeftPad(RandomNum(1,10), 5, "0") poderia resultar em "0009". |  
|Saída | Se a cadeia de caracteres tem um comprimento maior ou igual ao comprimento, uma cadeia de caracteres idêntica à cadeia de caracteres será retornada. <br/> Se o comprimento da cadeia de caracteres for menor do que o comprimento, então uma nova cadeia de caracteres do tamanho desejado é retornada, contendo a cadeia de caracteres preenchida com um padCharacter. <br/>  Se a **string** for nula, a função retorna uma cadeia de caracteres vazia.                                                   |

<[!NOTE]
**padCharacter** pode ser um caractere de espaço, mas não pode ser um valor nulo. Se o comprimento da **string** for igual ou maior do que o **length**, a **string** retornará inalterada.

| BitOr    |  |
|----- |------|
| Descrição  | A função BitOr define um bit específico em um sinalizador como 1.     |
| Assinatura da função  | Int BitOr(mask, flag)       |  
| Entradas     | 1. **mask.** Um valor hexadecimal que especifica o bit a ser definido no sinalizador. <br/> 2. **flag.** Um valor hexadecimal do qual um bit específico deve ser modificado.    |   
| Operações         | Essa função converte os dois parâmetros em uma representação binária e os compara: <br/> – Define um bit como 1 se um ou os dois bits correspondentes em mask e em flag forem 1, e como 0 se os dois bits correspondentes forem 0. <br/> – Ele retorna 1 em todos os casos, exceto onde os bits correspondentes de ambos os parâmetros são 0. <br/> – O padrão de bits resultante é o "conjunto" (1 ou true), de qualquer um dos dois operandos. Vários bits de flag podem ser definidos se vários bits tiverem o valor 1 em mask.  |
| Saída             | Uma nova versão do **flag**, com os bits especificados na **mask** definidos como 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Descrição        | A função BitAnd define um bit específico em um sinalizador como 0.                           |
| Assinatura da função | Int BitOr(mask, flag)                                                              |
| Entradas             | 1. **mask.** Um valor hexadecimal que especifica o bit a ser modificado no sinalizador. <br/> 2. **flag.** Um valor hexadecimal do qual um bit específico deve ser modificado   |
| Operações         | Essa função converte os dois parâmetros em uma representação binária e os compara: <br/> – Define um bit como 0 se um ou os dois bits correspondentes em **mask** e em **flag** forem 0, e como 1 se os dois bits correspondentes forem 1. <br/> – Ele retorna 0 em todos os casos, exceto onde os bits correspondentes de ambos os parâmetros são 1. Vários bits de sinalizador podem ser definidos como 0 se vários bits tiverem o valor de 0 em **mask.** |
| Saída             | Uma nova versão do **flag**, com os bits especificados na **mask** definidos como 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Descrição       | A função DateTimeFormat é usada para formatar uma DataHora do formato de cadeia para um formato especificado.     |
| Assinatura da função   | String DateTimeFormat(dateTime, format)      |
| Entradas   | 1. dateTime. Uma cadeia de caracteres que representa a DataHora para formatar.  <br/> 2. **format.** Uma cadeia de caracteres que representa o formato para conversão.  |   
| Operações           | A cadeia de formato especificada no formato é aplicada a DataHora na cadeia de caracteres dateTime. <br/> A cadeia de caracteres especificada no formato deve ser um formato de DataHora válido. Se não for, um erro retornará indicando que o formato não é um formato de DataHora válido. <br/> Exemplo: DateTime("12/25/2007", "aaaa-MM-dd") resulta em "2007-12-25".|   
| Saída     | Uma cadeia de caracteres resultante da aplicação de **format** em **dateTime.**   |

>[!Note]                                                                                                                                                                             
Para que os caracteres aceitos criem formatos definidos pelo usuário, confira [Formatos de data/hora definidos pelo usuário](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Descrição       | A função ConvertSidToString converte uma matriz de bytes que contém um identificador de segurança em uma cadeia de caracteres.         |
| Assinatura da função      | String ConvertSidToString(ObjectSID)    |
| Entradas  | **ObjectSID.** Uma matriz de bytes que contém um identificador de segurança (SID).   |
| Operações    | O SID do binário especificado é convertido em uma cadeia de caracteres.    |
| Saída              | Uma representação da cadeia de caracteres do SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Descrição         | A função **ConvertStringToGuid** converte a representação da cadeia de caracteres de um GUID em uma representação binária do GUID.      |
| Função            | Byte[] ConvertStringToGuid(stringGuid)  |  
| Entradas              | **Guid.** Uma cadeia de caracteres formatada neste padrão: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, no qual o valor do GUID é representado como uma série de dígitos hexadecimais em grupos de 8, 4, 4, 4 e 12 dígitos e separados por hifens. Este é um exemplo de valor de retorno: "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Operações          | A cadeia de caracteres **Guid** especificada no parâmetro 1 é convertida em sua representação binária. <br/> Se a cadeia de caracteres não for uma representação de um **Guid** válido, a função rejeitará o argumento com o seguinte erro: <br/> **O parâmetro para a função ConvertStringToGuid deve ser uma cadeia de caracteres que representa um Guid válido.**  |
| Saída              | Uma representação binária do Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Descrição         | A função ReplaceString substitui todas as ocorrências de uma cadeia de caracteres em outra cadeia de caracteres.  |   
| Função            | String ReplaceString(string, OldValue, NewValue)    |                                                                          
| Entradas              | 1. **String.** Uma cadeia de caracteres para substituição dos valores. <br/> 2. **OldValue.** A cadeia de caracteres para pesquisar e substituir. <br/> 3. NewValue. A cadeia de caracteres para substituir. |
| Operações          | Todas as ocorrências de OldValue na cadeia de caracteres serão substituídas por NewValue. A função deve ser capaz de lidar com estes caracteres especiais: <br/> - **\n.** Nova Linha. <br/> - **\r.** Retorno de Carro. <br/> - **\t.** Guia. <br/> Exemplo: ReplaceString("One\n\rMicrosoft\n\r\Way","\n\r"," ") retorna "One Microsoft Way". |   
| Saída              | Uma cadeia de caracteres com todas as ocorrências de **OldValue** na cadeia de caracteres substituídas por **NewValue.**      |

## <a name="data-retrieval-functions"></a>Funções de recuperação de dados

As funções de recuperação de dados são usadas para executar operações que recuperam os caracteres desejados de uma cadeia de caracteres.

| Word       |        |
|--------------------|---------------|
| Descrição        | A função Word retorna uma palavra contida em uma cadeia de caracteres com base nos parâmetros que descrevem os delimitadores para uso e o número de palavras para retornar.                                                                |
| Assinatura da função | String Word(string, number, delimiters)                                                                                                                                                                        |
| Entradas             | 1. **string.** A cadeia de caracteres a partir da qual retornar uma palavra. <br/> 2. **number.** Um número que identifica qual número de palavras deve retornar. <br/> 3. **delimeters.** Uma cadeia de caracteres que representa os delimitadores que devem ser usados para identificar palavras. |
| Operações         | Cada cadeia de caracteres separada por um dos caracteres nos delimitadores é identificada como uma palavra. A palavra encontrada na posição especificada no parâmetro 3 (number) é retornada: <br/> – Se number < 1, retornará uma cadeia de caracteres vazia. <br/> – Se string for nula, retornará uma cadeia de caracteres vazia. <br/><br/> Exemplos: <br/> 1. Word("Teste;de%função;", 3, ";$&%") retorna "função". <br/> 2. Word("Testar;;Função" , 2 , ";") retorna "" (uma cadeia vazia). 3. Word("Teste;de%função;", 0, ";$&%") retorna "" (uma cadeia vazia).
| Saída             | Uma cadeia de caracteres que contém a palavra na posição solicitada pelo usuário. Se **string** tiver uma quantidade menor do que o número de palavras, ou **string** não contiver palavras identificadas por **delimitadores**, uma cadeia de caracteres vazia retornará. |  


| Esquerda               |   |
|-------|-------|
| Descrição        | A função Left retorna um número especificado de caracteres do lado esquerdo de uma cadeia de caracteres.       |
| Assinatura da função | String Left(string, numChars)     |
| Entradas             | 1. **string.** A cadeia de caracteres a partir da qual retornar os caracteres. 2. **numChars.** Um número que identifica o número de caracteres para retornar do início de uma cadeia de caracteres.         |
| Operações         | Os caracteres em **numChars** são retornados da primeira posição da cadeia de caracteres. <br/> Exemplo: Left("Brenda Fernandes", 3) retorna "Bre".   |
| Saída             | Uma cadeia de caracteres que contém os primeiros caracteres numChars na cadeia de caracteres.  <br/> – Se numChars = 0, retornará uma cadeia de caracteres vazia. <br/> – Se numChars < 0, retornará uma cadeia de caracteres de entrada. <br/> – Se string for nula, retornará uma cadeia de caracteres vazia. |




| Right       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descrição | A função Right retorna um número especificado de caracteres a partir da direita (fim) de uma cadeia de caracteres.                                 |
| Assinatura da função   | String Right(string, numChars)   |
| Entradas      | 1. **String.** A cadeia de caracteres a partir da qual retornar os caracteres. <br/> 2. **numChars.** Um número que identifica o número de caracteres para retornar a partir do final da cadeia de caracteres.  |
| Operações  | **numChars.** Caracteres retornam do final de uma cadeia de caracteres. <br/> Exemplo: Right("Brenda Fernandes", 3) retorna "des".                  |
| Saída      | Uma cadeia de caracteres que contém os últimos caracteres numChars em uma cadeia de caracteres. Se numChars = 0, retornará uma cadeia de caracteres vazia. <br/> – Se **numChars** < 0, retornará uma cadeia de caracteres de entrada. <br/> – Se string for nula, retornará uma cadeia de caracteres vazia. <br/> – Se string contiver menos caracteres que o número especificado no numChars, uma cadeia de caracteres idêntica à cadeia de caracteres será retornada. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descrição | A função Mid retorna um número especificado de caracteres a partir de uma posição especificada em uma cadeia de caracteres.                              |
| Assinatura da função    | String Mid(string, pos, numChars)                                                                                             |
| Entradas      | 1. **string.** A cadeia de caracteres a partir da qual retornar os caracteres.   <br/> 2. **pos.** Um número que identifica a posição inicial em uma cadeia de caracteres para o retorno de caracteres. <br/> 3. **numChars.** Um número que identifica o número de caracteres para retorno a partir de uma posição na cadeia de caracteres.  |
| Operações  | Retorna **numChars** caracteres a partir da posição **pos** na cadeia de caracteres. <br/>Exemplo: Mid ("Brenda Fernandes", 3, 5) retornaria "enda". |
| Saída      | Uma cadeia de caracteres que contém **numChars** caracteres a partir da posição **pos** em uma cadeia de caracteres: <br/> – Se **numChars** = 0, retornará uma cadeia de caracteres vazia. <br/> – Se **numChars** < 0, retornará uma cadeia de caracteres vazia. <br/> – Se **pos** > o comprimento da cadeia de caracteres, retornará uma cadeia de caracteres de entrada. <br/> – Se **pos** ≤ 0, retornará uma cadeia de caracteres de entrada. <br/> – Se **string** for nula, retornará uma cadeia de caracteres vazia. <br/> Se não houver **numChar** caracteres restantes na **string** a partir da posição **pos**, a quantidade possível de caracteres retornará.

## <a name="data-generation-functions"></a>Funções de geração de dados

As funções de geração de dados são usadas para gerar valores para tipos de dados específicos.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Descrição        | O CRLF gera um Retorno de Carro/Alimentação de Linha. Use essa função para adicionar uma nova linha. |
| Assinatura da função | String CRLF                                                                              |
| Entradas             | Sem parâmetros                                                                            |
| Operações         | Um CRLF retorna.                                                                      |
|                    | Exemplo: Linha de endereço1 + CRLF() + Linha de endereço2 resulta em: <br/> – Linha de endereço1 <br/> – Linha de endereço2 |
| Saída             | Um CRLF é a saída.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função RandomNum retorna um número aleatório dentro de um intervalo especificado.                                       |   
| Assinatura da função | Int RandomNum(start, end)                                                                                         |   
| Entradas             | - **start**. Um número que identifica o limite inferior do valor aleatório para gerar.   <br/> - **end**. Um número que identifica o limite superior do valor aleatório para gerar.  |
| Operações         | Um número aleatório maior ou igual a **start** e menor ou igual a **end** é gerado. <br/>  Exemplo: Random(0.999) poderia retornar 100.                      |
| Saída             | Um número aleatório dentro do intervalo especificado por **start** e **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descrição        | O método *EscapeDNComponent* processa a cadeia de caracteres de entrada com base no tipo de agente de gerenciamento que está sendo usado. |
| Assinatura da função | String EscapeDNComponent(string)                                                                                  |
| Entradas     | **string**. Uma cadeia de caracteres usada para processar um nome distinto. A cadeia de caracteres não deve conter caracteres de escape. |
| Operações | O método EscapeDNComponent de MIISUtils é usado para executar essa operação. Esse método processa a cadeia de caracteres de entrada com base no tipo de agente de gerenciamento que está sendo usado. <br/> Como agentes de gerenciamento diferentes exigem formatos de nome distinto diferentes, este método processa as cadeias de caracteres de entrada com base no tipo do agente de gerenciamento. Os tipos são nome distinto LDAP (Lightweight Directory Access Protocol), como Active Directory® Domain Services, Sun Directory Server (conhecido antes como iPlanet Directory Server), Microsoft Exchange Server; não LDAP hierárquico, como Microsoft Lotus Notes; e extrínseco, como banco de dados e XML sem nomes distintos de LDAP. <br/> **Nome distinto de LDAP: ** <br/> – Todos os caracteres XML inválidos na porção de valor de uma determinada parte são codificados em formato hexadecimal. <br/>– Todos os caracteres ilegais (incluindo caracteres XML inválidos) na porção de nome de uma determinada parte geram um erro. <br/> Estes caracteres são escapados: <br/> &nbsp;&nbsp;&nbsp; – Vírgula (',') <br/> &nbsp;&nbsp;&nbsp; – Sinal de igual ('=') <br/> &nbsp;&nbsp;&nbsp; – Sinal de adição ('+') <br/> &nbsp;&nbsp;&nbsp; – Sinal de menor ('<') <br/> &nbsp;&nbsp;&nbsp; – Sinal de maior ('>') <br/> &nbsp;&nbsp;&nbsp; – Tecla jogo da velha ('#') <br/> &nbsp;&nbsp;&nbsp; – Ponto e vírgula (';') <br/> &nbsp;&nbsp;&nbsp; – Barra invertida ('\') <br/> &nbsp;&nbsp;&nbsp; – Aspas ('"') <br/> – Se o último caractere na cadeia de caracteres for um espaço, esse espaço será escapado. <br/> – Qualquer espaço incorreto à esquerda ou à direita ao redor de um nome de parte será removido. <br/> – Para o agente de gerenciamento de XML, se houver várias partes, as partes serão colocadas em ordem alfabética. <br/> – Se várias partes forem especificadas, a cadeia de caracteres de nome distinto composta será a concatenação das cadeias de caracteres individuais separadas por sinais de adição. <br/> – Um erro será gerado se a cadeia de caracteres de entrada não for uma cadeia de caracteres de nome distinto no estilo LDAP bem formada. <br/><br/> **LDAP não hierárquica** <br/> – Esses agentes de gerenciamento não dão suporte a componentes com diversas partes. Se várias cadeias de caracteres forem passadas para EscapeDNComponent, uma ArgumentException será gerada. <br/> – Se qualquer um dos caracteres na cadeia de caracteres de entrada for um caractere XML inválido, ArgumentException será gerada. <br/> – Todas as vírgulas e barras invertidas na cadeia de caracteres de entrada são escapadas. <br/> – Se o último caractere na cadeia de caracteres for um espaço, esse espaço será escapado. <br/><br/> **Extrínseco:** <br/> 1. Se qualquer parte for binária ou contiver um caractere XML inválido, essa parte será armazenada como uma versão codificada em formato hexadecimal dos dados brutos, com um caractere '#' prefixado à frente da cadeia de caracteres. Por exemplo, se uma parte for 'AxC' (em que x representa um caractere XML ilegal, como '0x10'), essa parte será codificada como '#410010004300'. <br/> 2. Caso contrário, todas as instâncias dos caracteres a seguir serão escapadas: <br/> &nbsp;&nbsp;&nbsp; – Barra invertida ('\') <br/> &nbsp;&nbsp;&nbsp; – Vírgula (',') <br/> &nbsp;&nbsp;&nbsp; – Sinal de adição ('+') <br/> &nbsp;&nbsp;&nbsp; – Tecla jogo da velha ('#') <br/> 3. Se o último caractere em uma determinada cadeia de caracteres de parte for um espaço, esse espaço será escapado. <br/> 4. Se várias partes forem especificadas, a cadeia de caracteres de nome distinto composta será a concatenação de todas as cadeias de caracteres individuais separadas por sinais de adição.
| Saída      | Uma cadeia de caracteres que contém um nome de domínio válido.                                                                                                                  |   

>[!NOTE]
A validação de nomes distintos é menos rigorosa do que a sintaxe definida nas especificações do LDAP. EscapeDNComponent(String[]) permite que um nome de parte contenha qualquer combinação de um ou mais destes caracteres 'a'-'z', 'A'-'Z', '0'-'9', '-' e '.'. <br/>
Não é possível especificar uma parte binária com esse método. No entanto, é possível ter uma parte binária em **CommitNewConnector** se o nome distinto for construído a partir de atributos de âncora, e um dos atributos de âncora for um tipo binário.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Descrição | A função Null é usada para definir se esse MA não tem um atributo para contribuir, e se a precedência do atributo deve continuar com o próximo MA. |   
| Assinatura da função    | String Null    |
| Entradas      | Sem parâmetros                                                                                                                                             |   
| Operações  | Um Null retorna. <br/> Exemplo: IIF(Eq(domain), "desconhecido", Null())                                                                                           |   
| Saída      | Um Null é exibido na saída.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Funções lógicas
As Funções lógicas são usadas para executar uma operação com base nas condições avaliadas pelo sistema.

| IIF        |  |
|-------------|---|
| Descrição | A função IIF retorna um de um conjunto de valores possíveis com base em uma condição específica.    |
| Assinatura da função   | Object IIF(condition, valueIfTrue, valueIfFalse)   |                                                 |
| Entradas      | 1. **Condition**. Qualquer valor ou expressão que possa ser avaliada como verdadeira ou falsa. 2. **valueIfTrue** Um valor que será retornado se a condição for avaliada como verdadeira. <br/> 3. **valueIfFalse** Um valor que será retornado se a condição for avaliada como falsa. <br/><br/> Veja a seguir funções disponíveis para uso como expressões na função IIF como **condição:** <br/> **Eq.** Essa função compara dois argumentos para verificar se há igualdade. <br/> **NotEquals.** Essa função compara dois argumentos para verificar se há desigualdade, retornando true se não forem iguais, e false se forem iguais.<br/> Exemplo: NotEquals(EmployeeType, "Prestador de Serviços")<br/> **LessThan.** Essa função compara dois números, retornando true se o primeiro for menor do que o segundo, e false caso contrário.<br/>Exemplo: LessThan(Salary, 100000) <br/>**GreaterThan.** Essa função compara dois números, retornando true se o primeiro for maior do que o segundo, e false caso contrário.<br/> Exemplo: GreaterThan(Salary, 100000) <br/> **LessThanOrEquals.** Essa função compara dois números, retornando true se o primeiro for menor ou igual ao segundo, e false caso contrário.<br/>Exemplo: LessThanOrEquals(Salary, 100000) <br/> **GreaterThanOrEquals.* Essa função compara dois números, retornando true se o primeiro for maior ou igual ao segundo, e false caso contrário. <br/>Exemplo: GreaterThanOrEquals(Salary, 100000)<br/> IsPresent. Essa função usa como entrada um atributo no esquema ILM e retorna true se o atributo não for nulo, e false se o atributo for nulo.|
| Operações  | Se **condition** for avaliada como true, retornará **valueIfTrue.** Caso contrário, retornará **valueIfFalse.** <br/>Exemplo: IIF(Eq(EmployeeType,"Estágio"),"t-" + Alias, Alias) retorna o alias de um usuário com "t-" adicionado ao início do mesmo, se o usuário for um estagiário. Caso contrário, retornará o alias do usuário como está. |
| Saída      | A saída é **valueIfTrue** se a condição for verdadeira ou **valueIfFalse** se a condição for falsa. |      
