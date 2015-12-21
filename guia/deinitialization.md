## Desconstrutores

### First Chapter

Um desconstrutor - do original *deinitializer* - é chamado imediatamente antes de uma instância de uma classe ser desalocada. Você escreve desconstrutores com a palavra-chave `deinit`, semelhante à forma como inicializadores são escritos com a palavra-chave `init`. Desconstrutores estão apenas disponíveis em tipos de classes.


### Como Desconstrução Funciona

Swift automaticamente desaloca suas instâncias quando elas não são mais necessárias, para liberar recursos. Swift manuseia o gerenciamento de memória de instâncias através da contagem automática de referência - do original *automatic reference counting (ARC)* - , como descrito em [Contagem Automática de Referência](chapter15.md). Tipicamente você não precisa realizar limpeza manual quando suas instâncias são desalocadas. Porém, quando você está trabalhando com os seus próprios recursos, você talvez precise realizar alguma limpeza adicional por conta própria. Por exemplo, se você criar uma classe customizada para abrir um arquivo e escrever alguns dados nele, você talvez precise fechá-lo antes da instância da classe ser desalocada.

Definições de classes podem conter no máximo um desconstrutor por classe. O desconstrutor não leva nenhum parâmetro e é escrito sem parênteses.

```swift
deinit {

    // perform the deinitialization
    
}
```

Desconstrutores são chamados automaticamente, bem antes da desalocação da instância entrar em cena. Você não tem permissão para chamar um desconstrutor por conta própria. Desconstrutores de superclasses são herdados por suas subclasses, e os desconstrutores das superclasses são chamados automaticamente ao final da implementação de um desconstrutor de uma subclasse. Desconstrutores de superclasses são sempre chamados, mesmo se a subclasse não fornecer um desconstrutor próprio.

Como uma instância não é desalocada até depois de seu desconstrutor ser chamado, um desconstrutor pode acessar todas as propriedades da instância na qual ele é chamado e pode modificar seu comportamento baseado naquelas propriedades (como visualizar o nome de um arquivo que precisa ser fechado).

### Desconstrutores em Ação

Aqui está um exemplo de um desconstrutor em ação. Este exemplo define dois novos tipos, `Banco` e `Jogador`, para um jogo simples. A classe `Banco` pode gerênciar a moeda fictícia, que nunca pode ter mais de 10.000 moedas em circulação. Só pode existir um 'Banco' no jogo, por isso o `Banco` será implementado como uma classe com propriedades e métodos do tipo para armazenar e gerênciar seu estado atual:

```swift
class Banco {

    static var moedasNoBanco = 10_000
    
    static func retirarMoedas(var númeroDeMoedasParaRetirar: Int) -> Int {
        
        númeroDeMoedasParaRetirar = min(númeroDeMoedasParaRetirar, moedasNoBanco)
        
        moedasNoBanco -= númeroDeMoedasParaRetirar
        
        return númeroDeMoedasParaRetirar
    }
    
    static func receberMoedas(moedas: Int) {
    
        moedasNoBanco += moedas
        
    }
    
}
```

`Banco` mantém controle do número atual de moedas que ele detém na sua propriedade `moedasNoBanco`. Ele também oferece  dois métodos—`retirarMoedas(_:)` e `receberMoedas(_:)`—para manusear a distribuição e coleta de moedas.

`retirarMoedas(_:)` verifica que há moedas sufucientes no banco antes de distruibí-las. Se houver moedas suficientes, `Banco` retorna um número menor do que o que foi solicitado (e retorna zero caso nenhuma moeda tenha sobrado no banco). `retirarMoedas(_:)` declara `númeroDeMoedasParaRetirar` como um parâmetro variável, para que o número possa ser modificado dentro do corpo do método sem precisar declarar uma nova variável. Ele retorna um valor inteiro para indicar o número atual de moedas que foram fornecidas.

O método `receberMoedas(_:)` simplesmente adiciona o múmero recebido de moedas de volta ao depósito de moedas do banco.

A classe `Jogador` descreve um jogador no jogo. Cada jogador tem um certo número de moedas armazenadas na carteira em um determinado momento. Isso é representado pela propriedade `moedasNaCarteira` do jogador:

```swift
class Jogador {

    var moedasNaCarteira: Int
    
    init(moedas: Int) {
    
        moedasNaCarteira = Banco.retirarMoedas(moedas)
        
    }
    
    func ganharMoedas(moedas: Int) {
    
        moedasNaCarteira += Banco.retirarMoedas(moedas)
        
    }
    
    deinit {
    
        Banco.receberMoedas(moedasNaCarteira)
        
    }
    
}
```

Cada isntância de `Jogador` é inicializada com uma permissão inicial para um número específico de moedas do banco durante a inicializacão, apesar de uma instância de `Jogador` poder receber menos do que aquele número se não houver moedas suficientes disponíveis.

A classe `Jogador` define um método `ganharMoedas(_:)`, o qual recupera um certo número de moedas do banco e as adiciona à carteira do jogador. A classe `Jogador`também implementa um desconstrutor, o qual é chamado bem antes de um instância de `Jogador` ser desalocada. Aqui, o desconstrutor simplesmente retorna todas as moedas do jogador ao banco:

```swift
var jogadorUm: Jogador? = Jogador(moedas: 100)

print("Um novo jogador se juntou ao jogo com \(jogadorUm!.moedasNaCarteira) moedas")

// imprime "Um novo jogador se juntou ao jogo com 100 moedas"

print("Existem agora \(Banco.moedasNoBanco) moedas sobrando no banco")

// imprime "Existem agora 9900 moedas sobrando no banco"

```
Uma nova instância de `Jogador` é criada, com uma solicitação de 100 moedas se elas estiverem disponíveis. Essa instância de `Jogador` é armazenada em uma variável opcional `Jogador` chamada `jogadorUm`. Uma variável opcional é usada aqui, porque jogadores podem sair do jogo a qualquer momento. A opcional lhe permite controlar a existência de um jogador no jogo.

Como `jogadorUm` é uma opcional, ele é qualificado com um ponto de exclamação (!) quando sua propriedade `moedasNaCarteira` é acessada para imprimir seu número atual de moedas, e sempre que seu método `ganharMoedas(_:)` é chamado:

```swift
jogadorUm!.ganharMoedas(2_000)

print("JogadorUm ganhou 2000 moedas & agora possui \(jogadorUm!.moedasNaCarteira) moedas")

// imprime "JogadorUm ganhou 2000 moedas & agora possui 2100 moedas"

print("O banco agora possui apenas \(Banco.moedasNoBanco) moedas sobrando")

// imprime "O banco agora possui apenas 7900 moedas sobrando"
```

Aqui, o jogador ganhou 2.000 moedas. A carteira do jogador agora possui 2.100 moedas, e o banco possui apenas 7.900 moedas sobrando.

```swift
jogadorUm = nil

print("JogadorUm saiu do jogo")

// imprime "JogadorUm saiu do jogo"

print("O banco possui agora \(Banco.moedasNoBanco) moedas")

// imprime "O banco possui agora 10000 moedas"
```

O jogador agora saiu do jogo. Isso é indicado configurando a variável opcional `jogadorUm` para `nil`, significando "sem instância de `Jogador`". No momento em que isso acontece, a referência da variável `jogadorUm` à instância de `Jogador` é quebrada. Nenhuma outra propriedade ou variável estão se referenciando à instância de `Jogador`, e por isso ela é desalocada a fim de liberar sua memória. Bem antes disso acontecer, seu desconstrutor é chamado automaticamente, e suas moedas são retornadas ao banco.
