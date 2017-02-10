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

### Opções de *Subscript*

*Subscripts* podem ter qualquer número de parâmetros de entrada, e estes parâmetros podem ser de qualquer tipo. *Subscripts* também podem retornar qualquer tipo. *Subscripts* podem usar *variadic parameters*, mas eles não podem usar parâmetros *in-out* ou fornecer valores padrão.

Uma *class* ou *struct* pode fornecer quantas implementações de *subscripts* forem necessárias, e o *subscript* apropriado para uso será inferido baseado nos tipos de valor ou valores que estão entre os colchetes do *subscript* no momento em que o *subscript* for usado. Esta definição de múltiplos *subscripts* é chamada de *sobrecarga de subscript*.

Apesar de ser comum um *subscript* receber um único parâmetro, é possível também definir um *subscript* com múltiplos parâmetros se for apropriado para o seu tipo. O exemplo a seguir define uma estrutura `Matrix`, a qual representa uma matriz bidimensional de valores de tipo `Double`. O *subscript* da estrutura `Matrix` recebe dois parâmetros inteiros:

```swift
struct Matrix {
    let rows: Int, columns: Int

    var grid: [Double]

    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }

    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }

    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Índice fora do intervalo")
            return grid[(row * columns) + column]
        }

        set {
            assert(indexIsValid(row: row, column: column), "Índice fora do intervalo")
            grid[(row * columns) + column] = newValue
        }
    }
}
```

`Matrix` fornece um inicializador que recebe dois parâmetros chamados `rows` e `columns`, e cria um *array* que é grande o bastante para armazenar `rows * columns` valores de tipo `Double`. Cada posição da matriz recebe um valor inicial `0.0`. Para tal, o tamanho do *array* e o valor inicial `0.0` são passados para um inicializador de *array* que cria e inicializa um novo *array* com o tamanho correto. Este inicializador é descrito com mais detalhes em Criando um Array com um Valor Padrão.<!--TODO: Adicionar link para o capítulo Criando um Array com um Valor Padrão -->

É possível construir uma nova instância de `Matrix` passando o número apropriado de linhas e colunas para o seu inicializador:

```swift
var matrix = Matrix(rows: 2, columns: 2)
```

O exemplo anterior cria uma nova instância de `Matrix` com duas linhas e duas colunas. O *array* `grid` desta instância de `Matrix` é efetivamente uma versão achatada (*flattened* no original) da matriz, lendo-se do canto superior esquerdo para o inferior direito:

<!-- TODO: Adicionar imagem -->

Valores podem ser atribuidos na matriz passando-se o número de linhas e colunas no *subscript*, separados por vírgula:

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

Estes dois comandos chamam o método `set` do *subscript* para atribuir o valor `1.5` à posição superior direita da matriz (onde `row` é `0` e `column` é `1`), e `3.2` à posição inferior esquerda (onde `row` é `1` e `column` é `0`):

<!-- TODO: Adicionar imagem -->

Os métodos `get` e `set` do *subscript* de `Matrix` contém uma asserção que verifica se os valores de `row` e `column` são válidos. Para auxiliar essas asserções, `Matrix` inclui um método de conveniência chamado `indexIsValid(row:column:)`, o qual verifica se os valores de `row` e `column` então dentro dos limites da matriz:

```swift
func indexIsValidForRow(row: Int, column: Int) -> Bool {
    return row >= 0 && row < rows && column >= 0 && column < columns
}
```

Uma asserção é disparada se você tentar acessar um *subscript* que está fora dos limites da matriz:

```swift
let someValue = matrix[2, 2]
// Isto dispara uma asserção, porque [2, 2] está fora dos limites da matriz
```
