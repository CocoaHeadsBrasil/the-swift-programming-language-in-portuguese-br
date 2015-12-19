# Tratamento de Erro

Tratamento de erro é o processo de responder e se recuperar de condições de erro em seu programa. O Swift oferece um sofisticado suporte para lançamento, captura, propagação e manipulação de erros recuperáveis em tempo de execução.

Algumas operações não têm garantia de sempre completar a execução ou produzir uma saída útil. *Optionals* são usados para representar a ausência de um valor, mas quando uma operação falha, geralmente é mais útil entender o que causou a falha para que assim seu código possa responder adequadamente.

Como um exemplo, considere uma tarefa de leitura e processamento de dados de um arquivo no disco. Existem várias possibilidades dessa tarefa falhar, incluindo o arquivo não existir no caminho especificado, o arquivo não ter permissão de leitura ou o arquivo não ter sido codificado em um formato compatível. Distinguir essas diferentes situações permite que um programa resolva alguns erros e comunique ao usuário quaisquer erros que ele não consegue resolver.

> ###### Nota
>Tratamento de erro no Swift funciona em conjunto com padrões de tratamento de erro que usam a classe *NSError* no *Cocoa* e *Objective-C*. Para mais informações sobre essa classe, veja *Using Swift with Cocoa and Objective-C (Swift 2.1)*

## Representando e lançando erros
No Swift, erros são representados por valores de tipos que estão em conformidade com o protocolo *ErrorType*. Esse protocolo vazio indica que um tipo pode ser usado para tratamento de erro.

Enumeradores em Swift são particularmente bem adequados para modelar um grupo de condições de erro relacionados, com valores associados permitindo informação adicional sobre a natureza de um erro à ser comunicado. Por exemplo, aqui está como você deve representar as condições de erro de operação de uma máquina de vendas dentro de um jogo:

>```
> enum MaquinaVendasError: ErrorType {
    case SelecaoInvalida
    case SaldoInsuficiente(moedasNecessarias: Int)
    case EstoqueVazio
 }
>```

Lançar um erro te permite indicar que alguma coisa inesperada aconteceu e o fluxo normal de execução não pode continuar. Você usa a instrução *throw* para lançar um erro. Por exemplo, o seguinte código lança um erro para indicar que cinco moedas adicionais são necessárias para a máquina de vendas:

> ```
> throw MaquinaVendasError.SaldoInsuficiente(moedasNecessarias: 5)
```

## Tratando erros
Quando um erro é lançado, algum código que o envolve deve ser responsável por tratar o erro - por exemplo, corrigindo o problema, tentando outra alternativa ou informando ao usuário sobre a falha.

Existem quatro formas para tratar erros no Swift. Você pode propagar o erro de uma função para o código que chamou essa função, tratar o erro usando uma instrução do-catch, tratar o erro como um valor *optional* ou afirmar que o erro não ocorrerá. Cada opção é descrita na seção abaixo.

Quando uma função lança um erro, ela muda o fluxo do seu programa, então é importante que você possa identificar rapidamente lugares no seu código que possam lançar erros. Para identificar esses lugares no seu código, escreva a palavra-chave *try* - ou as variações try? ou try! - antes do código que chama aquela função, método ou inicializador que pode lançar um erro. Essas palavras-chaves são descritas na seção abaixo.

> ###### Nota
> Tratamento de erro no Swift se assemelha a tratamento de exceções em outras linguagens, com o uso das palavras-chaves *try*, *catch* e *throw*. Não parecido com tratamento de exceções em muitas linguagens - incluindo *Objective-C* - tratamento de erro no Swift não envolve desdobramento da pilha (*unwinding stack*), um processo que pode ser caro computacionalmente. Como tal, as características de  performance de uma instrução *throw* são comparáveis àquelas de uma instrução *return*.
