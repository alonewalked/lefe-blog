#方法
---
###实例方法
属于特定类实例、结构体实例或者枚举实例的函数

	class Counter {
	    var count = 0
	    func increment() {
	        count += 1
	    }
	    func increment(by amount: Int) {
	        count += amount
	    }
	    func reset() {
	        count = 0
	    }
	}
	let counter = Counter() // the initial counter value is 0
	counter.increment() // the counter's value is now 1
	counter.increment(by: 5) // the counter's value is now 6
	counter.reset() // the counter's value is now 0

###self 属性
完完全全与实例本身相等

	struct Point {
	    var x = 0.0, y = 0.0
	    func isToTheRightOf(x: Double) -> Bool {
	        return self.x > x
	    }
	}
	let somePoint = Point(x: 4.0, y: 5.0)
	if somePoint.isToTheRightOf(x: 1.0) {
	    print("This point is to the right of the line where x == 1.0")
	}
	// Prints "This point is to the right of the line where x == 1.0"
	
###在实例方法中修改值类型
结构体和枚举是值类型。默认情况下，值类型属性不能被自身的实例方法修改，你可以选择在 func关键字前放一个 mutating关键字来使用这个行为

    struct Point {
	    var x = 0.0, y = 0.0
	    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
	        x += deltaX
	        y += deltaY
	    }
	}
	var somePoint = Point(x: 1.0, y: 1.0)
	somePoint.moveBy(x: 2.0, y: 3.0)
	print("The point is now at (\(somePoint.x), \(somePoint.y))")
	// prints "The point is now at (3.0, 4.0)"
	
	let fixedPoint = Point(x: 3.0, y: 3.0)
	fixedPoint.moveBy(x: 2.0, y: 3.0)
	// this will report an error

###在异变方法里指定自身
异变方法可以指定整个实例给隐含的 self属性

	struct Point {
	    var x = 0.0, y = 0.0
	    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
	        self = Point(x: x + deltaX, y: y + deltaY)
	    }
	}
	
枚举的异变方法可以设置隐含的 self属性为相同枚举里的不同成员
	
	enum TriStateSwitch {
	    case off, low, high
	    mutating func next() {
	        switch self {
	        case .off:
	            self = .low
	        case .low:
	            self = .high
	        case .high:
	            self = .off
	        }
	    }
	}
	var ovenLight = TriStateSwitch.low
	ovenLight.next()
	// ovenLight is now equal to .high
	ovenLight.next()
	// ovenLight is now equal to .off

###类型方法
通过在 func关键字之前使用 static关键字来明确一个类型方法。类同样可以使用 class关键字来允许子类重写父类对类型方法的实现	

	struct LevelTracker {
	    static var highestUnlockedLevel = 1
	    var currentLevel = 1
	    
	    static func unlock(_ level: Int) {
	        if level > highestUnlockedLevel { highestUnlockedLevel = level }
	    }
	    
	    static func isUnlocked(_ level: Int) -> Bool {
	        return level <= highestUnlockedLevel
	    }
	    
	    @discardableResult
	    mutating func advance(to level: Int) -> Bool {
	        if LevelTracker.isUnlocked(level) {
	            currentLevel = level
	            return true
	        } else {
	            return false
	        }
	    }
	}

	class Player {
	    var tracker = LevelTracker()
	    let playerName: String
	    func complete(level: Int) {
	        LevelTracker.unlock(level + 1)
	        tracker.advance(to: level + 1)
	    }
	    init(name: String) {
	        playerName = name
	    }
	}
	
	var player = Player(name: "Argyrios")
	player.complete(level: 1)
	print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
	// Prints "highest unlocked level is now 2"
	
	player = Player(name: "Beto")
	if player.tracker.advance(to: 6) {
	    print("player is now on level 6")
	} else {
	    print("level 6 has not yet been unlocked")
	}
	// Prints "level 6 has not yet been unlocked"