## Extensões

_Extensões_ adicionam novas funcionalidades a uma classe existente, estrutura, enumeração ou tipo de protocolo. Isso inclui a capacidade de estender tipos pelos quais você não possui acesso ao código fonte original (conhecido como _modelo retroativo_). 
Extensões são similares às categorias em Objetive-C. (Ao contrário das categorias em Objetive-C, extensões em Swift não possuem nomes.) 

Extensões em Swift podem: 

* Adicionar propriedades computadas e tipos computados 
* Definir métodos de instância e métodos de tipo 
* Fornecer novos inicializadores 
* Definir _subscripts_ 
* Definir e usar tipos aninhados novos
* Fazer um tipo existente obedecer a um protocolo 

Em Swift, você ainda pode estender um protocolo para fornecer implementações de seus requisitos ou adicionar funcionalidades para que os tipos que o obedeçam possam tirar vantagem. Para mais detalhes veja [Extensões de Protocolo](guia/protocolos.md#extensoesdeprotocolo) 

> NOTA
>
> Extensões podem adicionar novas funcionalidades para um tipo, mas não podem sobrescrever funcionalidades

### Sintaxe de Extensões 

Declare as extensões com a palavra-chave `extension`. 

```swift 
extension SomeType { 
// novas funcionalidades a serem adicionadas à SomeType entram aqui 
} 
``` 

Uma extensão pode estender uma funcionalidade existente para que ela possa adotar um ou mais protocolos. Quando for este o caso, o nome do protocolo é escrito exatamente da mesma forma que uma classe ou estrutura. 

```swift 
extension SomeType: SomeProtocol, AnotherProtocol { 
// implementation of protocol requirements goes here 
// implementações de requerimentos de protocolo entram aqui
} 
``` 

Adicionar protocolo de conformidade desta forma está descrito em [Adicionando Protocolo de Conformidade com Extensões](guia/protocolos.md#adicionandoprotocolosdeconformidade). 

> NOTA 
> 
> Se você definir uma extensão para adicionar uma nova funcionalidade para um tipo existente, a nova funcionalidade estará disponível para todas as instâncias daquele tipo, mesmo se elas foram criadas antes da extensão ser definida.