## Extensões

Extensões adicionam novas funcionalidades para uma classe existente, estrutura, enumeração ou tipo de protocolo. Isso inclui a capacidade de estender tipos pelos quais você não possui acesso ao código fonte original (conhecido como modelo retroativo). 
Extensões são similares as categorias em Objetive-C. (Ao contrário das categorias em Objetive-C, extensões em Swift não possuem nomes.) 

Extensões em Swift podem: 

* Adicionar propriedades computadas e tipos computados 
* Define métodos de instância e tipos de métodos 
* Fornecer novos inicializadores 
* Define _subscripts_ 
* Define e usa tipos aninhados novos
* Faz um tipo existente conformar com um protocolo 

Em Swift, você ainda pode estender um protocolo para fornecer implementações de seus requisitos ou adicionar funcionalidades para que os tipos conformados possam tirar vantagem. Para mais detalhes veja [Extensões de Protocolo](guia/protocolos.md#extensoesdeprotocolo) 

> **Nota** 
>
Extensões podem adicionar novas funcionalidades para um tipo, mas não podem sobrescrever funcionalidades existentes.