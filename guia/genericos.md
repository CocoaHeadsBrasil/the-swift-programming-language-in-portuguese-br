## Genéricos
<<<<<<< HEAD

_Código genérico_ permite você escrever código flexível, funções reutilizáveis e tipos que podem trabalhar com qualquer tipo, sujeito a requisitos que você define. Você pode escrever código que evita duplicação e expressa suas intenções de maneira clara e abstraída.

Genéricos são uma das características mais poderosas em Swift, e muito da biblioteca padrão de Swift é construída com código genérico. De fato, você vem usando genéricos através do _Guia da Linguagem_,  mesmo que nem tenha percebido isso. Por exemplo, os tipos `Array` e `Dictionary` em Swift são ambos coleções genéricas. Você pode criar um _array_ que mantêm valores `Int` ou um _array_ que mantêm valores `String`, ou até mesmo um _array_ para qualquer tipo que possa ser criado em Swift. Similarmente, você pode criar um dicionário para armazenar valores de qualquer tipo especificado, e não existem limitações sobre qual tipo pode ser.

## O Problema que genéricos resolve

Aqui está uma função padrão, não genérica, chamada `swapTwoInts(_:_:)`, que troca dois valores `Int`:

```swift
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

Esta função faz uso de parâmetros de entrada-saída para trocar os valores de `a` e `b`, como descrito em [Parâmetros de entrada-saída](./funcoes.md#inoutparameters).

A função `swapTwoInts(_:_:)` troca o valor original de `b` em `a`, e o valor original de `a` em `b`. Você pode chamar essa função para trocar os valores de duas variáveis `Int`:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt é agora \(someInt), e anotherInt é agora \(anotherInt)")
// prints "someInt é agora 107, e anotherInt é agora 3
```

A função `swapTwoInts(_:_:)` é útil, mas ela pode ser usada apenas com valores `Int`. Se você quiser trocar duas variáveis `String` ou duas variáveis `Double`, você teria que escrever mais funções, tais como `swapTwoString(_:_:)` ou `swapTwoDoubles(_:_:)`, como exibido abaixo:

```swift
func swapTwoStrings(inout a: String, inout _ b: String) {
    let temporaryA = a
    a = b
    b = temporaryA
}
 
func swapTwoDoubles(inout a: Double, inout _ b: Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

Você pode ter notado que o corpo das funções `swapTwoInts(_:_:)`, `swapTwoString(_:_:)` e `swapTwoDoubles(_:_:)` são idênticos. A única diferença é o tipo dos valores que elas aceitam (`Int`, `String` e `Double`).

Poderia ser muito mais útil, e consideravelmente mais flexível, escrevem uma única função que poderia troca dois valores de _qualquer_ tipo. Código genérico permite você escrever tais funções. (Uma versão genérica dessas funções é definida abaixo).

> NOTA
>
> Em todas as três funções, é importante que as variáveis `a` e `b` sejam do mesmo tipo. Se `a` e `b` não fossem do mesmo tipo, não seria possível a troca de seus valores. Swift é uma linguagem de tipo seguro, e não permite (por exemplo) uma variável de tipo `String` e uma variável de tipo `Double` de teres seus valores trocados entre si. Tentar fazer isso seria relatado como um erro em tempo de compilação.

### Funções genéricas

_Funções genéricas_ podem trabalhar com quaisquer tipos. Aqui está uma versão genérica da função `swapToInts(_:_:)` definida acima, chamada `swapTwoValues(_:_:)`:

```swift
func swapTwoValues<T>(inout a: T, inout _ b: T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

O corpo da função `swapTwoValues(_:_:)` é idêntico ao corpo da função `swapToInts(_:_:)`. Porém, a primeira linha de `swapTwoValues(_:_:)` é levemente diferente da função `swapToInts(_:_:)`. Aqui está a comparação da primeira linha das duas funções:

```swift
func swapTwoInts(inout a: Int, inout _ b: Int)
func swapTwoValues<T>(inout a: T, inout _ b: T)
```

A versão genérica da função usa um _nome de tipo reservado_ no local do  (chamado `T`, neste caso), ao invés do _verdadeiro_ nome do tipo (tal como `Int`, `String` ou `Double`). O nome de tipo reservado não diz nada sobre o que `T` deve ser, mas ele diz que ambos `a` e `b` devem ser do mesmo tipo `T`, seja lá o que `T` representa. O tipo verdadeiro a ser usado no lugar de `T` será determinado cada vez que a função `swapTwoValues(_:_:)` é chamada.

A outra diferença é que o nome da função genérica (`swapTwoValues(_:_:)`) é seguido pelo nome de tipo reservado (`T`) dentro dos sinais de maior/menor (`<T>`). Isso diz ao Swift que `T` é um nome de tipo reservado dentro da definição da função `swapTwoValues(_:_:)`. Como `T` é um nome de tipo reservado, Swift não procura por um tipo real chamado `T`.

A função `swapTwoValues(_:_:)` pode agora ser chamada da mesma forma que `swapToInts(_:_:)`, exceto que agora pode ser passado para ela dois valores de quaisquer tipo, contanto que ambos os valores sejam do mesmo tipo entre si. Cada vez que `swapTwoValues(_:_:)` é chamada, o tipo `T` é inferido dos tipos dos valores passados para a função.

Nos dois exemplos abaixo, `T` é inferido como `Int` e `String` respectivamente:

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt vale agora 107, e anotherInt vale agora 3
 
var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString vale agora "world", e anotherString vale agora "hello”
```

> NOTA
>
> A função `swapTwoValues(_:_:)` definida acima é inspirada pela função genérica `swap`, que faz parte da biblioteca padrão de Swift, e está automaticamente disponível para você usar em seus aplicativos. Se você precisa do comportamento de `swapTwoValues(_:_:)` em seu código, use a função `swap(_:_:_)` disponível na biblioteca padrão de Swift, ao invés de usar sua própria implementação.

### Tipos de Parâmetros

No exemplo de `swapTwoValues(_:_:)` acima, o nome de tipo reservado `T` é um exemplo de _parâmetro de tipo_. Um parâmetro de tipo especifica e nomeia um tipo reservado, e é escrito imediatamente depois do nome da função, entre um par de sinais de menor e maior. (assim como `<T>`).

Uma vez que você especifique um parâmetro de tipo, você pode usa-lo para definir os tipos dos parâmetros de uma função (tais como `a` e `b` na função `swapTwoValues(_:_:)`), ou como o valor devolvido por uma função, ou como anotação do tipo dentro do corpo de uma função. Em cada caso, o parâmetro de tipo é substituído com o tipo _verdadeiro_ sempre que a função é chamada. (No exemplo acima de `swapTwoValues(_:_:)`, `T` foi substituído com `Int` na primeira vez que a função foi chamada, e foi substituído por `String` na segunda vez que foi chamado.)

### Nomeando Tipos de Parâmetro

Na maioria dos casos, parâmetros de tipo tem nomes descritivos, como `Key` e `Value` em `Dictionary<Key, Value>` e `Element` em `Array<Element>`, o que diz ao leitor sobre o relacionamento entre o parâmetro de tipo e o tipo genérico ou função na qual é usada. Porém, quando não existe um relacionamento significativo entre elas, é tradicional nomeá-los usando simples letras como `T`, `U` e `V`, tal como `T` na função `swapTwoValues(_:_:)` acima.

> NOTA
>
> Sempre forneça os parâmetros de tipo nomes capitalizados sem espaços (assim como `T` e `MyTypeParameter`) para indicar que eles são o nome reservado para um tipo, não um valor


### Tipos genéricos

Em adição às funções, Swift permite também que você defina seus próprios _tipos genéricos_. Estes são classes, estruturas e enumerações personalizadas que podem trabalhar com _qualquer_ tipo, de modo similar a `Array` e `Dictionary`.

Esta seção mostra como você pode escrever um tipo de coleção genérica chamada `Stack` (pilha). A pilha é um conjunto ordenado de valores, similares a um `Array`, mas com um conjunto mais restrito de operações que o tipo `Array` de Swift. Um _Array_ permite novos itens serem inseridos ou removidos de qualquer posição. Uma pilha, porém, permite a inserção apensa no fim da coleção (conhecido como _empurrar_ (do inglês _push_) um novo valor para a pilha). De forma similar, uma pilha só permite remover itens do fim da coleção (conhecido como _estourar_ - do inglês _popping_ - um valor da pilha).

> NOTA
>
> O conceito de pilha é usado pela classe `UINavigationController` para modelar as `view controllers` na sua hierarquia de navegação. Você chama o método `pushViewController(_:animated:)` da classe `UINavigationController` para adicionar (ou empurrar) uma `view controller` na pilha de navegação, e o método `popViewContorllerAnimated(_:)` para remover uma `view controller` da pilha de navegação. Uma pilha é um modelo de coleção útil sempre que você tem uma abordagem "ultimo a entrar, primeiro a sair" rigorosa para gerenciar uma coleção.

A ilustração abaixo mostra o comportamento de empurrar/estourar de uma pilha.

![](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushPop_2x.png)


1. Existem atualmente três valores na pilha.
2. Um quarto valor é "empurrado" no topo da pilha.
3. A pilha agora armazena quatro valores, com o mais recente no topo.
4. O item no topo da pilha é removido, ou "estourado".
5. Depois de estourar um valor, a pilha novamente armazena três valores.

```swift
struct IntStack {
    var items = [Int]()
    mutating func push(item: Int) {
        items.append(item)
    }
    
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

Esta estrutura usa uma propriedade `Array` chamada `items` para armazenar os valores da pilha. `Stack` provê dois métodos, `push` e `pop`, para empurrar e estourar valores para e da pilha. Estes métodos são marcados como `mutating`, porque eles precisam modificar (ou _mutar_) a propriedade `items` da estrutura.

O tipo `IntStack` mostrado acima pode ser usado apenas com valores `Int`. Seria muito mais útil definir uma classe `Stack` genérica, que pode gerenciar uma pilha de _qualquer_ tipo.

Aqui está a versão genérica do mesmo código:

```swift
struck Stack<Element> {
	var items = [Element]()
	mutating func push(item: Element) {
		items.append(item)
	}
	
	mutating func pop() -> Element {
	return items.removeLast()
	}
}
```

Note como a versão genérica de `Stack` é essencialmente a mesma que a versão não genérica, mas com o parâmetro de tipo chamado `Element` ao invés do tipo verdadeiro `Int`. Este parâmetro de tipo é escrito dentro de um  par de sinais de maior/menor (`<Element>`) imediatamente depois do nome da estrutura.

`Element` define um nome de tipo reservado para "algum tipo `Element`" que será provido posteriormente. Este tipo futuro pode ser referenciado como `Element` em qualquer parte da definição da estrutura. Neste caso, `Element` é usado como um tipo reservado em três lugares:

* Para criar uma propriedade chamada `items`, que é inicializada com um conjunto vazio de valores do tipo `Element`
* Para especificar que o método `push(_:)` tem um único parâmetro chamado `item`, que deve ser do tipo `Element`
* Para especificar que o valor retornado pelo método `pop()` será um valor do tipo `Element`

Como este é um tipo genérico, `Stack` pode ser usado para criar uma pilha de _qualquer_ tipo válido em Swift, de maneira similar aos tipos `Array` e `Dictionary`.

Você pode criar uma nova instância de `Stack` ao escrever o tipo a ser armazenado pela pilha dentro de sinais de maior/menor. Por exemplo, para criar uma pilha de `String`, você pode escrever `Stack<String>()`:

```swift
var stackOfStrigns = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// a pilha contêm agora 4 variáveis String
```

Segue como `stackOfStrings` fica após empurrar esses quatro valores na pilha:


![](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushedFourStrings_2x.png)

Estourar um valor da pilha devolve e remove o valor do topo, no caso, _"cuatro"_:

```swift
let fromTheTop = stackOfStrings.pop
//fromTheTop é igual a "cuatro", e a pilha agora contém 3 valores String
```

Aqui está como a pilha fica após estourar o valor do topo:


![](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushPop_2x.png)

### Estendendo um tipo genérico

Quando você estende um tipo genérico, você não provê uma lista de parâmetros de tipo como parte da definição da extensão. Ao invés disso, a lista de parâmetros de tipo da definição _original_ do tipo está disponível dentro do corpo da extensão, e os nomes dos parâmetros de tipo do tipo original são usados para referenciar os parâmetros da definição original.

O exemplo seguinte estende a pilha genérica `Stack` para adicionar uma propriedade computada apenas para leitura chamada `topItem`, que devolve o item do topo, sem que seja necessário estourar o valor da pilha:

```swift
extension Stack P
	var topItem: Element? {
		return items.isEmpty ? nil : 
			items[items.count - 1]
	}
}
```

A propriedade `topItem` devolve um valor opcional do tipo `Element`, Se a pilha está vazia, `topItem` devolve `nil`; se a pilha não está vazia, `topItem` devolve o item final da propriedade `items`.

Note que esta extensão não define uma lista de parâmetros de tipo. Ao invés disso, `Element`, o nome original do parâmetro de tipo existente no tipo `Stack`, é usado dentro da extensão para indicar o tipo da propriedade computada `topItem`.

A propriedade computada `topItem` pode agora ser usada com qualquer instância de `Stack` para acessar e pesquisar seu item do topo sem removê-lo:

```swift
if let topItem = stackOfStrings.topItem {
	print("O elemento no topo da pilha é \(topItem).")
}
```

### <a name="TypeConstraints">Restrições de Tipo

A função `swapTwoValues(_:_:)` e o tipo `Stack` podem trabalhar com qualquer tipo. Apesar disso, algumas vezes é útil obrigar algumas _restrições de tipo_ em tipos que podem ser usados com funções genéricas e tipos genéricos. Restrições de tipo especificam que um parâmetro de tipo deve herdar de uma classe específica, ou obedecer à um protocolo específico ou uma composição de protocolos.

Por exemplo, o tipo `Dictionary` em Swift coloca uma limitação nos tipos que podem ser usados como chaves do dicionário. Como descrito em [Dicionários](./tipos_colecoes.md#dictionaries), o tipo da chave de um dicionário deve ser <!--TODO: Hashable?-->obedecer ao protocolo `Hashable`, isto é, deve prover um método para se fazer unicamente representável. Dicionários precisam que suas chaves sejam unicamente representáveis  para que possam verificar se ela já contém um valor para uma determinada chave. Sem esta exigência, `Dictionary` não poderia dizer se precisa inserir ou atualizar o valor para uma determinada chave, nem poderia localizar o valor para uma determinada uma chave que já está no dicionário.

Este requisito é forçado por uma restrição de tipo no tipo da chave para o `Dictionary`, que especifica que a chave deve obedecer ao protocolo `Hashable`, um protocolo especial definido na biblioteca padrão de Swift. Todos os tipos básicos de Swift (tais como `String`, `Int`, `Double` e `Bool`) seguem este protocolo por padrão.

Você pode definir suas próprias restrições de tipo quando criando tipos genéricos personalizados, e estas restrições proveem muito mais poder para a programação genérica. Conceitos abstratos como `Hashable` caracterizam tipos em termos de suas características conceituais, ao invés de seu tipo explícito.

#### Sintaxe de Restrição de Tipo

Você escreve restrições de tipo ao colocar uma única restrição de classe ou protocolo depois do nome do parâmetro de tipo, separado pelo sinal de dois pontos, como parte da lista de parâmetros de tipo. A sintaxe básica para as restrições de tipo em uma função genérica é mostrada abaixo (apesar da sintaxe ser a mesma para tipos genéricos):

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    //corpo da função
}
```

A função hipotética acima tem dois parâmetros de tipo. O primeiro, `T`, tem a restrição de tipo que requer `T` ser subclasse de `SomeClass`. O segundo parâmetro de tipo, `U`, tem a restrição que requer que `U` obedeça ao protocolo `SomeProtocol`.

### Restrições de Tipo em Ação

Aqui está uma função não-genérica chamada `findStringIndex`, para a qual é dado um valor `String` para encontrar e um `Array` de valores `String` no qual desejamos procurar. A função `findStringIndex(_:_:)` devolve um valor opcional `Int`, que será o índice da primeira ocorrência da `String` buscada dentro do `Array`, ou `nil` caso não encontrado:

```swift
func findStringIndex(array: [String], _ valueToFind: String) -> Int? {
	for (índex, value) in array.enumerate() {
		if value == valueToFind {
			return index
		}
	}
	return nil
}
```

A função `findStringIndex(_:_:)`pode ser usada para localizar um valor `String` em uma matriz de `String`:

```swift
let strings = ["gato", "cachorro", "lhama", "periquito", "tartaruga"]

if let foundIndex = findStringIndex(strings, "lhama") {
	print("O índice da lhama é \(foundIndex)")
}
// imprime "O índice da lhama é 2"
```

Apesar disso, o princípio de encontrar o índice de um valor em um `Array` não é útil apenas para _strings_. Você pode escrever a mesma funcionalidade com uma função genérica chamada `findIndex`, ao substituir qualquer menção à _strings_ com valores de algum tipo `T`.

Segue como você pode esperar que a versão genérica de `findStringIndex`, chamada `findString`, seja escrita. Note que o tipo devolvido nesta função continua sendo `Int?`, porque a função devolve um índice numérico opcional, não um valor opcional do _array_. Esteja avisado, entretanto - esta função não compila por motivos que serão explicados depois do exemplo:

```swift
func findIndex<T>(array: [T], _ valueToFind: T) -> Int? {
	for (index, value) in array.enumerate() {
		if value == valueToFind {
			return index
		}
	}
	
	return nil
}
```

Da forma escrita acima, esta função não compila. O problema está na verificação de qualidade, "`if value == valueToFind`". Nem todo tipo em Swift pode ser comparado com o operador de igual (`==`). Se você criar sua própria classe ou estrutura para representar um modelo de dados complexo, por exemplo, então o significado de "igual a" para esta classe ou estrutura é algo que Swift não pode adivinhar por você. Por causa disso, não é possível garantir que este código vá funcionar para _todo_ tipo `T` possível. e um erro apropriado é relatado quando você tenta compilar este código.

Nem tudo está perdido, entretanto. A biblioteca padrão de Swift define um protocolo chamado `Equatable`, que requer que qualquer tipo obedecendo implemente o operador igual à (`==`) e o operador diferente (`!=`) para comparar quaisquer dois valores deste tipo. Todos os tipos da biblioteca padrão automaticamente adotam o protocolo `Equatable`.

Qualquer tipo que obedeça ao protocolo `Equatable` pode ser usado com segurança com a função `findIndex(_:_:)`, porque o suporte ao operador `==` é garantido. Para expressar esse fato, você escreve uma restrição de `Equatable` como parte da definição do tipo do parâmetro quando você define a função:

```swift
func findIndex<T: Equatable>(array: [T], _ valueToFind: T) -> Int? {
	for (index, value) in array.enumerate() {
		if value == valueToFind {
			return index
		}
	}
	
	return nil
}
```

O único parâmetro de tipo para `findIndex` é escrito como `T: Equatable`, que significa "qualquer tipo `T` que obedeça ao protocolo `Equatable`".

A função `findIndex(_:_:)` agora compila com sucesso e pode ser usada com qualquer tipo `Equatable`, tal como `Double` ou `String`.

```swift
let doubleIndex = findIndex([3.14159, 0.1, 0.25], 9.3)
// doubleIndex é um Int opcional com nenhum valor, já que 9.3 não está no array

let stringIndex = findIndex(["Mike", "Malcolm", "Andrea"], "Andrea")
// stringIndex é um Int opcional contendo valor 2
```

### Tipos Associados

Quando definido um protocolo, às vezes é útil declarar um ou mais _tipos associados_ como parte da definição do protocolo. Um tipo associado dá um nome reservado (ou _pseudônimo_) para um tipo que será usado como parte do protocolo. O tipo verdadeiro a ser usado para aquele nome de tipo reservado não é especificado até o protocolo ser adotado. Tipos associados são especificados com a palavra reservada `typealias`.

#### Tipos Associados em Ação

Aqui está um exemplo de protocolo chamado `Container`, que declara um tipo associado chamado `ItemType`:


```swift
protocol Container {
	typealias ItemType
	mutating func append(item: ItemType)
	var count: Int { get }
	subscript(i: Int) -> ItemType { get }
}
```

O protocolo `Container` define três capacidades necessárias que qualquer contêiner deve fornecer:

* Deve ser possível adicionar um novo item ao contêiner com o método `append(_:)`.
* Deve ser possível acessar a contagem de itens do contêiner através de uma propriedade `count` que devolve um valor inteiro.
* Deve ser possível obter cada item dentro do contêiner com um valor de índice do tipo `Int`.

Este protocolo não especifica como os itens dentro do contêiner devem ser armazenados ou que tipo eles devem ser. O protocolo apenas específica os três pedaços de funcionalidade que qualquer tipo deve prover para ser considerado um `Container`. Um tipo obedecendo pode fornecer funcionalidades adicionais, contanto que satisfaça esses três requisitos.

Qualquer tipo que obedeça ao protocolo `Container` deve ser apto a especificar o tipo de valor que armazena. Especificamente, ele deve assegurar que apenas items do tipo certo são adicionados ao contêiner, e deve ser claro sobre os tipos de items devolvidos por seu índice.

Para definir estes requisitos, o protocolo `Container` precisa de uma forma de referenciar o tipo de elemento que o contêiner irá armazenar, sem saber que tipo ele é para um contêiner específico. O protocolo `Container` precisa especificar que qualquer valor passado para o método `append(_:)` deve ter o mesmo tipo que o tipo de elemento do contêiner, e que o valor devolvido pelo índice de contêiner será do mesmo tipo que o tipo de elemento do contêiner.

Para alcançar este objetivo, o protocolo `Container` declara um tipo associado chamado `ItemType`, escrito na forma `typealias ItemType`. O protocolo não define do que `ItemType` é um pseudônimo - esta informação é deixada para ser fornecida por qualquer tipo adotando o protocolo. No entanto, o pseudônimo `ItemType` provê uma forma de referenciar o tipo de itens de um `Container`, e definir um tipo para ser usado com o método `append(_:)` e com o índice, para assegurar que o comportamento esperado de qualquer `Container` é garantido.

Aqui está uma versão do tipo não genérico `IntStack` anterior, adaptado para obedecer ao protocolo `Container`:

```swift
struct IntStack: Container {
    // implementação original de IntStack
    var items = [Int]()
    mutating func push(item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformidade com o protocolo Container
    typealias ItemType = Int
    mutating func append(item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

O tipo `IntStack` implementa todos os três requisitos do protocolo `Container`, e em cada caso envolve parte da funcionalidade existente do tipo `IntStack` para satisfazer estes requisitos.

Além disso, `IntStack` especifica que para esta implementação de `Container`, o `ItemType` apropriado a ser usado é um tipo `Int`. A definição `typealias ItemType = Int` torna o tipo de dados abstrato de `ItemType` no tipo concreto `Int` para esta implementação do protocolo `Container`.

Graças à inferência de tipos de `Swift`, você não precisa necessariamente declarar um tipo concreto `Int` de `ItemType` como parte da definição de `IntStack`. Como `IntStack` obedece a todos os requisitos do protocolo `Container`, Swift pode inferir o `IntType` apropriado a ser usado, simplesmente olhando para o tipo do parâmetro `item` do método `append(_:)` e o tipo devolvido pelo subscrito. De fato, se você remover a sentença `typealias ItemType = Int` no código acima, tudo ainda vai funcionar, porque é claro qual tipo deveria ser usado para `ItemType`.

Você também pode criar um `Stack` genérico obedecendo ao protocolo:

```swift
struct Stack<Element>: Container {
    // implementação original de Stack<Element>
    var items = [Element]()
    mutating func push(item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // conformidade ao protocolo Container
    mutating func append(item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

Desta vez, o parâmetro de tipo `Element` é usado como tipo para o parâmetro `item` do método `append(_:)` e como o tipo devolvido pelo subscrito. Swift pode a partir disso inferir qual `Element` apropriado deve ser usado como `ItemType` em um contêiner apropriado.

#### Estendendo o Tipo Existente para Especificar um Tipo Associado

Você pode estender um tipo para adicionar conformidade para um protocolo, como descrito em [Adicionando conformidade para um protocolo com uma Extensão](./protocolos.md#ProtocolConformanceWithExtension). Isto inclui um protocolo com um tipo associado.

O tipo `Array` de Swift já provê um método `append(_:)`, uma propriedade `count`, e um subscrito com um índice `Int` para recuperar estes elementos. Estas três capacidades coincidem com os requisitos do protocolo `Container`. isto significa que você pode estender `Array` para obedecer ao protocolo `Container` simplesmente declarando que `Array` adota o protocolo. Você pode fazer isso com uma extensão vazia, como descrito em [Declarando Adição à um Protocolo com uma extensão](./protocolos.md#DeclaringProtocolAdoptionWithExtension):

```swift
extension Array: Container {}
```

O método `append(_:)` existente em `Array` e o subscrito permitem ao Swift inferir o tipo apropriado para usar com `ItemType`, da mesma forma que com o tipo `Stack` genérico acima. Depois de definir esta extensão, você pode usar qualquer `Array` como um `Container`.

### _Cláusulas Onde_

Restrições de tipo, como definidas em [Restrições de tipo](./genericos.md#TypeConstraints), permitem a você definir requisitos nos parâmetros de tipos com a função ou tipo genérico.

Ele também pode ser útil para definir requisitos para tipos associados. Você faz isso definindo _Cláusulas Onde_ - do inglês _where clauses_ - como parte de uma lista de parâmetro de tipo. Uma cláusula onde permite você requerer que um tipo associado deve obedecer a um certo protocolo, ou que um certo parâmetro de tipo e tipos associados devem ser os mesmos. Você escreve uma cláusula onde ao colocar a palavra chave `where` imediatamente após a lista de parâmetros de tipo, seguido pelas restrições para is tipos associados ou equalidade entre tipos e tipos associados.

O exemplo abaixo define uma função genérica chamada `allItemsMatch`, que verifica se cada duas instâncias de `Container` contém os mesmos itens na mesma ordem. A função devolve um valor binário de `true` caso todos os itens coincidam ou `false` caso contrário.

Os dois contêineres não precisam ser do mesmo tipo (mesmo que possam ser), mas eles devem armazenar os mesmos tipos de itens. Este requisito é expresso atraves de uma combinação de restrições de tipos e cláusulas onde:

```swift
func allItemsMatch<
    C1: Container, C2: Container
    where C1.ItemType == C2.ItemType, C1.ItemType: Equatable>
    (someContainer: C1, _ anotherContainer: C2) -> Bool {
        
        // verifica que ambos os contêineres armazenam a mesma quantidade de itens
        if someContainer.count != anotherContainer.count {
            return false
        }
        
        // verifica cada par validando se eles são equivalentes
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }
        
        // todos os itens coincidem, devolva true
        return true
        
}
```

Esta função recebe dois argumentos chamados `someContainer` e `anotherContainer`. O argumento `someContainer` é do tipo `C1`, e o argumento `anotherContainer` é do tipo `C2`. Ambos `C1` e `C2` são parâmetros de tipo para dois tipos de contêineres a serem determinados quando a função é chamada.

A lista de parâmetros de tipos coloca as seguintes restrições nos dois parâmetros de tipo:

* `C1` deve obedecer ao protocolo `Container` (escrito da forma `C1: Container`).
* `C2` deve obedecer ao protocolo `Container` (escrito da forma `C2: Container`).
* O `ItemType` de `C1` deve ser o mesmo que o `ItemType` de `C2` (escrito da forma `C1.ItemType == C2.ItemType`).
* O `ItemType` de `C1` deve obedecer ao protocolo `Equatable` (escrito da forma `C1.ItemType: Equatable`).

Os terceiro e quarto requisitos são definidos como parte de uma cláusula onde, e são escritos após a palavra chave `where` como parte da lista de parâmetros de tipo.

Estes requisitos significam:

* `comeContainer` é um contêiner do tipo `C1`
* `anotherContainer` é um contêiner do tipo `C2`
* `someContainer` e `anotherContainer` armazenam os mesmos tipos de elementos.
* Os items de `someContainer` podem ser verificados com o operador não igual (`!=`) para verificar se ão diferentes entre si.

Os terceiro e quarto requisitos combinam para significar que os itens em `anotherContainer` também podem ser verificados com o operador `!=`, porque eles são exatamente do mesmo tipo que os itens em `someContainer`.

Estes requisitos permitem a função `allItemsMatch(_:_:)` comparar dois contêineres, mesmo que sejam tipos diferentes de contêiner.

A função `allItemsMatch(_:_:)` começa verificando que ambos os contêineres armazenam a mesma quantidade de itens. Se a quantidade é diferente, não existe nenhuma forma deles serem os mesmos, então a função devolve `false`.

Depois dessa verificação, a função itera sobre todos os itens em `someContainer` com um laço `for-in` e o operador de intervalo semi-aberto (`..<`). Para cada items, a função verifica se o item de `someContainer` não igual é ao item correspondente em `anotherContainer`. Se os dois itens não são iguais, então os contêineres não coincidem, e a função devolve `false`.

Se o laço completa sem encontrar um descasamento, os dois contêineres coincidem, e a função devolve `true`.

Aqui está como `allItemsMatch(_:_:)` se parece em ação:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
 
var arrayOfStrings = ["uno", "dos", "tres"]
 
if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("Todos os itens coincidem.")
} else {
    print("Nem todos os itens coincidem.")
}
// imprime "Todos os itens coincidem."
```

O exemplo acima cria uma instância de `Stack` para armazenar valores `String`, e empurra três valores `String` na pilha. O exemplo também cria uma instância de `Array` inicializada com um _array_ literal contendo os mesmos três valores `String` da pilha. Ainda que a pilha e o _array_ sejam de tipos diferentes, ambos adotam o protocolo `Container`, e ambos contêm o mesmo tipo de valores. Você pode portanto chamar a função `allItemsMatch(_:_:)` com esses dois contêineres como argumentos, No exemplo acima, a função corretamente relata que todos os itens dos dois contêineres coincidem.

=======
>>>>>>> master
