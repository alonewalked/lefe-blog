#继承
---
一个类可以从另一个类继承方法、属性和其他的特性。当一个类从另一个类继承的时候，继承的类就是所谓的子类，而这个类继承的类被称为父类
###定义一个基类
	class Vehicle {
	    var currentSpeed = 0.0
	    var description: String {
	        return "traveling at \(currentSpeed) miles per hour"
	    }
	    func makeNoise() {
	        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
	    }
	}
	
	let someVehicle = Vehicle()
	print("Vehicle: \(someVehicle.description)")
	// Vehicle: traveling at 0.0 miles per hour
###子类
	class Bicycle: Vehicle {
	    var hasBasket = false
	}
	
	let bicycle = Bicycle()
	bicycle.hasBasket = true
	
	bicycle.currentSpeed = 15.0
	print("Bicycle: \(bicycle.description)")
	// Bicycle: traveling at 15.0 miles per hour
子类本身也可以被继承

	class Tandem: Bicycle {
	    var currentNumberOfPassengers = 0
	}
	
	let tandem = Tandem()
	tandem.hasBasket = true
	tandem.currentNumberOfPassengers = 2
	tandem.currentSpeed = 22.0
	print("Tandem: \(tandem.description)")
	// Tandem: traveling at 22.0 miles per hour
###重写
在重写定义前面加上 override 关键字，override 关键字会执行 Swift 编译器检查你重写的类的父类(或者父类的父类)是否有与之匹配的声明来供你重写
####重写方法
	class Train: Vehicle {
	    override func makeNoise() {
	        print("Choo Choo")
	    }
	}
	
	let train = Train()
	train.makeNoise()
	// prints "Choo Choo"
####重写属性
	class Car: Vehicle {
	    var gear = 1
	    override var description: String {
	        return super.description + " in gear \(gear)"
	    }
	}

	let car = Car()
	car.currentSpeed = 25.0
	car.gear = 3
	print("Car: \(car.description)")
	// Car: traveling at 25.0 miles per hour in gear 3
####重写属性观察器
	class AutomaticCar: Car {
	    override var currentSpeed: Double {
	        didSet {
	            gear = Int(currentSpeed / 10.0) + 1
	        }
	    }
	}
	
	let automatic = AutomaticCar()
	automatic.currentSpeed = 35.0
	print("AutomaticCar: \(automatic.description)")
	// AutomaticCar: traveling at 35.0 miles per hour in gear 4
###阻止重写
- 阻止属性： final var
- 阻止方法： final func
- 阻止下标： final subscript
- 类不能被继承： final class