## Tratamento de Erro

Tratamento de erro é o processo de responder e se recuperar de condições de erro em seu programa. O Swift oferece um sofisticado suporte para lançamento, captura, propagação e manipulação de erros recuperáveis em tempo de execução.

Algumas operações não têm garantia de sempre completar a execução ou produzir uma saída útil. *Optionals* são usados para representar a ausência de um valor, mas quando uma operação falha, geralmente é mais útil entender o que causou a falha para que assim seu código possa responder adequadamente.

Como um exemplo, considere uma tarefa de leitura e processamento de dados de um arquivo no disco. Existem várias possibilidades dessa tarefa falhar, incluindo o arquivo não existir no caminho especificado, o arquivo não ter permissão de leitura ou o arquivo não ter sido codificado em um formato compatível. Distinguir entre essas diferentes situações permite que um programa resolva alguns erros e comunique ao usuário quaisquer erros que ele não consiga resolver.

> ###### Nota
>Tratamento de erro no Swift funciona em sincronia com padrões de tratamento de erro que usam a classe *NSError* no *Cocoa* e *Objective-C*. Para mais informações sobre essa classe, veja *Using Swift with Cocoa and Objective-C (Swift 2.1)*

### Representando e lançando erros
No Swift, erros são representados por valores de tipos que estão em conformidade com o protocolo *ErrorType*. Esse protocolo vazio indica que um tipo pode ser usado para tratamento de erro.

Enumeradores em Swift são particularmente bem adequados para modelar um grupo de condições de erro relacionados, com valores associados permitindo informação adicional sobre a natureza de um erro à ser comunicado. Por exemplo, aqui está como você poderia representar as condições de erro da operação de uma máquina de vendas dentro de um jogo:

```Swift
enum VendingMachineError: ErrorType {
    case InvalidSelection
    case InsufficientFunds(coinsNeeded: Int)
    case OutOfStock
}
```

Lançar um erro te permite indicar que alguma coisa inesperada aconteceu e o fluxo normal de execução não pode continuar. Você usa uma instrução *throw* para lançar um erro. Por exemplo, o seguinte código lança um erro para indicar que cinco moedas adicionais são necessárias para a máquina de vendas:

```Swift
throw VendingMachineError.InsufficientFunds(coinsNeeded: 5)
```

### Tratando erros
Quando um erro é lançado, algum código que o envolve deve ser responsável por tratar o erro - por exemplo, corrigindo o problema, tentando outra alternativa ou informando ao usuário sobre a falha.

Existem quatro formas para tratar erros no Swift. Você pode propagar o erro de uma função para o código que a chamou, tratar o erro usando uma instrução do-catch, tratar o erro como um valor *optional* ou afirmar que o erro não ocorrerá. Cada opção é descrita na seção abaixo.

Quando uma função lança um erro, ela muda o fluxo do seu programa, então é importante que você possa identificar rapidamente lugares no seu código que possam lançar erros. Para identificar esses lugares no seu código, escreva a palavra-chave *try* - ou as variações try? ou try! - antes do código que chama aquela função, método ou inicializador que pode lançar um erro. Essas palavras-chaves são descritas na seção abaixo.

> ###### Nota
> Tratamento de erro no Swift se assemelha a tratamento de exceções em outras linguagens, com o uso das palavras-chaves *try*, *catch* e *throw*. Não parecido com tratamento de exceções em muitas linguagens - incluindo *Objective-C* - tratamento de erro no Swift não envolve desdobramento da pilha (*unwinding stack*), um processo que pode ser caro computacionalmente. Como tal, as características de performance de uma instrução *throw* são comparáveis àquelas de uma instrução *return*.

### Propagando erros usando funções lançadoras

Para indicar que uma função, método ou inicializador pode lançar um erro, você escreve a palavra-chave *throws* na declaração de uma função após os parâmetros dela.
Uma função marcada com *throws* é chamada de função lançadora (*throwing function*). Se uma função especifica um tipo de retorno, você escreve a palavra-chave *throws* antes da seta de retorno (->).

```Swift
func canThrowErrors() throws -> String

func cannotThrowErrors() -> String

```

Uma função lançadora propaga erros que são lançados dentro dela para o escopo ao qual a chamou.

> ###### Nota
> Apenas funções lançadoras podem propagar erros. Quaisquer erros lançados dentro de uma função não-lançadora devem ser tratados dentro da função.

No exemplo abaixo, a classe *VendingMachine* tem um método `vend(itemNamed:)` que lança um ‘VendingMachineError' apropriado se o item requisitado não estiver disponível, se o estoque estiver vazio ou o preço está além do valor atual depositado:

 ```Swift
 struct Item {
   var price: Int
   var count: Int
 }

class VendingMachine {
   var inventory = [
      "Candy Bar": Item(price: 12, count: 7),
      "Chips": Item(price: 10, count: 4),
      "Pretzels": Item(price: 7, count: 11)
   ]
   var coinsDeposited = 0
   func dispenseSnack(snack: String) {
      print("Dispensing \(snack)")
   }

   func vend(itemNamed name: String) throws {
       guard var item = inventory[name] else {
           throw VendingMachineError.InvalidSelection
       }

       guard item.count > 0 else {
           throw VendingMachineError.OutOfStock
       }

       guard item.price <= coinsDeposited else {
           throw VendingMachineError.InsufficientFunds(coinsNeeded: item.price - coinsDeposited)
       }

       coinsDeposited -= item.price
       --item.count
       inventory[name] = item
       dispenseSnack(name)
   }
}     
```

A implementação do método `vend(itemNamed:)` usa instruções *guard* para sair do método mais cedo e lançar erros apropriados se algum dos requisitos para comprar um lanche não for cumprido. Como uma instrução *throw* transfere, imediatamente, o controle do programa, um item só será vendido se todos esses requisitos forem cumpridos.

Como o método `vend(itemNamed:)` propaga qualquer erro que ele lança, lugares no seu código que o chamam, devem tratar os error diretamente - usando uma instrução *do-catch*, *try?* ou *try!* - ou continuar a propagá-los. Por exemplo, o `buyFavoriteSnack(_:vendingMachine:)` no exemplo abaixo também é uma função lançadora e quaisquer erros que o método `vend(itemNamed:)` lançar irá ser propagado um nível acima até o ponto onde a função `buyFavoriteSnack(_:vendingMachine:)` é chamada.

```Swift
let favoriteSnacks = [
    "Alice": "Chips",
    "Bob": "Licorice",
    "Eve": "Pretzels",
]

func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}
```

Nesse exemplo, a função `buyFavoriteSnack(_:vendingMachine:)` olha o lanche favorito de uma dada pessoa e tenta comprá-lo para ela chamando o método `vend(itemNamed:)`. Como o método `vend(itemNamed:)` pode lançar um erro, ele é chamado com a palavra-chave *try* na frente dele.

### Trantado erros usando *Do-Catch*

Você usa uma instrução *do-catch* para tratar erros rodando um bloco de código. Se um erro é lançado pelo código na cláusula *do*, ele é comparado com as cláusulas *catch* para determinar qual delas pode tratar o erro.

Aqui está uma forma geral de uma instrução *do-catch*:

```Swift
do {
    try [expression]
    [statements]
} catch [pattern 1] {
    [statements]
} catch [pattern 2] where [condition] {
    [statements]
}
```

Você escreve um padrão (*pattern*) depois do *catch* para indicar que erros aquela cláusula pode tratar. Se uma cláusula *catch* não tiver um padrão, a cláusula combina com qualquer erro e vincula o erro com a constante local chamada *error*. Para mais informações sobre combinação de padrões, veja [Patterns](…/referencialiguagem/padroes).

A cláusula *catch* não precisa tratar todo erro possível que o código dentro da sua cláusula *do* possa lançar. Se nenhuma das cláusulas *catch* pode tratar o erro, o erro é propagado para um escopo que o envolve. Todavia, o erro deve ser tratado por algum escopo que o envolve - ou por uma cláusula *do-catch* que o envolve que trata o erro ou estando dentro de uma função lançadora. Por exemplo, o código seguinte trata os três casos do enumerador *VendingMachineError*, mas todos os outros erros tem que ser tratado por seu escopo envolvente:

```Swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack("Alice", vendingMachine: vendingMachine)
} catch VendingMachineError.InvalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.OutOfStock {
    print("Out of Stock.")
} catch VendingMachineError.InsufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
}
// prints "Insufficient funds. Please insert an additional 2 coins.
```

No exemplo acima, a função `buyFavoriteSnack(_:vendingMachine:)` é chamada em uma expressão *try*, porque ela pode lançar um erro. Se um erro é lançado, a execução transfere imediatamente para a cláusula *catch*, que decide se permite que a propagação continue. Se nenhum erro é lançado, as instrução restantes na instrução *do* são executadas.
