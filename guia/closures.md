# Closures

*Closures* são blocos de funcionalidade idependentes que podem ser passados e usados em seu código. *Closures*, em Swift, são similares aos blocos em C e Objective-C e à *lambdas* em outras linguagens de programção.

*Closures* podem capturar e armazenar referências para qualquer constante e variáveis do contexto no qual ele foi definido. Isso é conhecido como *fechamento sobre* constantes e variáveis. O Swift se encarrega de realizar o gerenciamento de memória de captura por você.

>Nota

>Não se preocupe se você não esta familiarizado com o conceito de captura. Isso é explicado em detalhes em [Capturando Valores](./closures.md#capturing_values).

Funcões alinhadas e globais, como apresentado em [Funcões](./functions.md#Functions), são casos realmente especiais de *closures*. *Closures* possuem uma, de três formas:

* Funcões globais são *closures* que possuem um nome e podem capturar quaisquer valores.
* Funções alinhadas são *closures* que possuem um nome e podem capturar valores a partir de suas funções fechadas.
* Expressões de *closure* são *closures* sem nome, escritos com uma sintaxe limpa, que podem capturar valores dos contextos ao seu redor.

As expressões de *closure* do Swift possuem uma sintaxe clara com otimizações que encorajam uma sintaxe curta e que evita mal entendidos em cenários comums. Essas otimizações incluem:

* Dedução de parametros e tipos de valores de retorno a partir do contexto.
* Retorno implícito a partir de expressões de fechamento únicas.
* Argumentos com nomes simples .
* Sintaxe do fechamento de fuga. (Trailing closure sintax).