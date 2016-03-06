## Subscripts

*Classes*, *structures* e *enumerations* podem definir *subscripts*, que são atalhos usados para acessar elementos de uma coleção, lista ou sequência. Você pode usar *subscripts* para definir e obter valores a partir de um índice sem a necessidade de métodos sepadados para estas ações. Por exemplo, você pode acessar elementos de uma instância de 'Array' utilizando 'someArray[index]' e acessar elementos de uma instância de 'Dictionary' utilizando 'someDictionary[key]'.

Você pode definir múltiplos *subscripts* para um mesmo tipo, e o *subscript* sobrecarregado apropriado para uso é escolhido baseado no tipo do índice que foi passado para o *subscript*. *Subscripts* não estão limitados a uma única dimensão, e você pode definir *subscripts* com múltiplos parâmetros de entrada para se adequar às necessidades do tipo que você está definindo.

### Sintaxe de *Subscript*

*Subscripts* permitem que você solicite instâncias de um tipo escrevendo um ou mais valores entre colchetes logo após o nome da instância. Sua sintaxe é similar à sintaxe de métodos de instância e de propriedades computadas. Você define *subscripts* com a palavra-chave 'subscript', e especifica um ou mais parâmetros de entrada juntamente com um tipo de retorno, assim como é feito em métodos de instância. Diferentemente de métodos de instância, *subscripts* podem ser de leitura-escrita ou somente-leitura. Este comportamento é obtido através do uso de métodos 'get' e 'set' do mesmo modo que é feito com propriedades computadas:

'''swift
subscript(index: Int) -> Int {

    get {
        // Aqui é retornado um valor subscrito apropriado
    }

    set(newValue) {
        // Aqui é executada uma ação de atribuição apropriada
    }

}
'''

O tipo de 'newValue' é o mesmo do valor de retorno do *subscript*. Assim como em propriedades computadas, você pode escolher não especificar o parâmetro 'newValue' do método 'set'. Um parâmetro padrão chamado 'newValue' é fornecido para o seu método 'set' se você não especificá-lo.

Assim como em propriedades computadas de somente-leitura, você pode desprezar a palavra-chave 'get' para *subscripts* de somente-leitura:

'''swift
subscript(index: Int) -> Int {

    // Aqui é retornado um valor subscrito apropriado

}
'''

Aqui está um exemplo de uma implementação de um *subscript* de somente-leitura, o qual define um *struct* chamado 'TimesTable' para representar uma tabuada de multiplicação *n*-ária de inteiros:

'''swift
struct TimesTable {

    let multiplier: Int

    subscript(index: Int) -> Int {

        return multiplier * index

    }

}

let threeTimesTable = TimesTable(multiplier: 3)

print("six times three is \\(threeTimesTable[6])")

// Imprime "six times three is 18"
'''

Neste exemplo, uma nova instância de 'TimesTable' é criada para representar a tabuada de multiplicação por três. Isto é indicado ao passar o valor '3' ao inicializador do *struct* como o valor a ser usado pelo parâmetro 'multiplier' da instância.

Você pode fazer solicitações à instância 'threeTimesTable' utilizando o seu *subscript*, como mostrado na chamada 'threeTimesTable[6]'. Este comando solicita a sexta entrada da tabuada de multiplicação por 3, o qual retorna o valor '18', ou '3' multiplicado por '6'.

> NOTA
>
> Uma tabuada de multiplicação *n*-ária é baseada em uma definição matemática fixa. Não é apropriado atribuir a 'threeTimesTable[someIndex]' um novo valor, e por isso o *subscript* de 'TimesTable' é definido como um *subscript* de somente-leitura.
