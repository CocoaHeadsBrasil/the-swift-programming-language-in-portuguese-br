## Um Tour pelo Swift

A tradição sugere que o primeiro programa em uma nova linguagem deve imprimir as palavras "Hello, world!" na tela. Em Swift, isso pode ser feito em uma única linha:

```swift
print("Hello, world!")
```

Se você já escreveu código em C ou em Objective-C, essa sintaxe lhe é  familiar — em Swift, essa linha de código é um programa completo. Você não precisa importar uma biblioteca separada para funcionalidades como entrada/saída ou manipulação de strings. O código escrito num escopo global é usado como ponto de entrada para o programa, portanto você não precisa de uma função `main()`. Você também não precisa colocar ponto e vírgula no final de cada instrução de código.

Este tour lhe dá informação suficiente para começar a escrever código em Swift, mostrando como realizar diferentes tipos de tarefas de programação. Não se preocupe caso não entenda alguma coisa — tudo que for apresentado nesse tour será explicado em detalhes no restante deste livro.

>**NOTA**
>
>Em um Mac, baixe este Playground e dê um duplo clique no arquivo para abrí-lo no Xcode: <a href="https://developer.apple.com/go/?id=swift-tour">https://developer.apple.com/go/?id=swift-tour</a>

### Valores simples

Use `let` para criar uma constante e `var` para criar uma variável. O valor de uma constante não precisa ser conhecido em tempo de compilação, mas você deve atribuí-lo exatamente uma única vez. Isso significa que você pode usar constantes para nomear um valor que você determina uma vez mas que usa em vários lugares.

```swift
var myVariable = 42
myVariable = 50
let myConstante = 42
```

Uma constante ou uma variável deve ter o mesmo tipo que o valor que você queira atribuir a ela. Contudo, você não precisa escrever o tipo explicitamente. Fornecer um valor quando você cria uma constante ou variável faz com que o compilador deduza o seu tipo. No exemplo acima, o compilador deduz que `myVariable` é um inteiro porque seu valor inicial é um inteiro.

Se o valor inicial não fornecer informação suficiente (ou se não existir um valor inicial), especifique um tipo escrevendo logo após a variável, separado pelo sinal de dois pontos.

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

>Experimento: Crie uma constante com tipo explícito `Float` e valor `4`.

Valores nunca são implicitamente convertidos para outro tipo. Se você precisar converter um valor para um tipo diferente, crie uma instância do tipo desejado explicitamente.

```swift
let label = "A largura é "
let width = 94
let widthLabel = label + String(width)
```

>Experimento: Tente remover a conversão para `String` na última linha. Que erro você recebe?

Existe uma forma ainda mais simples de incluir valores em strings: Escreva o valor entre parênteses e adicione uma barra invertida antes dos parênteses. Por exemplo:

```swift
let apples = 3
let oranges = 5
let appleSummary = "Eu tenho \(apples) maçãs."
let fruitSummary = "Eu tenho \(apples + oranges) frutas."
```

>Experimento: