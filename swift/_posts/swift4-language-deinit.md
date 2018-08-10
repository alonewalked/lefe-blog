---
title: swift4-0起步系列-0x02-反初始化
date: 2018-08-01 11:03:01
tags: 
- swift
- ios
categories: 
- IOS-Swift4
---

- 当实例不再被需要的时候 Swift会自动将其释放掉，以节省资源
- 当你的实例被释放时，你不需要手动清除它们
- 每个类当中只能有一个反初始化器。反初始化器不接收任何形式参数，并且不需要写圆括号

```
deinit {
    // perform the deinitialization
}
```
	
- 反初始化器会在实例被释放之前自动被调用。你不能自行调用反初始化器。父类的反初始化器可以被子类继承，并且子类的反初始化器实现结束之后父类的反初始化器会被调用。父类的反初始化器总会被调用，就算子类没有反初始化器	
- 由于实例在反初始化器被调用之前都不会被释放，反初始化器可以访问实例中的所有属性并且可以基于这些属性修改自身行为

		class Bank {
		    static var coinsInBank = 10_000
		    static func distribute(coins numberOfCoinsRequested: Int) -> Int {
		        let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
		        coinsInBank -= numberOfCoinsToVend
		        return numberOfCoinsToVend
		    }
		    static func receive(coins: Int) {
		        coinsInBank += coins
		    }
		}
		
		class Player {
		    var coinsInPurse: Int
		    init(coins: Int) {
		        coinsInPurse = Bank.distribute(coins: coins)
		    }
		    func win(coins: Int) {
		        coinsInPurse += Bank.distribute(coins: coins)
		    }
		    deinit {
		        Bank.receive(coins: coinsInPurse)
		    }
		}
		
		var playerOne: Player? = Player(coins: 100)
		print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
		// Prints "A new player has joined the game with 100 coins"
		print("There are now \(Bank.coinsInBank) coins left in the bank")
		// Prints "There are now 9900 coins left in the bank"
		
		playerOne!.win(coins: 2_000)
		print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
		// Prints "PlayerOne won 2000 coins & now has 2100 coins"
		print("The bank now only has \(Bank.coinsInBank) coins left")
		// Prints "The bank now only has 7900 coins left"
		
		playerOne = nil
		print("PlayerOne has left the game")
		// prints "PlayerOne has left the game"
		print("The bank now has \(Bank.coinsInBank) coins")
		// prints "The bank now has 10000 coins"