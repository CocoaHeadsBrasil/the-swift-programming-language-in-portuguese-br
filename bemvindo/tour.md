## Um Tour pelo Swift

A tradição sugere que o primeiro programa em uma nova linguagem deve imprimir as palavras "Hello, world!" na tela. Em Swift, isso pode ser feito em uma única linha:

```swift
print("Hello, world!")
```

Se você já escreveu código em C ou em Objective-C, essa sintaxe lhe é  familiar — em Swift, essa linha de código é um programa completo. Você não precisa importar uma biblioteca separada para funcionalidades como entrada/saída ou manipulação de strings. O código escrito num escopo global é usado como ponto de entrada para o programa, portanto você não precisa de uma função `main()`. Você também não precisa colocar ponto e vírgula no final de cada instrução de código.

Este tour lhe dá informação suficiente para começar a escrever código em Swift, mostrando como realizar diferentes tipos de tarefas de programação. Não se preocupe caso não entenda alguma coisa — tudo que for apresentado nesse tour será explicado em detalhes no restante deste livro.

>NOTA: Em um Mac, baixe o Playgroud e dê um duplo clique no arquivo para abrí-lo no Xcode: (https://developer.apple.com/go/?id=swift-tour)[https://developer.apple.com/go/?id=swift-tour]

### Valores simples

Use `let` para criar uma constante e `var` para criar uma variável. O valor de uma constante não precisa ser conhecido em tempo de compilação, mas você deve atribuí-lo exatamente uma única vez. Isso significa que você pode usar constantes para nomear um valor que você determina uma vez mas que usa em vários lugares.

```swift
var myVariable = 42
myVariable = 50
let myConstante = 42
```

Uma constante ou uma variável deve ter o mesmo tipo que o valor que você queira atribuir a ela. Contudo, você não precisa escrever o tipo explicitamente. Fornecer um valor quando você cria uma constante ou variável faz com que o compilador deduza o seu tipo. No exemplo acima, o compilador deduz que `myVariable` é um inteiro porque seu valor inicial é um inteiro.

