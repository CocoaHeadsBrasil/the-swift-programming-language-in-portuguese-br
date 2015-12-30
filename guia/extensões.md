## Extensões

_Extensões_ adicionam novas funcionalidades a uma classe existente, estrutura, enumeração ou tipo de protocolo. Isso inclui a capacidade de estender tipos pelos quais você não possui acesso ao código fonte original (conhecido como _modelo retroativo_). 
Extensões são similares às categorias em Objetive-C. (Ao contrário das categorias em Objetive-C, extensões em Swift não possuem nomes.) 

Extensões em Swift podem: 

* Adicionar propriedades computadas e tipos computados 
* Definir métodos de instância e métodos de tipo 
* Fornecer novos inicializadores 
* Definir _subscripts_ 
* Definir e usar tipos aninhados novos
* Fazer um tipo existente obedecer com um protocolo 

Em Swift, você ainda pode estender um protocolo para fornecer implementações de seus requisitos ou adicionar funcionalidades para que os tipos conformados possam tirar vantagem. Para mais detalhes veja [Extensões de Protocolo](guia/protocolos.md#extensoesdeprotocolo) 

> **Nota** 
>
Extensões podem adicionar novas funcionalidades para um tipo, mas não podem sobrescrever funcionalidades existentes.