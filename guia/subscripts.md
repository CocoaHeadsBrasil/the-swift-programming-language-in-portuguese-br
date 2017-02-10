## Subscripts

*Classes*, *structures* e *enumerations* podem definir *subscripts*, os quais são atalhos para acessar os elementos de uma coleção, lista ou sequência. *Subscripts* são usados para definir e obter valores a partir de um índice, sem a necessidade de métodos separados para estas ações. Por exemplo, para acessar elementos de uma instância de `Array` utiliza-se `someArray[index]` e para acessar elementos de uma instância de `Dictionary` utiliza-se `someDictionary[key]`.

É possível definir múltiplos *subscripts* para um único tipo, e o *subscript* sobrecarregado apropriado para uso é selecionado baseado no tipo do índice passado para o *subscript*. *Subscripts* não estão limitados a uma única dimensão, e é possível definir *subscripts* com múltiplos parâmetros de entrada para se adequar às necessidades do tipo que está sendo criado.

### Sintaxe de *Subscript*

*Subscripts* permitem que instâncias de um tipo sejam consultadas escrevendo um ou mais valores entre colchetes logo após o nome da instância. Sua sintaxe é similar à sintaxe de métodos de instância e de propriedades computadas.

Para definir um *subscripts* utiliza-se a palavra-chave `subscript`, então especifica-se um ou mais parâmetros de entrada e um tipo de retorno, da mesma forma que métodos de instância. Diferente de métodos de instância, *subscripts* podem ser do tipo leitura-escrita ou somente-leitura. Este comportamento é obtido através do uso de métodos `get` e `set` do mesmo modo que é feito em propriedades computadas:

```swift
subscript(index: Int) -> Int {

    get {
        // Retorne aqui um valor apropriado
    }

    set(newValue) {
        // Execute aqui uma ação de atribuição apropriada
    }

}
```

O tipo de `newValue` é o mesmo do valor de retorno do *subscript*. Assim como em propriedades computadas, é possível não especificar o parâmetro `(newValue)` do método `set`. Um parâmetro padrão chamado `newValue` é fornecido para o método `set` caso nenhum parâmetro seja especificado. Assim como em propriedades computadas de tipo somente-leitura, é possível desprezar a palavra-chave `get` para *subscripts* de tipo somente-leitura:

```swift
subscript(index: Int) -> Int {

    // Retorne aqui um valor apropriado

}
```

Aqui está um exemplo de uma implementação de um *subscript* de tipo somente-leitura, o qual define uma *struct* chamada `TimesTable` para representar uma tabuada de multiplicação *n*-ária de inteiros:

```swift
struct TimesTable {

    let multiplier: Int

    subscript(index: Int) -> Int {

        return multiplier * index

    }

}

let threeTimesTable = TimesTable(multiplier: 3)

print("seis vezes três é \(threeTimesTable[6])")

// Imprime "seis vezes três é 18"
```

Neste exemplo, uma nova instância de `TimesTable` é criada para representar a tabuada de multiplicação por três. Isto é feito ao passar o valor `3` ao inicializador da *struct* como o valor a ser usado pelo parâmetro `multiplier` desta instância.

É possível fazer consultas à instância `threeTimesTable` chamando o seu *subscript*, como mostrado na chamada `threeTimesTable[6]`. Este comando requisita a sexta entrada da tabuada de multiplicação por três, o qual retorna o valor `18`, ou `3` vezes `6`.

> NOTA
>
> Uma tabuada de multiplicação *n*-ária é baseada em uma definição matemática fixa. Não é apropriado atribuir um novo valor a `threeTimesTable[someIndex]`, por isso o *subscript* de `TimesTable` é definido como um *subscript* de tipo somente-leitura.

### Uso de *Subscript*

A função exata de um *subscript* depende do contexto no qual ele é usado. Normalmente, *subscripts* são usados como atalhos para acessar os elementos de uma coleção, lista ou sequência. Você é livre para implementar *subscripts* da forma mais apropriada à funcionalidade da sua *class* ou *struct*.

Por exemplo, o tipo `Dictionary` em Swift implementa um *subscript* para atribuir e recuperar os valores armazenados em uma instância de `Dictionary`.

É possível adicionar um valor em um *dictionary* fornecendo uma chave com o mesmo tipo que a chave do *dictionary* entre colchetes de *subscript*, e então atribuir um valor com o mesmo tipo que o valor do *dictionary* ao *subscript*:

```swift
var numberOfLegs = ["aranha": 8, "formiga": 6, "gato": 4]

numberOfLegs["pássaro"] = 2
```

O exemplo acima define uma variável chamada `numberOfLegs` e a inicializa com um literal *dictionary* contendo três pares chave-valor. O tipo do *dictionary* `numberOfLegs` é inferido como sendo `[String: Int]`. Após criar o *dictionary*, este exemplo usa a atribuição de *subscript* para adicionar uma chave `"pássaro"` de tipo `String` e o valor `2` de tipo `Int` ao *dictionary*. Para mais informações sobre *subscripts* em `Dictionary`, visite Acessando e Modificando um *Dictionary*.<!--TODO: Adicionar link para o capítulo Acessando e Modificando um Dictionary -->

> NOTA
>
> O tipo `Dictionary` em Swift implementa um *subscript* chave-valor que recebe e retorna um tipo *opcional*. Para o *dictionary* acima `numberOfLegs`, o *subscript* chave-valor recebe e retorna um valor do tipo `Int?`, ou "`Int` opcional". O tipo `Dictionary` usa um *subscript* de tipo opcional para representar o fato de que nem toda chave possuirá um valor associado, e para fornecer um meio de apagar um valor de uma chave atribuindo o valor `nil` para esta chave.
