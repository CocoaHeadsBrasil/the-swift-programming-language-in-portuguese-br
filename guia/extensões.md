## Extensões

_Extensões_ adicionam novas funcionalidades a uma classe existente, estrutura, enumeração ou tipo de protocolo. Isso inclui a capacidade de estender tipos pelos quais você não possui acesso ao código fonte original (conhecido como _modelo retroativo_). 
Extensões são similares às categorias em Objetive-C. (Ao contrário das categorias em Objetive-C, extensões em Swift não possuem nomes.) 

Extensões em Swift podem: 

* Adicionar propriedades computadas e tipos computados 
* Definir métodos de instância e métodos de tipo 
* Fornecer novos inicializadores 
* Definir _subscripts_ 
* Definir e usar tipos aninhados novos
* Fazer um tipo existente obedecer a um protocolo 

Em Swift, você ainda pode estender um protocolo para fornecer implementações de seus requisitos ou adicionar funcionalidades para que os tipos que o obedeçam possam tirar vantagem. Para mais detalhes veja [Extensões de Protocolo](guia/protocolos.md#extensoesdeprotocolo) 

> NOTA
>
> Extensões podem adicionar novas funcionalidades para um tipo, mas não podem sobrescrever funcionalidades

### Sintaxe de Extensões 

Declare as extensões com a palavra-chave `extension`. 

```swift 
extension SomeType { 
// novas funcionalidades a serem adicionadas à SomeType entram aqui 
} 
``` 

Uma extensão pode estender uma funcionalidade existente para que ela possa adotar um ou mais protocolos. Quando for este o caso, o nome do protocolo é escrito exatamente da mesma forma que uma classe ou estrutura. 

```swift 
extension SomeType: SomeProtocol, AnotherProtocol { 
// implementações de requisitos do protocolo entram aqui
} 
``` 

Adicionar protocolo de conformidade desta forma está descrito em [Adicionando Protocolo de Conformidade com Extensões](guia/protocolos.md#adicionandoprotocolosdeconformidade). 

> NOTA 
> 
> Se você definir uma extensão para adicionar uma nova funcionalidade para um tipo existente, a nova funcionalidade estará disponível para todas as instâncias daquele tipo, mesmo se elas foram criadas antes da extensão ser definida.

### Propriedades Computadas 

Extensões podem adicionar propriedades de instância computada e propriedades de tipo computado para tipos existentes. Este exemplo adiciona cinco propriedades de instância computadas para o tipo `Double` do Swift, para fornecer suporte básico para trabalhar com unidades de distância: 

```swift 
extension Double { 
    var km: Double { return self * 1_000.0 } 
    var m: Double { return self } 
    var cm: Double { return self / 100.0 } 
    var mm: Double { return self / 1_000.0 } 
    var ft: Double { return self / 3.28084 } 
} 
let oneInch = 25.4.mm 
print("Uma polegada são \(oneInch) metros") 
// imprime "Uma polegada são 0.0254 metros" 
let threeFeet = 3.ft 
print("Três pés são \(threeFeet) metros") 
// imprime "Três pés são 0.914399970739201 metros 
``` 
Estas propriedades computadas expressam que um tipo `Double` deve ser considerado como uma determinada unidade de tamanho. Embora elas sejam implementadas como propriedades computadas, os nomes dessas propriedades podem ser anexadas para um valor literal ponto flutuante com a sintaxe de ponto, como uma maneira de usar aquele valor literal para realizar conversões de distância. 

Neste exemplo, um valor `Double` de `1.0` é considerado para representar "um metro". É por isso que a propriedade computada `m` retorna a auto-expressão, 1.m é considerada para calcular um valor `Double` de `1.0`. 

Outras unidades requerem alguma conversão para serem expressadas como um valor medido em metros. Um quilômetro é o mesmo que 1.000 metros, então a propriedade computada `km` multiplica o valor por `1_000.00` para converter em um número expressado em metros. Da mesma forma, existem `3,28084` pés em um metro, e assim a propriedade computada `ft` divide o valor `Double` subjacente por `3,28084`, para convertê-lo de pés para metros. 

Essas propriedades são propriedades computadas de apenas leitura, e assim elas são expressadas sem a palavra-chave `get`, por questões de abreviatura. Seus valores de retorno são do tipo `Double`, e podem ser usadas dentro de um cálculo matemático onde quer que um `Double` for aceito: 

```swift 
let aMarathon = 42.km + 195.m 
print("Uma maratona tem \(aMarathon) metros de comprimento") 
// imprime "Uma maratona tem 42.1950 metros de comprimento 
``` 

> NOTA 
> 
> Extensões podem adicionar novas propriedades computadas, mas elas não podem adicionar propriedades armazenadas, ou adicionar observadores de propriedade para uma propriedade existente.

###Inicializadores

Extensões podem adicionar novos inicializadores para tipos existentes. Isso possibilita que você estenda outros tipos para que aceitem seus próprios tipos customizados como parâmetros inicializadores, ou fornecer opções adicionais de inicializadores que não foram incluidos como parte da implementação original.

Extensões podem adicionar novos `convenience initializers` para uma classe, mas eles não podem adicionar novos `designated initializers` ou `deinitializers` para uma classe.
`Designated initializers` e `deinitializers` devem sempre ser fornecidos pela implementação original da classe. 

> NOTA 
> 
> Se você usar uma extensão para adicionar um inicializador para um tipo de valor que fornece valores padrão para todas as suas propriedades armazenadas e não define quaisquer inicializadores personalizados, você pode chamar o inicializador padrão e `memberwise initializer` para esse tipo de valor de dentro de sua extensão inicializadora. 
> 
> Esse não seria o caso se você tivesse escrito o inicializador como parte da implementação original do tipo de valor, como está descrito em [Inicializadores de Delegação para Tipos de Valor](guia/initialization#InicializadoresDeDelegacaoParaTiposDeValor).

O exemplo abaixo define uma estrutura customizada `Rect` para representar uma retângulo geométrico. O exemplo também define duas estruturas de apoio chamadas `Size` e `Point`, ambas as quais fornecem os valores padrão de `0.0` para todas as suas propriedades:


```swift 
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}”
```

Porque a estrutura `Rect` fornece valores padrão para todas as suas propriedades, ela recebe um inicializador padrão e um `memberwise initializer` automaticamente, conforme descrito em [Inicializadores Padrão](guia/initialization#InicializadoresPadrao). Estes inicializadores podem ser usados para criar novas instâncias `Rect`:

```swift 
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
    size: Size(width: 5.0, height: 5.0)) 
```

Você pode estender a estrutura `Rect` para fornecer um inicializador adicional que usa um ponto central específico e tamanho:

```swift 
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```
Este novo inicializador começa calculando um ponto de origem adequado com base no ponto central fornecido e valor de tamanho. O inicializador então chama a estrutura de inicialização automática `memberwise initializer` `init(origin:size:)`, que armazena o novo valor de origem e tamanho nas propriedades adequadas:

```swift 
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
    size: Size(width: 3.0, height: 3.0))
// A origem de centerRect é (2.5, 2.5) e seu tamanho é (3.0, 3.0)
```

> NOTA 
> 
> Se você fornecer um novo inicializador com uma extensão, você ainda é responsável por garantir que cada instância é totalmente inicializada assim que o inicializador terminar.


###Métodos

As extensões podem adicionar novos métodos de instância e métodos de tipo para tipos existentes. O exemplo a seguir adiciona um novo método de instância chamado repetições para o tipo `Int`:

```swift 
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
```
O método de repetições`(_:)` leva um único argumento do tipo `() -> Void`, o que indica uma função que não tem parâmetros e não retorna um valor.

Depois de definir esta extensão, você pode chamar o método de repetição`(_:)` em qualquer número inteiro para executar uma tarefa para aquele número de vezes:

```swift 
3.repetitions({
    print("Olá!")
})
// Olá!
// Olá!
// Olá!
```
Use a sintaxe `trailing closure` para fazer a chamada mais sucinta:

```swift 
3.repetitions {
    print("Até logo!")
}
// Até logo!
// Até logo!
// Até logo!
```

###Mutação de Métodos de Instância

Métodos de instância adicionados com uma extensão também podem modificar (ou mutar) a própria instância. Estrutura e métodos de enumeração que modificam `self` ou suas propriedades, devem marcar o método de instância como `mutating `, assim como métodos de mutação de uma implementação original.

O exemplo a seguir adiciona um novo método mutante chamado `square` para o tipo `Int` do Swift, que eleva o valor original ao quadrado:

```swift 
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt agora é 9
```


