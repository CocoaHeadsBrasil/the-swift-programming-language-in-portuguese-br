## Desconstrutores

Um desconstrutor - do original *deinitializer* - é chamado imediatamente antes de uma instância de uma classe ser desalocada. Você escreve desconstrutores com a palavra-chave `deinit`, semelhante à forma como inicializadores são escritos com a palavra-chave `init`. Desconstrutores estão disponíveis apenas em tipos de classes.

### Como Desconstrução Funciona

Swift automaticamente desaloca suas instâncias quando elas não são mais necessárias, para liberar recursos. Swift manuseia o gerenciamento de memória de instâncias através da contagem automática de referência - do original *automatic reference counting (ARC)* - , como descrito em [Contagem Automática de Referência](guia/automatic_reference_couting.md). Em geral, você não precisa realizar limpeza manual quando suas instâncias são desalocadas. Porém, quando você está trabalhando com os seus próprios recursos, você talvez precise realizar alguma limpeza adicional por conta própria. Por exemplo, se você criar uma classe customizada para abrir um arquivo e escrever alguns dados nele, você talvez precise fechá-lo antes da instância da classe ser desalocada.

Definições de classes podem conter no máximo um desconstrutor por classe. O desconstrutor não leva nenhum parâmetro e é escrito sem parênteses.

```swift
deinit {

    // perform the deinitialization
    
}
```

Desconstrutores são chamados automaticamente, logo antes da desalocação da instância entrar em cena. Você não tem permissão para chamar um desconstrutor por conta própria. Desconstrutores de superclasses são herdados por suas subclasses, e os desconstrutores das superclasses são chamados automaticamente ao final da implementação de um desconstrutor de uma subclasse. Desconstrutores de superclasses são sempre chamados, mesmo se a subclasse não fornecer um desconstrutor próprio.

Como uma instância não é desalocada até o final de seu desconstrutor ser chamado, um desconstrutor pode acessar todas as propriedades da instância na qual ele é chamado e pode modificar seu comportamento baseado naquelas propriedades (como visualizar o nome de um arquivo que precisa ser fechado).

### Desconstrutores em Ação

Aqui está um exemplo de um desconstrutor em ação. Este exemplo define dois novos tipos, `Bank` e `Player`, para um jogo simples. A classe `Bank` pode gerenciar a moeda fictícia, que nunca pode ter mais de 10.000 moedas em circulação. Só pode existir um `Bank` no jogo, por isso o `Bank` será implementado como uma classe com propriedades e métodos do tipo para armazenar e gerenciar seu estado atual:

```swift
class Bank {

    static var coinsInBank = 10_000
    
    static func vendCoins(var numberOfCoinsToVend: Int) -> Int {
    
        numberOfCoinsToVend = min(numberOfCoinsToVend, coinsInBank)
        
        coinsInBank -= numberOfCoinsToVend
        
        return numberOfCoinsToVend
    }
    
    static func receiveCoins(coins: Int) {
    
        coinsInBank += coins
        
    }
    
}
```

`Bank` mantém controle do número atual de moedas que ele detém na sua propriedade `coinsInBank`. Ele também oferece  dois métodos—`vendCoins(_:)` e `receiveCoins(_:)`—para manusear a distribuição e coleta de moedas.

`vendCoins(_:)` verifica que há moedas sufucientes no banco antes de distruibí-las. Se houver moedas suficientes, `Bank` retorna um número menor do que o que foi solicitado (e retorna zero caso nenhuma moeda tenha sobrado no banco). `vendCoins(_:)` declara `numberOfCoinsToVend` como um parâmetro variável, para que o número possa ser modificado dentro do corpo do método sem precisar declarar uma nova variável. Ele retorna um valor inteiro para indicar o número atual de moedas que foram fornecidas.

O método `receiveCoins(_:)` simplesmente adiciona o múmero recebido de moedas de volta ao depósito de moedas do banco.

A classe `Player` descreve um jogador no jogo. Cada jogador tem um certo número de moedas armazenadas na carteira em um determinado momento. Isso é representado pela propriedade `coinsInPurse` do jogador:

```swift
class Player {

    var coinsInPurse: Int
    
    init(coins: Int) {
    
        coinsInPurse = Bank.vendCoins(coins)
        
    }
    
    func winCoins(coins: Int) {
    
        coinsInPurse += Bank.vendCoins(coins)
        
    }
    
    deinit {
    
        Bank.receiveCoins(coinsInPurse)
    
     }
     
}
```

Cada instância de `Player` é inicializada com uma permissão inicial para um número específico de moedas do banco durante a inicializacão, apesar de uma instância de `Player` poder receber menos do que aquele número se não houver moedas suficientes disponíveis.

A classe `Player` define um método `winCoins(_:)`, o qual recupera um certo número de moedas do banco e as adiciona à carteira do jogador. A classe `Player`também implementa um desconstrutor, o qual é chamado bem antes de um instância de `Player` ser desalocada. Aqui, o desconstrutor simplesmente retorna todas as moedas do jogador ao banco:

```swift
var playerOne: Player? = Player(coins: 100)

print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")

// prints "A new player has joined the game with 100 coins"

print("There are now \(Bank.coinsInBank) coins left in the bank")

// prints "There are now 9900 coins left in the bank"
```
Uma nova instância de `Player` é criada, com uma solicitação de 100 moedas se elas estiverem disponíveis. Essa instância de `Player` é armazenada em uma variável opcional `Player` chamada `Player`. Uma variável opcional é usada aqui, porque jogadores podem sair do jogo a qualquer momento. A opcional lhe permite controlar a existência de um jogador no jogo.

Como `playerOne` é uma opcional, ele é qualificado com um ponto de exclamação (`!`) quando sua propriedade `coinsInPurse` é acessada para imprimir seu número atual de moedas, e sempre que seu método `winCoins(_:)` é chamado:

```swift
playerOne!.winCoins(2_000)

print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")

// prints "PlayerOne won 2000 coins & now has 2100 coins"

print("The bank now only has \(Bank.coinsInBank) coins left")

// prints "The bank now only has 7900 coins left"
```

Aqui, o jogador ganhou 2.000 moedas. A carteira do jogador agora possui 2.100 moedas, e o banco possui apenas 7.900 moedas sobrando.

```swift
playerOne = nil

print("PlayerOne has left the game")

// prints "PlayerOne has left the game"

print("The bank now has \(Bank.coinsInBank) coins")

// prints "The bank now has 10000 coins"
```

O jogador agora saiu do jogo. Isso é indicado configurando a variável opcional `playerOne` para `nil`, significando "sem instância de `Player`". No momento em que isso acontece, a referência da variável `playerOne` à instância de `Player` é quebrada. Nenhuma outra propriedade ou variável estão se referenciando à instância de `Player`, e por isso ela é desalocada a fim de liberar sua memória. Bem antes disso acontecer, seu desconstrutor é chamado automaticamente, e suas moedas são retornadas ao banco.
