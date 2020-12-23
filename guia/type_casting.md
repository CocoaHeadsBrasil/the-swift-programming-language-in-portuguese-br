##Moldagem de Tipo

Moldagem de tipo - do original *type casting* - é uma forma de verificar o tipo de uma instância, ou tratá-la como uma superclasse ou subclasse diferente que pertença a sua própria hierarquia.

Moldagem de tipo em Swift é implementada com os operadores `is`e `as`. Esses dois operadores fornecem uma forma simples e expressiva de verificar o tipo de um valor ou moldá-lo para um tipo diferente.

Você também pode usar moldagem de tipo para verificar se um tipo está em conformidade com um protocolo, como descrito em [Verificando por conformidades em protocolos](link)

### Definindo uma Classe Hierarquica para Moldagem de Tipo

Você pode usar a moldagem de tipo com um hierarquia de classes ou subclasses para verificar o tipo de uma instância de uma classe em particular e modá-la para uma outra classe dentro da mesma hierarquia. Esses três trechos de código abaixo definem uma hierarquia de classes e uma lista contendo instâncias dessas classes, para uso em um exemplo de moldagem de tipo.

O primeiro trecho define uma nova classe base chamada `MediaItem`. Essa classe fornece funcionalidades básicas para qualquer tipo de item que aparecer em uma biblioteca de mídia digital. Especificamente, ela declara uma propriedade `nome` do tipo `String`, e um inicializador `init name`. (É considerado que todo item de mídia, incluindo todos os filmes e músicas, terão um nome).

```swift

class MediaItem {

    var name: String

    init(name: String) {

        self.name = name

    }

}

```

O próximo trecho define duas subclasses de `MediaItem`. A primeira subclasse, `Movie`, encapsula informações adicionais sobre um filme. Ela adiciona uma propriedade `director` no topo da classe base `MediaItem`, com um inicializador correspondente. A segunda subclasse, `Song`, adiciona uma propriedade `artist` e um inicializador no topo da classe base.

O último trecho de código cria uma lista constante chamada `library`, a qual contém duas instâncias de `Movie` e três de `Song`. O tipo da lista `library` é inferido ao inicializá-la com o conteúdo de uma lista literal. O verificador de tipo de Swift é capaz de deduzir que `Movie` e `Song` têm uma superclasse em comum do tipo `MediaItem`, logo é inferido o tipo de `[MediaItem]` para a lista `library`:

```swift

let library = [

    Movie(name: "Casablanca", director: "Michael Curtiz"),

    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),

    Movie(name: "Citizen Kane", director: "Orson Welles"),

    Song(name: "The One And Only", artist: "Chesney Hawkes"),

    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")

]

// the type of "library" is inferred to be [MediaItem]

```

Os itens armazenados em `library` ainda são instâncias de `Movie` e `Song` por trás das cenas. Porém, se você iterar sobre o conteúdo dessa lista, os itens que você vai receber de volta são tipados como `MediaItem`, e não como `Movie` e `Song`. A fim de poder trabalhar com eles usando seus tipos nativos, você precisa verificar seus tipos, ou reduzir o molde - do original *downcast* - do tipo deles para um tipo diferente, como descrito abaixo.

### Verificando o Tipo
Use o operador de verificação de tipo (`is`) para verificar se uma instância é de um certo de tipo de uma subclasse. Esse operador retorna `true` se a instância é do tipo de uma subclasse e `falso` caso contrário.

O exemplo abaixo define duas variáveis, `movieCount` e `songCount`, as quais contam o número de instâncias `Movie` e `Song` na lista `library`:

```swift

var movieCount = 0

var songCount = 0

for item in library {

    if item is Movie {

        ++movieCount

    } else if item is Song {

        ++songCount

    }

}

print("Media library contains \(movieCount) movies and \(songCount) songs")

// prints "Media library contains 2 movies and 3 songs

```
Este exemplo itera sobre todos os itens na lista `library`. A cada passo, o laço `for-in` configura a constante `item` para o próximo `MediaItem` na lista.

`item is Movie` retorna `true` se o `MediaItem` atual é uma instância de `Movie` e `falso` caso contrário. De forma similar, `item is Song` verifica se o item é uma instância de `Song`. Ao final do laço `for-in`, os valores de `movieCount` e `songCount` contém uma contagem de quantas instâncias `MediaItem` foram encontradas em cada tipo.

### Reduzindo/Redução o/do/de Molde ????

Uma constante ou variável de um certo tipo de uma classe pode na verdade se referir a uma instância de uma subclasse por trás das cenas. Sempre que você acreditar que este possa ser o caso, você pode tentar realizar uma redução do molde para o tipo de uma subclasse com o operador de moldagem de tipo (`as?` ou `as!`)

Como a redução do molde pode falhar, o operador de moldagem de tipo vem em duas formas distintas. A forma condicional, `as?`, retorna um valor opcional do tipo para o qual você está tentando realizar a redução do molde. A forma forçada, `as!`, tenta a redução do molde e força o desempacotamento do resultado em uma única ação composta.

Use a forma condicional do operador de moldagem de tipo (`as?`) quando você não tem certeza se a redução do molde será bem sucessida. Esta forma do operador sempre retornará um valor opcional, e o valor será `nil` se não foi possível realizar a redução do molde. Isto permite a você verificar se a redução do molde foi bem sucessida.

Use a forma forçada do operador de moldagem de tipo (`as!`) apenas quando você tem certeza que a redução do molde sempre será bem sucessida. Esta forma do operador irá disparar um erro em tempo de execução se você tentar realizar a redução do molde para um tipo incorreto de uma classe.

O exemplo abaixo itera sobre cada `MediaItem` na `library`, e imprime uma descrição apropriada para cada item. Para fazer isso, ele precisa acessar cada item verdadeiramente como um `Movie` ou `Song`, e não apenas como um `MediaItem`. Isto é necessário a fim de possibilitar o acesso à propriedade `director` ou `artist` de um `Movie` ou `Song` para serem usados na descrição.

Neste exemplo, cada item na lista pode ser um `Movie`, ou pode ser um `Song`. Você não sabe previamente qual classe usar para cada item, logo é apropriado usar a forma condicional do operador de moldagem de tipo (`as?`) para verificar a redução do molde cada vez através da iteração:

```swift

for item in library {

    if let movie = item as? Movie {

        print("Movie: '\(movie.name)', dir. \(movie.director)")

    } else if let song = item as? Song {

        print("Song: '\(song.name)', by \(song.artist)")

    }

}

// Movie: 'Casablanca', dir. Michael Curtiz

// Song: 'Blue Suede Shoes', by Elvis Presley

// Movie: 'Citizen Kane', dir. Orson Welles

// Song: 'The One And Only', by Chesney Hawkes

// Song: 'Never Gonna Give You Up', by Rick Astley

```

O exemplo começa tentando realizar uma redução do molde no valor atual como um `Movie`. Como `item` é uma instância de `MediaItem`, é possível que ela seja um `Movie`; igualmente, também é possível que ela seja um `Song`, ou até mesmo um `MediaItem`. Por causa dessa incerteza, a forma `as?` do operador de moldagem de tipo retorna um valor opcional quando é realizada a tentativa de redução do molde para o tipo da subclasse. O resultado de `item as? Movie` é do tipo `Movie?`, ou "`Movie` opcional"

Realizar a redução do molde para `Movie` falha quando aplicado às instâncias na lista biblioteca. Para lidar com isso, o exemplo acima utiliza o vínculo opcional para verificar se o `Movie` opcional realmente contém um valor (ou seja, para descobrir a redução do molde foi bem sucessida). Esse *Optional Binding* é escrito "`if let movie = item as? Movie`", que pode ser lido como:

"Tente acessar `item` como um `Movie`. Se obtiver sucesso, configure uma nova constante temporária chamada `movie` para o valor armazenado no `Movie` opcional retornado."

Se a redução do molde for bem sucessida, as propriedades de `movie` são usadas posteriormente para imprimir uma descrição para essa instância de `Movie`, incluindo o nome de seu `director`. Um princípio similar é usado para verificar por instâncias de `Song`, e imprimir uma descrição apropriada (incluindo o nome do `artist`) sempre que um `Song` for achado na biblioteca.

> NOTA <br>

> *Casting* não modifica realmente a instância ou modifica seua valores. Por baixo dos panos, a instância permanece a mesma; ela é simplesmente tratada e acessada como uma instância do tpo para o qual ela foi *cast* (moldada...?)

### Moldando o tipo a partir de *Any* e *AnyObject*

Swift fornece dois pseudônimos especiais para trabalhar com valores não especificados:

- `AnyObject` pode representar uma instância de qualquer classe.
- `Any` pode representar uma instância de qualquer tipo no geral, incluindo tipos de funções.


> NOTA <br> 

> Use `Any` e `AnyObject` apenas quando você precisar explicitamente dos comportamentos e recursos que eles fornecem. É sempre melhor ser específico acerca dos tipos que você espera dentro do seu código. 

### *AnyObject*

Quando trabalhando com as *APIs* de *Cocoa*, é comum receber uma lista com um tipo de `[AnyObject]`, ou "uma lista de valores do tipo de qualquer objeto". Isto porque *Objective-C* não possui listas tipadas explicitamente. Porém, você pode sempre estar certo sobre o tipo dos objetos contidos em tal lista apenas por meio das informações que você sabe sobre a *API* que forneceu a lista.

Nessas situações, você pode usar a versão forçada do operador de molde do tipo (`as!`) para realizar a redução do molde em cada item na lista para uma classe mais específica do que `AnyObject`, sem a necessidade de um desempacotamento opcional.

O exemplo abaixo define uma lista do tipo `[AnyObject]` e popula essa lista com três instâncias da classe `Movie`:

```swift

let someObjects: [AnyObject] = [

    Movie(name: "2001: A Space Odyssey", director: "Stanley Kubrick"),

    Movie(name: "Moon", director: "Duncan Jones"),

    Movie(name: "Alien", director: "Ridley Scott")

]

```

Como essa lista é conhecida apenas por conter instâncias de `Movie`, você pode realizar a redução do molde e desempacotar diretamente para um `Movie` não opcional com a versão forçada do operador de molde do tipo (`as!`):

```swift

for object in someObjects {

    let movie = object as! Movie

    print("Movie: '\(movie.name)', dir. \(movie.director)")

}

// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick

// Movie: 'Moon', dir. Duncan Jones

// Movie: 'Alien', dir. Ridley Scott

```

Para uma forma ainda mais curta dessa iteração, realizar a redução do molde na lista `someObjects` para um tipo `[Movie]` as invés de realizar a redução do molde em cada item:

```swift

for movie in someObjects as! [Movie] {

    print("Movie: '\(movie.name)', dir. \(movie.director)")

}

// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick

// Movie: 'Moon', dir. Duncan Jones

// Movie: 'Alien', dir. Ridley Scott

```

### *Any*

Aqui está um exemplo do uso de `Any` para se trabalhar com uma mistura de diferentes tipos, incluindo tipos de funções e tipos que não são classes. O exemplo cria uma lista chamada `things`, que pode armazenar valores do tipo `Any`:

```swift

var things = [Any]()

things.append(0)

things.append(0.0)

things.append(42)

things.append(3.14159)

things.append("hello")

things.append((3.0, 5.0))

things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))

things.append({ (name: String) -> String in "Hello, \(name)" })

```

A lista `things` contém dois valores `Int`, dois `Double`, um `String`, uma tupla do tipo `(Double, Double)`, o filme `Ghostbusters`, e uma expressão *closure* que leva como parâmetro um valor `String` e retorna um outro valor `String`.

Você pode usar os operadores `is` e `as` nos casos de uma instrução `switch` para descobrir o tipo específico de uma constante ou variável que é conhecida apenas por ser do tipo `Any` ou `AnyObject`. O exemplo abaixo itera sobre os itens na lista `things` e consulta o tipo de cada item com a instrução `switch`. Muitos dos casos de um instrução `switch` vinculam seu valor combinado à uma constante do tipo especificado para permitir que seu valor seja impresso:

```swift

for thing in things {

switch thing {

    case 0 as Int:

        print("zero as an Int")

    case 0 as Double:

        print("zero as a Double")

    case let someInt as Int:

        print("an integer value of \(someInt)")

    case let someDouble as Double where someDouble > 0:

        print("a positive double value of \(someDouble)")

    case is Double:

        print("some other double value that I don't want to print")

    case let someString as String:

        print("a string value of \"\(someString)\"")

    case let (x, y) as (Double, Double):

        print("an (x, y) point at \(x), \(y)")

    case let movie as Movie:

        print("a movie called '\(movie.name)', dir. \(movie.director)")

    case let stringConverter as String -> String:

        print(stringConverter("Michael"))

    default:

        print("something else")

    }

}

// zero as an Int

// zero as a Double

// an integer value of 42

// a positive double value of 3.14159

// a string value of "hello"

// an (x, y) point at 3.0, 5.0

// a movie called 'Ghostbusters', dir. Ivan Reitman

// Hello, Michael

```


