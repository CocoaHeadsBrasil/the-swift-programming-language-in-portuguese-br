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
// implementações de requerimentos de protocolo entram aqui
} 
``` 

Adicionar protocolo de conformidade desta forma está descrito em [Adicionando Protocolo de Conformidade com Extensões](guia/protocolos.md#adicionandoprotocolosdeconformidade). 

> NOTA 
> 
> Se você definir uma extensão para adicionar uma nova funcionalidade para um tipo existente, a nova funcionalidade estará disponível para todas as instâncias daquele tipo, mesmo se elas foram criadas antes da extensão ser definida.

### Propriedades Computadas 

Extensões podem adicionar propriedades de instância computada e propriedades de tipo computado para tipos existentes. Este exemplo adiciona cinco propriedades de instância computada para o tipo `Double` do Swift, para fornecer suporte básico para trabalhar com unidades de distância: 

```swift 
extension Double { 
var km: Double { return self * 1_000.0 } 
var m: Double { return self } 
var cm: Double { return self / 100.0 } 
var mm: Double { return self / 1_000.0 } 
var ft: Double { return self / 3.28084 } 
} 
let oneInch = 25.4.mm 
print("Um centímetro é \(oneInch) metros") 
// imprimi "Um centímetro são 0.0254 metros" 
let threeFeet = 3.ft 
print("Três centímetros são \(threeFeet) metros") 
// imprimi "Três centímetros são 0.914399970739201 metros 
``` 
Estas propriedades computadas expressão que um tipo `Double` deve ser considerado como um tipo de tamanho de unidade. Embora elas sejam implementadas como propriedades computadas, os nomes dessas propriedades podem ser anexadas para um valor literal `floating-point` com a sintaxe de ponto, como uma maneira de usar aquele valor literal para realizar conversões de distância. 

Neste exemplo, um valor `Double` de 1.0 é considerado representando "um metro". É por isso que a propriedade computada `m` retorna a auto-expressão, 1.m é considerada para calcular um valor `Double` de 1.0. 

Outras unidades requerem alguma conversão para serem expressadas como um valor medido em metros. Um quilometro é o mesmo que 1,00 metros, então a propriedade computada `km` multiplica o valor por 1_000.00 para converter em um número expressado em metros. Da mesma forma, existem 3.28084 pés em um metro, e assim a propriedade computada `ft` divide o valor `Double` subjacente por 3.28084, para convertê-lo de pés para metros. 

Essas propriedades são propriedades computadas de apenas leitura, e assim elas são expressadas sem a palavra-chave `get`, por questões de abreviatura. Seus valores de retorno são do tipo `Double`, e podem ser usadas dentro de um calculo matemático onde quer que um `Double` for aceito: 

```swift 
let aMarathon = 42.km + 195.m 
print("Uma maratona são \(aMarathon) metros de comprimento") 
// imprimi "Uma maratona são 42195.0 metros de comprimento 
``` 

> NOTA 
> 
> Extensões podem adicionar novas propriedades computadas, mas elas não podem adicionar propriedades armazenadas, ou adicionar observadores de propriedade para uma propriedade existente.