## Closures

*Closures* são blocos de funcionalidade idependentes que podem ser passados e usados em seu código. *Closures*, em Swift, são similares aos blocos em C e Objective-C e à *lambdas* em outras linguagens de programação.

*Closures* podem capturar e armazenar referências para quaisquer constantes e variáveis do contexto no qual ele foi definido. Isso é conhecido como *closure* sobre constantes e variáveis. O Swift se encarrega de realizar o gerenciamento de memória de captura por você.

>Nota

>Não se preocupe se você não esta familiarizado com o conceito de captura. Isso é explicado em detalhes adiante em [Capturando Valores](./closures.md#capturing_values).

Funcões globais e alinhadas, como apresentado em [Funcões](./functions.md#functions), são casos realmente especiais de *closures*. *Closures* possuem uma, de três formas:

* Funcões globais são *closures* que possuem um nome e podem capturar quaisquer valores.
* Funções alinhadas são *closures* que possuem um nome e podem capturar valores a partir de suas funções fechadas.
* Expressões de *closure* são *closures* sem nome, escritos com uma sintaxe limpa, que podem capturar valores dos contextos ao seu redor.

As expressões de *closure* do Swift possuem uma sintaxe clara com otimizações que encorajam uma sintaxe curta e que evita mal entendidos em cenários comums. Essas otimizações incluem:

* Dedução de parametros e tipos de valor de retorno a partir do contexto.
* Retorno implícito a partir de expressões de *closure* únicas.
* Argumentos com nomes simples .
* Sintaxe do *closure* de fuga. - do inglês *Trailing closure sintax*.

###Expressões de ***closure***

Funções alinhadas, como apresentado em [Funções Alinhadas](./functions.md#nested_functions), são um meio conveniente de nomear e definir blocos independentes de código como parte de uma função maior. Contudo, algumas vezes é útil escrever versões mais curtas de funções como contructos sem uma declaração completa e nome. Isso é particularmente verdade quando você trabalha com funções ou métodos que pegam funções como um ou mais de seus parametros.

Expressões de *closure* são uma meio de escrever *closures* com uma sintax breve e clara. Expressões de *closure* proporcionam varias otimizações de sintaxe para se escrever *closures* de uma maneira breve sem que haja perca de claridade ou intenção. Os exemplos de expressões de *closure* abaixo ilustram essas otimizações através do refinamento de um único exemplo de um methodo de ordenação `sort(_:)` através de várias iterações, cada qual expressa a mesma funcionalidade na maneira mais sucinta.

####O Metódo de Ordenação

A biblioteca padrão do Swift fornece um metódo chamado `sort(_:)`, que ordena um `array` de valores com tipo conhecido, baseado na saída de um *closure* de ordenação que você fornece. Uma vez que o seu *closure* complete o processo de ordenação, o método `sort(_:)` devolve um novo `array` do mesmo tipo e tamanho do original, com os elementos ordenados corretamente. O `array` original não é modificado pelo método `sort(_:)`.

Os exemplos de expressões de *clousers* abaixo usam o método `sort(_:)` para ordenar um `array` de valores do tipo `String` em ordem alfabética invertida. Este é o `array` inicial a ser ordenado:

```swift
    let names = ["Chirs", "Alex", "Ewa", "Barry", "Daniella"]
```

O método `sort(_:)` aceita um *clouser* que pegue dois argumentos do mesmo tipo como `array` de conteúdo, e devolva um valor `Bool` para condicionar se o primeiro valor deveria aparecer antes ou depois do segundo valor, uma vez que os valores estejam ordenados. O *closure* de ordenação precisa devolver `true` caso o primeiro valor deva aparecer antes do segundo valor, e `false` caso contrário.

Este exemplo está ordenando um `array` com valores do tipo `String`, e portanto o *closure* de ordenação precisa ser uma função do tipo `(String, String) -> Bool`.

Uma maneira de prover um *closure* de ordenação é escrever uma funcão normal do tipo correto, e passar-la como um argumento para o método `sort(_:)`:

```swift
    func backwards(s1: String, _ s2: String) -> Bool {
        return s1 > s2
    }
    
    var reversed = names.sort(backwards)
    //reversed está idêntico a ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

Se o primeira conjunto de caractéres (`s1`) é maior do que a segunda conjunto de caractéres (`s2`), a função `backwards(_:_:)` irá de devolver `true`, indicando que `s1` deveria aparecer antes de `s2`no `array` ordenado. Para os caractéres no conjunto de caractéres, "maior que" significa "aparece mais tarde no alfabeto do que". Isto significa que a letra `"B"` é "maior que" a letra `"A"`, e o conjunto de caractéres `"Tom"` é maior que o conjunto de caractéres `"Tim"`. Isso resulta em uma ordenação alfabetica inversa , com `"Barry"` sendo colocado antes de `"Alex"`e assim por diante.

Contudo, esta não é a melhor maneira de escrever o que essencialmente é uma funcão de expressão única `(a > b)`. Neste exemplo seria preferível escrever o *closure* de ordenação usando sintaxe de expressão de *closure*.

###Sintaxe da Expressão de ***closure***

A sintaxe de expressão de *closure* geralmente possui a seguinte forma:

```swift
    {(parameters) -> return type in
        statements
    }
```
A sintaxe de expressões de *closure* podem usar parâmetros constantes, parâmetros variáveis e parâmetros de entrada e saída. Valores padrão não podem ser fornecidos. Parâmetros variádicos podem ser usados caso você os nomeie. Tuplas, também, podem ser utilizadas como tipos de parâmetro e tipos de devolução.

O exemplo abaixo mostra uma versão da expressão de *closure* da função `backwards(_:_:)` que definimos anteriormente:

```swift
reversed = names.sort({(s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

Note que a declaração dos parâmetros e o tipo de retorno para esse *closure* é idêntica a declaração da função `backwards(_:_:)`. Em ambos os casos, o *closure* é escrito como `(s1: String, s2: String) -> Bool`. Contudo, para a expressão de *closure* em linha, os parâmetros e os tipos de devolução são escritos dentro da chaves `{...}` e não fora delas.

O inicio do corpo do *closure* é introduzido pela palavra-chave `in`. Essa palavra-chave indica que a definição dos parâmetros e o tipo de devolução do *closure* terminou e o corpo do *closure* está prestes a começar.

Devido ao pequeno tamanho de seu corpo, o *closure* pode ser escrito em uma única linha de código:

```swift
reversed = names.sort({(s1:String, s2:String) -> Bool in return s1 > s2 })
```

Isso ilustra que as chamadas do método `sort(_:)` permaneceram as mesmas. Um par de parenteses ainda envolvem o argumento do método. Contudo, esse argumento agora é um *closure* em linha.

###Deduzindo o tipo a partir do contexto

Devido ao fato de o *closure* de ordenação ser passado como um método, o Swift e capaz de deduzir os tipos de paramêtros do *closure* e o tipo de valor que o *closure* devolve. O método `sort(_:)` começa a ser chamado sobre um *array* de *strings*, portanto seus argumentos devem ser uma função do tipo `(String, String) -> Bool`. Isso significa que os tipos `(String, String)` e `Bool` não precisam ser escritos como parte da definição da expressão de *closure*. Como todos os tipos podem ser deduzidos, a seta de devolução (`->`) e os parênteses envolta dos nomes dos parametros também podem ser omitidos:

```swift
reversed = names.sort( { s1, s2 in return s1 > s2 } )
```

É sempre possível deduzir os tipos dos parâmetros e o tipo de devolução quando passamos um *closure* para a função ou método como uma expressão de *closure* em linha. Como resultado, você nunca precisará escrever uma *inline closure* em sua forma completa quando o *closure* é usado como um argumento de função ou de método. Além do mais, você ainda pode fazer tipos explicitos se desejar, e encorajamos a faze-lo caso evite ambiguidade para os leitores do seu código. No caso do método `sort(_:)`, o propósito do *closure* é claro a partir do fato de que o objetivo é a ordenação, e é seguro para o leitor assumir que o *closure* está trabalhando com valores do tipo `String`, porque ele é usado para ordenação de um *array* de *strings*.

###Devolução Implicita de *Closure* de Expressão Única

*Closures* de expressão única podem devolver, de forma implicita, o resultado de suas expressões únicas através da omissão da palavra-chave `return` de sua declaração, assim como nesta versão do exemplo anterior:

```swift
    reversed = names.sort({ s1, s2 in s1 > s2 })
```

Aqui, o tipo da função do argumento do método `sort(_:)` torna claro que um valor `Bool` deverá ser devolvido pelo *closure*. Como o corpo do *closure* contain uma expressão única `( s1 > s2)` que devolve um valor `Bool`, não existe ambiguidade, e a palavra-chave `return` pode ser omitida.

###Nomes curtos para argumentos
O Swift disponibiliza automaticamente nomes curtos para argumentos em *inline closures*, que podem ser utilizados para se referirem a valores dos argumentos do *closure* através dos nomes `$0`,`$1`, `$2` e assim pordiante.

Se você utilizar esses nomes simplificados de argumentos na sua expressão de *closure*, você poderá omitir a lista de argumento do *closure* da sua definição, e o número e tipo do argumento com nome simplificado serão deduzidos a partir do tipo de função esperada. A palavra-chave `in` pode também ser omitida, porque a expressão do *closure* é feita totalmente em seu corpo:

```swift
    reversed = names.sort( {$0 > $1} )
```

Aqui, `$0`, e `$1` referem-se ao primeiro e segundo argumento do *closure*, que são do tipo `String`.

###Funções de Operadores
Existe ainda uma maneira mais simplificada de escrever a expressão de *closure* que vimos acima.
O tipo `String` do Swift define uma implementação especifica do seu operador de "maior que" (`>`) como uma função que possui dois parametros do tipo `String` e retorna um valor do tipo `Bool`. Que combina exatamente com o tipo de função esperada pelo método `sort(_:)`. Desta forma, você pode simplesmente passar o operador "maior que", e o Swift irá deduzir que você quer usar a implementação específica para *strings*:

```swift
    reversed = names.sort(>)
```

Para saber mais sobre funções de operadores, veja [Funções de Operadores](./functions.md#operation_functions).

###***Closures* de fuga** - *Trailing Closures*