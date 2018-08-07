#下标
---
类、结构体和枚举可以定义下标，它可以作为访问集合、列表或序列成员元素的快捷方式
###下标的语法

	subscript(index: Int) -> Int {
	    get {
	        // return an appropriate subscript value here
	    }
	    set(newValue) {
	        // perform a suitable setting action here
	    }
	}
	
	subscript(index: Int) -> Int {
	    // return an appropriate subscript value here
	}
下面是一个只读下标实现的栗子

	struct TimesTable {
	    let multiplier: Int
	    subscript(index: Int) -> Int {
	        return multiplier * index
	    }
	}
	let threeTimesTable = TimesTable(multiplier: 3)
	print("six times three is \(threeTimesTable[6])")
	// prints "six times three is 18"	
###下标用法

	var numberOfLegs = ["spider": 8， "ant": 6， "cat": 4]
	numberOfLegs["bird"] = 2
###下标选项

	struct Matrix {
	    let rows: Int, columns: Int
	    var grid: [Double]
	    init(rows: Int, columns: Int) {
	        self.rows = rows
	        self.columns = columns
	        grid = Array(repeating: 0.0, count: rows * columns)
	    }
	    func indexIsValid(row: Int, column: Int) -> Bool {
	        return row >= 0 && row < rows && column >= 0 && column < columns
	    }
	    subscript(row: Int, column: Int) -> Double {
	        get {
	            assert(indexIsValid(row: row, column: column), "Index out of range")
	            return grid[(row * columns) + column]
	        }
	        set {
	            assert(indexIsValid(row: row, column: column), "Index out of range")
	            grid[(row * columns) + column] = newValue
	        }
	    }
	}	
	
	var matrix = Matrix(rows: 2, columns: 2)
	matrix[0, 1] = 1.5
	matrix[1, 0] = 3.2