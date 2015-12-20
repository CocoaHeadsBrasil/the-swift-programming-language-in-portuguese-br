# Closures

*Closures* são blocos de funcionalidade idependentes que podem ser passados e usados em seu código. *Closures*, em Swift, são similares aos blocos em C e Objective-C e à *lambdas* em outras linguagens de programção.

*Closures* podem capturar e armazenar referências para qualquer constante e variáveis do contexto no qual ele foi definido. Isso é conhecido como *fechamento sobre* constantes e variáveis. O Swift se encarrega de realizar o gerenciamento de memória de captura por você.

>Nota

>Não se preocupe se você não esta familiarizado com o conceito de captura. Isso é explicado em detalhes em [Capturando Valores](./closures.md#capturing_values).

Funcões alinhadas e globais, como apresentado em [Funcões](./functions.md#functions), são casos realmente especiais de *closures*. *Closures* possuem uma, de três formas:

* Funcões globais são *closures* que possuem um nome e podem capturar quaisquer valores.
* Funções alinhadas são *closures* que possuem um nome e podem capturar valores a partir de suas funções fechadas.
* Expressões de *closure* são *closures* sem nome, escritos com uma sintaxe limpa, que podem capturar valores dos contextos ao seu redor.

As expressões de *closure* do Swift possuem uma sintaxe clara com otimizações que encorajam uma sintaxe curta e que evita mal entendidos em cenários comums. Essas otimizações incluem:

* Dedução de parametros e tipos de valor de retorno a partir do contexto.
* Retorno implícito a partir de expressões de fechamento únicas.
* Argumentos com nomes simples .
* Sintaxe do fechamento de fuga. (Trailing closure sintax).

###Expressões de Closure

Funções alinhadas, como apresentado em [Funções Alinhadas](./functions.md#nested_functions), são um meio conveniente de nomear e definir blocos independentes de código como parte de uma função maior. Contudo, algumas vezes é útil escrever versões mais curtas de funções como contructos sem uma declaração completa e nome. Isso particularmente verdade quando você trabalha com funções ou métodos que pegam funções como um ou mais de seus parametros.

Expressões de *closure* são uma meio de escrever *closures* com uma sintax breve e clara. Expressões de *closure* proporcionam varias otimizações de sintaxe para se escrever *closures* de uma maneira breve sem que haja perca de claridade ou intenção. Os exemplos de expressões de *closures* abaixo ilustram essas otimizações através do refinamento de um único exemplo de um methodo de ordenação `sort(_:)` através de várias iterações, cada qual expressa a mesma funcionalidade na maneira mais sucinta.

####O Metódo de Ordenação

A biblioteca padrão do Swift disponibiliza um metódo chamado `sort(_:)`, que ordena um `array` de valores com tipo conhecido, baseado na saída de um *closure* de ordenação que você deisponibiliza. Uma vez que o seu *closure* complete o processo de ordenação, o método `sort(_:)` devolve um novo `array` do mesmo tipo e tamanho do original, com os elementos ordenados corretamente. O `array` orinal não é modificado pelo método `sort(_:)`.

Os exemplos de expressões de *clousers* abaixo usam o método `sort(_:)` para ordenar um `array` de valores do tipo `String` em ordem alfabética invertida. Este é o `array` inicial a ser ordenado:

```swift
    let names = ["Chirs", "Alex", "Ewa", "Barry", "Daniella"]
```

O método `sort(_:)` aceita um *clouser* que pegue dois argumentos do mesmo tipo como `array` de conteúdo, e devolva um valor `Bool` para condicionar se o primeiro valor deveria aparecer antes ou depois do segundo valor, uma vez que os valores estejam ordenados. O *closure* de ordenação precisa devolver `true` se o primeiro valor deveria aparecer antes do segundo valor, e `false` caso contrário.

Este exemplo está ordenando um `array` com valores do tipo `String`, e portanto o *closure* de ordenação precisa ser uma função do tipo `(String, String) -> Bool`.

Uma maneira de prover *closure* de ordenação é escrever uma funcão normal do tipo correto, e passar-la como um argumento para o método `sort(_:)`:

```swift
    func backwards(s1: String, _ s2: String) -> Bool {
        return s1 > s2
    }
    
    var reversed = names.sort(backwards)
    //reversed está idêntico a ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

Se o primeira conjunto de caractéres (`s1`) é maior do que a segunda conjunto de caractéres (`s2`), a função `backwards(_:_:)` irá de devolver `true`, indicando que `s1` deveria aparecer antes de `s2`no `array` ordenado. Para os caractéres no conjunto de caractéres, "maior que" significa "aparece mais tarde no alfabeto do que". Isto significa que que a letra `"B"` é "maior que" a letra `"A"`, e o conjunto de caractéres `"Tom"` é maior que o conjunto de caractéres `"Tim"`. Isso resulta em uma ordenação alfabetica inversa , com `"Barry"` sendo colocado antes de `"Alex"`e assim por diante.

Contudo, esta não é a melhor maneira de escrever o que essencialmente é uma funcão de expressão única `(a > b)`. Neste exemplo seria preferível escrever o *closure* de ordenação usando sintaxe de expressão de *closure*.