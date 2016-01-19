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
let vegetable = "pimenta vermelha"
switch vegetable {
case "aipo":
    print("Adicione umas passas e faça um 'Formiguinhas no Tronco'")
case "pepino", "agrião":
    print("Isso daria um bom sanduíche")
case let x where x.hasPrefix("pimenta"):
    print("É uma \(x) que arde?")
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

Você pode manter um índice em um laço de duas formas: ou usando `..<` para fazer uma série de índices ou escrevendo uma inicialização explícita, condição e incremento. Estes dois laços fazem a mesma coisa:

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

Funções são na verdade um caso especial de _closures_: blocos de código que podem ser chamados mais tarde. O código em um _closure_ tem acesso a coisas como variáveis e funções que estavam disponíveis no escopo onde o _closure_ foi criado, mesmo se o _closure_ estiver num escopo diferente quando for executado - você já viu um exemplo disto com funções aninhadas. Você pode escrever um _closure_ sem um nome, envolvendo o código com chaves (`{}`). Utilize `in` para separar os argumentos e o tipo que a função devolve, do corpo.

```swift
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

>EXPERIMENTO
>
>Reescreva o _closure_ para devolver zero para todos os números ímpares.

Você tem diversas opções parar escrever _closures_ mais consistentes. Quando o tipo do _closure_ já é conhecido, tal como um _callback_ para um _delegate_, você pode omitir o tipo dos seus parâmetros, o tipo que ele devolve, ou ambos. _Closures_ de uma única expressão devolvem implicitamente o valor de sua expressão.

```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```

Você pode referenciar parâmetros por números ao invés de nomes - esta abordagem é especialmente útil em _closures_ muito curtos. Um _closure_ passado como último argumento de uma função pode aparecer imediatamente após os parênteses. Quando um _closures_ é o único argumento de uma função, você pode omitir completamente os parênteses.

```swift
let sortedNumbers = numbers.sort { $0 > $1 }
print(sortedNumbers)
```

### Objetos e classes

Utilize `class` seguido do nome da classe parar criar uma classe. A declaração de uma propriedade em uma classe é escrita da mesma forma que a declaração de uma constante ou variável, exceto que está no contexto da classe. Declarações de métodos e funções são escritos da mesma forma.

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "Um polígono com \(numberOfSides) lados."
    }
}
```

>EXPERIMENTO
>
>Inclua uma constante com `let` e adicione um outro método que recebe um argumento.

Crie uma instância de uma classe colocando parênteses após o nome da classe. Use a sintaxe de pontos para acessar as propriedades e os métodos da instância.

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

Nesta versão da classe `Shape` está faltando algo importante: um inicializador para configurar a classe quando uma instância é criada. Utilize `init` para criar um.


```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func simpleDescription() -> String {
        return "Um polígono com \(numberOfSides) lados."
    }
}
```

Repare como `self` é usado para distinguir a propriedade `name` do argumento `name` passado para o inicializador. Os argumentos do inicializador são passados como uma chamada de função quando você cria uma instância da classe. Toda propriedade precisa de um valor atribuído - ou em sua declaração (como em `numberOfSides`) ou no seu inicializador (como em `name`).

Utilize `deinit` para criar um desconstrutor se você precisar executar alguma limpeza antes do objeto ser desalocado.

Subclasses incluem o nome da sua superclasse após seu próprio nome, separado por um sinal de dois pointos. Não há nenhum requisito que classes estendam alguma classe raiz padrão, portanto você pode incluir ou omitir uma superclasse conforme for preciso.

Métodos em uma subclasse que sobrescrevem a implementação da superclasse são marcados com `override` - sobrescrevendo um método por acidente, sem `override`, é detectado pelo compilador como um erro. O compilador também detecta métodos com `override` que na verdade não sobrescrevem nenhum método da superclasse.

```swift
class Square: NameShape {
    var sideLength: Double
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() -> Double {
        return sideLegth * sideLength
    }
    
    override func simpleDescription() -> String {
        return "Um quadrado com lados de tamanho \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "meu quadrado teste")
test.area()
test.simpleDescription()
```

>EXPERIMENTO
>
>Crie outra subclasse de `NamedShape` chamada `Circle` que recebe um raio e um nome como argumentos do seu inicializador. Implemente os métodos `area()` e `simpleDescription()` na classe `Circle`.

Em adição às propriedades simples que são armazenadas, propriedades podem ter uma função específica que é chamada quando queremos pegar o seu valor e outra quando queremos definir o seu valor, também conhecidos como _getter_ e _setter_.

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }
    
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }
    
    override func simpleDescription() -> String {
        return "Um triângulo equilátero com lados de tamanho \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "um triângulo")
print(triangle.perimeter)
triangle.perimeter = 9.9
print(triangle.sideLength)
```

Na função de definição (_setter_) de `perimeter`, o novo valor tem o nome implícito `newValue`. Você pode fornecer um nome explícito entre parênteses após o `set`.

Repare que o inicializador da classe `EquilateralTriangle` possui três passos diferentes:

1. Definindo o valor das propriedades que a subclasse declara
2. Chamando o inicializador da superclasse
3. Alterando o valor de propriedades definidas pela superclasse. Qualquer trabalho de ajuste adicional que utilize métodos, _getters_ ou _setters_, também podem ser feitos neste ponto.

Se você não precisa computar a propriedade mas ainda precisa fornecer algum código que é executado antes e depois de definir o novo valor, utilize `willSet` e `didSet`. O código que você fornece é executado toda hora que o valor é alterado fora de um inicializador. Por exemplo, a classe abaixo garante que o tamanho do lado de seu triângulo é sempre o mesmo que o tamanho do lado de seu quadrado.

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "outro polígono teste")
print(triangleAndSquare.square.sideLength)
print(triangleAndSquare.triangle.sideLength)
triangleAndSquare.square = Square(sideLength: 50, name: "quadrado maior")
print(triangleAndSquare.triangle.sideLength)
```

Quando se está trabalhando com valores opcionais, você pode escrever `?` antes de operações como métodos, propriedades e _subscripts_. Se o valor antes de `?` for `nil`, tudo após `?` é ignorado e o valor da expressão inteira é `nil`. Caso contrário, o valor opcional é desembrulhado, e tudo que vem após `?` age sobre o valor desembrulhado. Em ambos os casos, o valor da expressão inteira é um valor opcional.

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "quadrado opcional")
let sideLength = optionalSquare?.sideLength
```

### Enumerações e estruturas

Use `enum` para criar uma enumeração. Assim como classes e todos os outros tipos nomeados, enumerações podem ter métodos associados a elas.

```swift
enum Rank: Int {
    case Ace = 1
    case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
    case Jack, Queen, King
    func simpleDescription() -> String {
        switch self {
        case .Ace:
            return "ás"
        case .Jack:
            return "valete"
        case .Queen:
            return "dama"
        case .King:
            return "rei"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.Ace
let aceRawValue = ace.rawValue
```

>EXPERIMENTO
>
>Escreva uma função que compara dois valores de `Rank` através de seus respectivos valores brutos - do inglês _rawValue_.

No exemplo acima, o tipo do valor bruto da enumeração é um `Int`, portanto você somente precisa especificar o primeiro valor bruto. O restante dos valores brutos são atribuídos em ordem. Você também pode usar _strings_ ou números de ponto flutuante como o tipo bruto de uma enumeração. Use a propriedade `rawValue` para acessar o valor bruto de um caso de enumeração.

Utilize o inicializador `init?(rawValue:)` para criar uma instância de uma enumeração a partir de um valor bruto.

```swift
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

Os valores de caso de uma enumeração são valores reais, não somente outra forma de escrever seus valores brutos. De fato, em casos onde não há um valor bruto com um significado real, você não precisa fornecer um.

