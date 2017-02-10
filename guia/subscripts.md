## Subscripts

*Classes*, *structures* e *enumerations* podem definir *subscripts*, os quais são atalhos para acessar os elementos de uma coleção, lista ou sequência. *Subscripts* são usados para definir e obter valores a partir de um índice, sem a necessidade de métodos separados para estas ações. Por exemplo, para acessar elementos de uma instância de 'Array' utiliza-se 'someArray[index]' e para acessar elementos de uma instância de 'Dictionary' utiliza-se 'someDictionary[key]'.

É possível definir múltiplos *subscripts* para um único tipo, e o *subscript* sobrecarregado apropriado para uso é selecionado baseado no tipo do índice passado para o *subscript*. *Subscripts* não estão limitados a uma única dimensão, e é possível definir *subscripts* com múltiplos parâmetros de entrada para se adequar às necessidades do tipo que está sendo criado.

### Sintaxe de *Subscript*

*Subscripts* permitem que instâncias de um tipo sejam consultadas escrevendo um ou mais valores entre colchetes logo após o nome da instância. Sua sintaxe é similar à sintaxe de métodos de instância e de propriedades computadas.

Para definir um *subscripts* utiliza-se a palavra-chave 'subscript', então especifica-se um ou mais parâmetros de entrada e um tipo de retorno, da mesma forma que métodos de instância. Diferente de métodos de instância, *subscripts* podem ser do tipo leitura-escrita ou somente-leitura. Este comportamento é obtido através do uso de métodos 'get' e 'set' do mesmo modo que é feito em propriedades computadas:

'''swift
subscript(index: Int) -> Int {

    get {
        // Retorne aqui um valor apropriado
    }

    set(newValue) {
        // Execute aqui uma ação de atribuição apropriada
    }

}
'''

O tipo de 'newValue' é o mesmo do valor de retorno do *subscript*. Assim como em propriedades computadas, é possível não especificar o parâmetro '(newValue)' do método 'set'. Um parâmetro padrão chamado 'newValue' é fornecido para o método 'set' caso nenhum parâmetro seja especificado. Assim como em propriedades computadas de tipo somente-leitura, é possível desprezar a palavra-chave 'get' para *subscripts* de tipo somente-leitura:

'''swift
subscript(index: Int) -> Int {

    // Retorne aqui um valor apropriado

}
'''

Aqui está um exemplo de uma implementação de um *subscript* de tipo somente-leitura, o qual define uma *struct* chamada 'TimesTable' para representar uma tabuada de multiplicação *n*-ária de inteiros:

'''swift
struct TimesTable {

    let multiplier: Int

    subscript(index: Int) -> Int {

        return multiplier * index

    }

}

let threeTimesTable = TimesTable(multiplier: 3)

print("seis vezes três é \\(threeTimesTable[6])")

// Imprime "seis vezes três é 18"
'''

Neste exemplo, uma nova instância de 'TimesTable' é criada para representar a tabuada de multiplicação por três. Isto é feito ao passar o valor '3' ao inicializador da *struct* como o valor a ser usado pelo parâmetro 'multiplier' desta instância.

É possível fazer consultas à instância 'threeTimesTable' chamando o seu *subscript*, como mostrado na chamada 'threeTimesTable[6]'. Este comando requisita a sexta entrada da tabuada de multiplicação por três, o qual retorna o valor '18', ou '3' vezes '6'.

> NOTA
>
> Uma tabuada de multiplicação *n*-ária é baseada em uma definição matemática fixa. Não é apropriado atribuir um novo valor a 'threeTimesTable[someIndex]', por isso o *subscript* de 'TimesTable' é definido como um *subscript* de tipo somente-leitura.
