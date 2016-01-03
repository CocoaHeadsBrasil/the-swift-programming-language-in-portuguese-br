## Um Tour pelo Swift

A tradição sugere que o primeiro programa em uma nova linguagem deve imprimir as palavras "Hello, world!" na tela. Em Swift, isso pode ser feito em uma única linha:

```swift
print("Hello, world!")
```

Se você já escreveu código em C ou em Objective-C, essa sintaxe lhe é familiar — em Swift, essa linha de código é um programa completo. Você não precisa importar uma biblioteca separada para funcionalidades como entrada/saída ou manipulação de strings. O código escrito num escopo global é usado como ponto de entrada para o programa, portanto você não precisa de uma função `main()`. Você também não precisa colocar ponto e vírgula no final de cada instrução de código.

Este tour lhe dá informação suficiente para começar a escrever código em Swift, mostrando como realizar diferentes tipos de tarefas de programação. Não se preocupe caso não entenda alguma coisa — tudo que for apresentado nesse tour será explicado em detalhes no restante deste livro.

>NOTA
>
>Em um Mac, baixe este Playground e dê um duplo clique no arquivo para abrí-lo no Xcode: [https://developer.apple.com/go/?id=swift-tour"](https://developer.apple.com/go/?id=swift-tour)

### Valores simples

Use `let` para criar uma constante e `var` para criar uma variável. O valor de uma constante não precisa ser conhecido em tempo de compilação, mas você deve atribuí-lo exatamente uma única vez. Isso significa que você pode usar constantes para nomear um valor que você determina uma vez mas que usa em vários lugares.

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

Uma constante ou uma variável deve ter o mesmo tipo que o valor que você queira atribuir a ela. Contudo, você não precisa escrever o tipo explicitamente. Fornecer um valor quando você cria uma constante ou variável faz com que o compilador deduza o seu tipo. No exemplo acima, o compilador deduz que `myVariable` é um inteiro porque seu valor inicial é um inteiro.

Se o valor inicial não fornecer informação suficiente (ou se não existir um valor inicial), especifique um tipo escrevendo logo após a variável, separado pelo sinal de dois pontos.

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

>EXPERIMENTO
>
>Crie uma constante com tipo explícito `Float` e valor `4`.

Valores nunca são implicitamente convertidos para outro tipo. Se você precisar converter um valor para um tipo diferente, crie uma instância do tipo desejado explicitamente.

```swift
let label = "A largura é "
let width = 94
let widthLabel = label + String(width)
```

>EXPERIMENTO
>
>Tente remover a conversão para `String` na última linha. Que erro você recebe?

Existe uma forma ainda mais simples de incluir valores em strings: Escreva o valor entre parênteses e adicione uma barra invertida antes dos parênteses. Por exemplo:

```swift
let apples = 3
let oranges = 5
let appleSummary = "Eu tenho \(apples) maçãs."
let fruitSummary = "Eu tenho \(apples + oranges) frutas."
```

>EXPERIMENTO
>
>Use `\()` para incluir o cálculo de um número de ponto flutuante em uma string e o nome de alguém em uma saudação.

Crie listas e dicionários usando colchetes (`[]`) e acesse seus elementos escrevendo o índice ou a chave dentro deles. Uma vírgula é permitida após o último elemento.

```swift
var shoppingList = ["peixe", "água", "tulipas", "tinta azul"]
shoppingList[1] = "garrafa d'água"

var occupations = [
	"Malcolm": "Capitão",
	"Kaylee": "Mecânico",
]

occupations["Jayne"] = "Relações Públicas"
```

Para criar uma lista ou um dicionário vazio, use a sintaxe de inicializadores:

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

Se a informação do tipo puder ser inferida, você pode escrever uma lista vazia como `[]` e um dicionário vazio como `[:]` - por exemplo, quando você define um novo valor para uma variável ou passa um argumento para um função.

```swift
shoppingList = []
occupations = [:]
```

### Fluxo de Controle

Utilize `if` e `switch` para criar condições e use `for-in`, `for`, `while` e `repeat-while` para criar laços de repetição. Parênteses ao redor da condição ou da variável de laço são opcionais. Chaves ao redor do bloco de código são obrigatórias.

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
print(teamScore)
```

Na declaração de um `if`, a condição deve ser uma expressão binária - isso significa que código como `if score { ... }` é um erro, não uma comparação implícita com zero.

Você pode usar `if` e `let` juntos para trabalhar com valores que podem estar ausentes. Estes valores são representados como opcionais. Um valor opcional ou contém um valor, ou contém `nil`, para indicar a ausência de valor. Escreva uma ponto de interrogação (`?`) após o tipo de um valor para marcar o valor como opcional.

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

>EXPERIMENTO
>
>Altere `optionalName` para `nil`. Que saudação você recebe? Adicione uma cláusula `else` que define uma saudação diferente se `optionalName` for `nil`.

Se o valor opcional for `nil`, a condição é `false` e o código entre chaves é pulado. Caso contrário, o valor opcional é desempacotado e atribuído à constante após o `let`, tornando-o  disponível dentro do bloco de código.

Outra maneira de lidar com valores opcionais é fornecer um valor padrão usando o operador `??`. Se o valor opcional estiver ausente, o valor padrão será utilizado.

```swift
let nickName: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickName ?? fullName)"
```

As instruções `switch` aceitam qualquer tipo de dado e uma variedade ampla de operações de comparação - elas não são limitadas a números inteiros e testes de igualdade.

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Adicione umas passas e faça um 'Ants on a Log'")
case "cucumber", "watercress":
    print("Isso daria um bom sanduíche")
case let x where x.hasSuffix("pepper"):
    print("É um \(x) apimentado?")
default:
    print("Tudo fica saboroso numa sopa.")
}
```

>EXPERIMENTO
>
>Tente remover o caso `default`. Que erro você recebe?

Repare como o `let` pode ser usado dentro de um padrão para atribuir à uma constante o valor que coincidiu com ele.

Depois de executar o código dentro do `case` que foi utilizado, o programa sai do bloco de `switch`. A execução não continua para o próximo `case`, portanto não há necessidade de quebrar explicitamente o `switch` no final de cada bloco de `case`.

Você usa `for-in` para iterar sobre itens de um dicionário, fornecendo um par de nomes a serem usados para cada par chave-valor. Dicionários são coleções não-ordenadas, portanto suas chaves e valores são iteradas numa ordem arbitrária.

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25]
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
```

>EXPERIMENTO
>
>Adicione outra variável para armazenar de que tipo era o maior número, bem como que número foi esse.


Utilize `while` para repetir um bloco de código até que uma determinada condição seja alterada. A condição do laço de repetição pode estar no final também, garantindo que o laço será executado ao menos uma vez.

```swift
var n = 2
while n < 100 {
    n = n * 2
}
print(n)

var m = 2
repeat {
    m = m * 2
} while m < 100
print(m)
```

Você pode manter um índice em um laço ou usando `..<` para fazer uma série de índices ou escrevendo uma inicialização explícita, condição e incremento. Estes dois laços fazem a mesma coisa:

```swift
var firstForLoop = 0
for i in 0..<4 {
    firstForLoop += i
}
print(firstForLoop)

var secondForLoop = 0
for var i = 0; i < 4; ++i {
    secondForLoop += i
}
print(secondForLoop)
```

Utilize `..<` para criar uma série que ignora o valor mais alto e `...` para criar uma série que inclui ambos os valores.

> NOTA DO TRADUTOR
>
> Esta tradução se baseia na versão de 02/01/2015, e após a disponibilização do livro original sob a CC by 4.0, foi aprovada a [SE-0007](https://github.com/apple/swift-evolution/blob/master/proposals/0007-remove-c-style-for-loops.md), que determina que esse estilo de laço de repetição será removido na versão 3.0 do Swift, e que na versão 2.2 final - o uso desse tipo de construção irá gerar um aviso de compilação - um _warning_. Por isso, o uso deste tipo de laço de repetição é desencorajado."

### Funções e Closures

Utilize `func` para declarar uma função. Chame uma função através de seu nome seguido de uma lista de argumentos entre parênteses. Use `->` para separar os nomes e tipos dos parâmetros do tipo devolvido pela função.

```swift
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}
greet("Bob", day: "Tuesday")
```

>EXPERIMENTO
>
>Remova o parâmetro `day`. Adicione um parâmetro para incluir o prato do dia na saudação.

Utilize uma tupla para criar um valor composto - por exemplo, para devolver múltiplos valores de uma função. Os elementos de uma tupla podem ser referenciados por um nome ou por um número.

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0
    
    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }
    
    return (min, max, sum)
}
let statistics = calculateStatistics([5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```

Funções também podem receber um número variável de argumentos, sendo coletados num _array_.

```swift
func sumOf(numbers: Int...) -> {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(42, 597, 12)
```

>EXPERIMENTO
>
>Escreva uma função que calcula a média dos valores de seus argumentos.

Funções podem ser aninhadas. Funções aninhadas têm acesso às variáveis declaradas na função externa. Você pode usar funções aninhadas para organizar o código de uma função que seja grande ou complexa.

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

Funções são tipos de primeira classe. Isto significa que uma função pode devolver outra função como seu valor.

```swift
func makeIncrementer() -> ((Int) -> (Int)) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

Uma função pode receber uma outra função como um de seus argumentos.

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(numbers, condition: lessThanTen)
```

Funções são na verdade um caso especial de _closures_: blocos de código que podem ser chamados mais tarde. O código em um _closure_ tem acesso a coisas como variáveis e funções que estavam disponíveis no escopo onde o _closure_ foi criado, mesmo se o _closure_ estiver num escopo diferente quando for executado - você já viu um exemplo disto com funções aninhadas. Você pode escrever um _closure_ sem um nome, envolvendo o código com chaves (`{}`). Utilize `in` para separar os argumentos e o tipo de retorno, do corpo.

```swift
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

>EXPERIMENTO
>
>Reescreva o _closure_ para devolver zero para todos os números ímpares

Você 