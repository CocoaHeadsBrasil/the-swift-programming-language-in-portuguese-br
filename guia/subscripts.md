## Subscripts

*Classes*, *structures* e *enumerations* podem definir *subscripts*, que são atalhos usados para acessar elementos de uma coleção, lista ou sequência. Você pode usar *subscripts* para definir e obter valores a partir de um índice sem a necessidade de métodos sepadados para estas ações (métodos *get* e *set*). Por exemplo, você pode acessar elementos de uma instância de 'Array' utilizando 'someArray[index]' e acessar elementos de uma instância de 'Dictionary' utilizando 'someDictionary[key]'.

Você pode definir múltiplos *subscripts* para um mesmo tipo, ou seja, você pode sobrecarregar *subscripts*, e o *subscript* apropriado para uso é escolhido baseado no tipo do índice que foi passado para o *subscript*. *Subscripts* não estão limitados a uma única dimensão, e você pode definir *subscripts* com múltiplos parâmetros de entrada para se adequar às necessidades do tipo que você está definindo.
