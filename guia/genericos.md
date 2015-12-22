## Genéricos

_Código genérico_ permite você escrever código flexível, funções reutilizáveis e tipos que podem trabalhar com com qualquer tipo, sujeito a requisitos que você define. Você pode escrever código que evita duplicação e expresa suas intenções de maneira clara e abstraída.

Genéricos são uma das características mais poderosas em Swift, e muito da biblioteca padrão de Swift é construída com código genérico. De fato, você vem usando genéricos através do _Guia da Linguagem_,  mesmo que nem tenha percebido isso. Por exemplo, os tipos  `Array` e `Dictionary` em Swift são ambos coleções genéricas. Você pode criar um _array_ que mantêm valores `Int` ou um _array_ que mantêm valores `String`, ou até mesmo um _array_ para qualquer tipo que pode ser criado em Swift. Similarmente, você pode criar um dicionário para armazenar valores de qualquer tipo especificado, e não existem limitações sobre qual tipo pode ser.

## O Problema que genéricos resolve

Aqui está uma função padrão, não genérica, chamada `swapTwoInts(_:_:)`, que troca dois valores `Int`:

```swift
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

Esta função faz uso de parametros de entrada-saída para trocar os valores de `a` e `b`, como descrito em [Parametros de entrada-saída](./funcoes.md#inoutparameters).

A função `swapTwoInts(_:_:)` troca o valor original de `b` em `a`, e o valor original de `a` em `b`. Você pode chamar essa função para trocar os valores de duas variáveis `Int`:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt é agora \(someInt), e anotherInt é agora \(anotherInt)")
// prints "someInt é agora 107, e anotherInt é agora 3
```

A função `swapTwoInts(_:_:)` é util, mas ela pode ser usada apenas com valores `Int`. Se você quiser trocar duas variáveis `String` ou duas variáveis `Double`, você teria que escrever mais funções, tais como `swapTwoString(_:_:)` ou `swapTwoDoubles(_:_:)`, como exibido abaixo:

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

Você pode ter notado que o corpo das funções `swapTwoInts(_:_:)`, `swapTwoString(_:_:)` e `swapTwoDoubles(_:_:)` são idênticos. A unica diferença é o tipo dos valores que elas aceitam (`Int`, `String` e `Double`).

Poderia ser muito mais útil, e consideravelmente mais flexível, escrevem uma unica função que poderia troca dois valores de _qualquer_ tipo. Código genérico permite você escrever tais funções. (Uma versão genérica dessas funções é definida abaixo).

> NOTA
>
> Em todas as três funções, é importante que as variáveis `a` e `b` sejam do mesmo tipo. Se `a` e `b` não fossem do mesmo tipo, não seria possível a troca de seuus valores. Swift é uma linguagem de tipo seguro, e não permite (por exemplo) uma variável de tipo `String` e uma variável de tipo `Double` de teres seus valores trocados entre si. Tentar fazer isso seria relatado como um erro em tempo de compilação.

### Funções genéricas

_Funções genéricas_ podem trabalhar com quaisquer tipos. Aqui está uma versão genérica da função `swapToInts(_:_:)` definida acima, chamada `swapTwoValues(_:_:)`:

```swift
func swapTwoValues<T>(inout a: T, inout _ b: T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

O corpo da função `swapTwoValues(_:_:)` é identico ao corpo da função `swapToInts(_:_:)`. Porém, a primeira linha de `swapTwoValues(_:_:)` é levemente diferente da função `swapToInts(_:_:)`. Aqui está a comparação da primeira linha das duas funções:

```swift
func swapTwoInts(inout a: Int, inout _ b: Int)
func swapTwoValues<T>(inout a: T, inout _ b: T)
```

A versão generica da função usa um _nome de tipo reservado_ no local do  (chamado `T`, neste caso), ao invés do _verdadeiro_ nome do tipo (tal como `Int`, `String` ou `Double`). O nome de tipo reservado não diz nada sobre o que `T` deve ser, mas ele diz que ambos `a` e `b` devem ser do mesmo tipo `T`, seja lá o que `T` representa. O tipo verdadeiro a ser usado no lugar de `T` será determinado cada vez que a função `swapTwoValues(_:_:)` é chamada.

A outra diferença é que o nome da função genérica (`swapTwoValues(_:_:)`) é seguida pelo nome de tipo reservado (`T`) dentro dos sinais de maior/menor (`<T>`). Isso diz ao Swift que `T` é um nome de tipo reservado dentro da definição da função `swapTwoValues(_:_:)`. Como `T` é um nome de tipo reservado, Swift não procura por um tipo real chamado `T`.

A função `swapTwoValues(_:_:)` pode agora ser chamada da mesma forma que `swapToInts(_:_:)`, exceto que agora pode ser passado para ela dois valores de quaisquer tipo, contanto que ambos os valores sejam do mesmo tipo entre si. Cada vez que `swapTwoValues(_:_:)` é chamada, o tipo `T` é inferido dos tipos dos valores passados para a função.

Nos dois exemplos abaixo, `T` é inferido como `Int` e `String` respectivamente:

```swift
“var someInt = 3
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
> A função `swapTwoValues(_:_:)` definida acima é inspirada pela função genérica `swap`, que faz parte da biblioteca padrão de Swift, e está automaticamente disponível para você usar em em seus aplicativos. Se você precisa do comportamento de `swapTwoValues(_:_:)` em seu código, use a função `swap(_:_:_)` disponível na biblioteca padrão de Swift, ao invés de usar sua própria implementação.

### Parâmetros de Tipos

No exemplo de `swapTwoValues(_:_:)` acima, o nome de tipo reservado `T` é um exemplo de _parâmetro de tipo_. Um tipo de parâmetro especifica e nomeia um tipo reservado, e é escrito imediatamente depois do nome da função, entre um par de sinais de menor e maior. (assim como `<T>`).

Uma vez que você especifique um parâmetro de tipo, , você pode usa-lo para definir os tipos dos parâmetros de uma função (tais como `a` e `b` na função `swapTwoValues(_:_:)`), ou como o valor devolvido por uma função, ou como anotação do tipo dentro do corpo de uma função. Em cada caso, o pariametro de tipo é substituído com o tipo _verdadeiro_ sempre que a função é chamada. (no exemplo acima de `swapTwoValues(_:_:)`, `T` foi substituído com `Int` na primeira vez que a função foi chamada, e foi substituído por `String` na segunda vez que foi chamado.)

### Nomeando Parâmetros de tipo

Na maioria dos casos, parâmetros de tipo tem nomes descritivos, como `Key` e `Value` em `Dictionary<Key, Value>` e `Element` em `Array<Element>`, o que diz ao leitor sobre o relacionamento entre o parâmetro de tipo e o tipo genérico ou função na qual é usada. Porém, quando não existe um relacionamento significativo entre elas, é tradicional nomea-los usando simples letras como `T`, `U` e `V`, tal como `T` na função `swapTwoValues(_:_:)` acima.

> NOTA
>
> Sempre forneça os parametros de tipo nomes capitalizados sem espaços (assim como `T` e `MyTypeParameter`) para indicar que eles são o nome reservado para um tipo, não um valor


### Tipos genéricos

Em adição as funções, Swift permite também que você defina seus próprios _tipos genéricos_. Estes são classes, estruturas e enumerações personalizadas que podem trabalhar com _qualquer_ tipo, de modo similar à `Array` e `Dictionary`.

Este seção mostra mostra como você pode escrever um tipo de coleção genérica chamada `Stack` (Pilha). A pilha é um conjunto ordenado de valores, similares a um `Array`, mas com um conjunto mais restrito de operações que o tipo `Array` de Swift. Um _Array_ permite novos itens serem inseridos ou removidos de qualquer posição. Uma pilha, porém, permite a inserção apensa no fim da coleção (conhecido como _empurrar_ um novo valor para a pilha). De forma similar, uma pilha só permite remover itens do fim da coleção (conhecido como _estourar_ - do inglês _popping - um valor da pilha).

> NOTA
>
> O conceito de pilha é usado pela classe `UINavigationContorller` para modelar as `view controllers` na sua hierarquia de navegação. Você chama a o método `pushViewController(_:animated:)` da classe `UINavigationController` para adicionar (ou empurrar) uma `view controller` na pilha de navegação, e o método `popViewContorllerAnimated(_:)` para remover uma `view controller` da pilha de navegação. Uma pilha é um modelo de coleção util sempre que você tem uma abordagem "ultimo a entrar, primeiro a sair" rigorosa para gerenciar uma coleção.

