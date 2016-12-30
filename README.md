# AIArtist
AIArtist technique sum up



## lib sum 
* [KLCPopup](https://github.com/jmascia/KLCPopup)   @pop up a view with animation

* [PXAlertView](https://github.com/alexanderjarvis/PXAlertView) alert view that can customs replace systom alertvc






## knowledge sum 
1. 多个地方会调用(公用)的类或者控制器, 比如分享弹窗控制器, 登录控制器, 应在init方法传入来源控制器的信息(source), 可能用于统计.
2. 协议, 可以弥补无法多继承的缺点.  协议即是增加方法, 面向协议(面向接口)开发更加规范.
3. block用于回调, 完成某些代码之后开始执行block, 或者完成某些代码之后回调返回结果(有参或有返回值).
4. 善于利用通知, 一对多的作用. 比如账户信息变化影响很多界面使用通知, 比如很多地方做分享, 分享成功都发送通知(分散功能用通知聚合, 统一处理).
5. 线程安全, 关于数据处理逻辑的代码, 应该新建一个类来管理, 对可能造成线程冲突的属性加锁.
6. 多态的使用, 一个baseClass, 多个继承与baseClass的类, 比如下载类(七牛, 亚马逊)
7. delegate不一定是控制器, 不一定是self, 如果是全局的(比如注册所有分享平台的回调), 可以用一个全局的单例子来用作delegate, 要有"全局观念"!








关键字: @mutating fallthrough
@mutating  关键字用来标记一个会修改结构体的方法
你可以在每个需要该特性的 case 分支中使用 fallthrough 关键字。

日了狗了了印象笔记

///!!!:
结构体和枚举是值类型

类是引用类型


switch  case 后面不用加 break
甚至case可以写比较值值
    let x where (后面的值赋给x)
```swift
switch vegetable {
    case "celery":
print("Add some raisins and make ants on a log.")
    case "cucumber", "watercress":
print("That would make a good tea sandwich.")
    case let x where x.hasSuffix("pepper"):
print("Is it a spicy \(x)?")
```
函数可以带有可变个数的参数，这些参数在函数内表现为数组的形式：
```swift
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(numbers: 42, 597, 12)
```

也可以返回多个值,  使用元组来让一个函数返回多个值
```swift
也可以返回多个值,  使用元组来让一个函数返回多个值
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int,         sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0
    return (min, max, sum)
}
let statistics = calculateStatistics(scores:[5, 3, 100, 3, 9])
    print(statistics.sum)
    print(statistics.2)
sumOf(numbers: 42, 597, 12)
```

函数是第一等类型，这意味着函数可以作为另一个函数的返回值
```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

创建类
```swift
class Square: NamedShape {
    var sideLength: Double
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() ->  Double {
        return sideLength * sideLength
    }
    
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

Swift枚举的枚举比较造孽。就像类和其他所有命名类型一样，枚举可以包含方法
方法是对象的使用
```swift
enum Rank: Int {
    case Ace = 1
    case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
    case Jack, Queen, King
    func simpleDescription() -> String {
        switch self {
        case .Ace:
            return "ace"
        case .Jack:
            return "jack"
        case .Queen:
            return "queen"
        case .King:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.Ace
let aceRawValue = ace.rawValue
```

枚举用于处理服务器返回的数据, 判断对错
```swift
enum ServerResponse {
    case Result(String, String)
    case Failure(String)
}

let success = ServerResponse.Result("6:00 am", "8:09 pm")
let failure = ServerResponse.Failure("Out of cheese.")

switch success {
    case let .Result(sunrise, sunset):
    let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
    case let .Failure(message):
    print("Failure...  \(message)")
}
```

错误处理

使用采用Error协议的类型来表示错误。
```swfit
enum PrinterError: Error {
    case OutOfPaper
    case NoToner
    case OnFire
}
```

使用throw来抛出一个错误并使用throws来表示一个可以抛出错误的函数。如果在函数中抛出一个错误，这个函数会立刻返回并且调用该函数的代码会进行错误处理。
```swfit
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```
有多种方式可以用来进行错误处理。一种方式是使用do-catch。在do代码块中，使用try来标记可以抛出错误的代码。在catch代码块中，除非你另外命名，否则错误会自动命名为error。
```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
```

另一种处理错误的方式使用try?将结果转换为可选的。如果函数抛出错误，该错误会被抛弃并且结果为nil。否则的话，结果会是一个包含函数返回值的可选值。
```swfit
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```
使用defer代码块来表示在函数返回前，函数中最后执行的代码。无论函数是否会抛出错误，这段代码都将执行。使用defer，可以把函数调用之初就要执行的代码和函数调用结束时的扫尾代码写在一起，虽然这两者的执行时机截然不同。
```swfit
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]

func fridgeContains(_ food: String) -> Bool {
    fridgeIsOpen = true
    defer {
        fridgeIsOpen = false
    }
    
    let result = fridgeContent.contains(food)
    return result
}
fridgeContains("banana”)  //false
    print(fridgeIsOpen)  //false
```
    
    ///!!!: 泛型
    在类型名后面使用where来指定对类型的需求，比如，限定类型实现某一个协议，限定两个类型是相同的，或者限定某个类必须有一个特定的父类。
    <T: Equatable>和<T> ... where T: Equatable>是等价的。
```swfit
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
        for lhsItem in lhs {
            for rhsItem in rhs {
                if lhsItem == rhsItem {
                    return true
                }
            }
        }
        return false
}
anyCommonElements([1, 2, 3], [3])
```

#chapter2

print(_:separator:terminator:) 是一个用来输出一个或多个值到适当输出区的全局函数。如果你用 Xcode，print(_:separator:terminator:) 将会输出内容到“console”面板上。separator 和 terminator 参数具有默认值，因此你调用这个函数的时候可以忽略它们。默认情况下，该函数通过添加换行符来结束当前行。如果不想换行，可以传递一个空字符串给 terminator 参数--例如，print(someValue, terminator:"") 。关于参数默认值的更多信息，请参考默认参数值。


Double精确度很高，至少有15位数字，而Float只有6位数字。选择哪个类型取决于你的代码需要处理的值的范围，在两种类型都匹配的情况下，将优先选择 Double。
let pi = 3.14159
// pi 会被推测为 Double 类型
当推断浮点数的类型时，Swift 总是会选择 Double 而不是Float。

数值类字面量可以包括额外的格式来增强可读性。整数和浮点数都可以添加额外的零并且包含下划线，并不会影响字面量：
let oneMillion = 1_000_000

///!!!:
元组（tuples）把多个值组合成一个复合值。元组内的值可以是任意类型，并不要求是相同类型。
    let http404Error = (404, "Not Found”)
    let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)”)
你可以在定义元组的时候给单个元素命名：
    let http200Status = (statusCode: 200, description: "OK”) //http200Status.statusCode
    作为函数返回值时，元组非常有用。一个用来获取网页的函数可能会返回一个 (Int, String) 元组来描述是否获取成功。和只能返回一个类型的值比较起来，一个包含两个不同类型值的元组可以让函数的返回信息更有用。
    
    
错误处理
```swfit
func canThrowAnError() throws {
    // 这个函数有可能抛出错误
}
do {
    try canThrowAnError()
    // 没有错误消息抛出
} catch {
    // 有一个错误消息抛出
}
```
断言
let age = -3
assert(age >= 0, "A person's age cannot be less than zero")
// 因为 age < 0，所以断言会触发 终止程序

检测当前平台
if #available(iOS 10, macOS 10.12, *) {
    // 在 iOS 使用 iOS 10 的 API, 在 macOS 使用 macOS 10.12 的 API
} else {
    // 使用先前版本的 iOS 和 macOS 的 API
}

指定参数标签, 可读性更强  也可以用_代替
func someFunction(argumentLabel parameterName: Int) {
    // 在函数体内，parameterName 代表参数值
}

默认参数值
```swfit
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // 如果你在调用时候不传第二个参数，parameterWithDefault 会值为 12 传入到函数体中。
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6)
someFunction(parameterWithoutDefault: 4) // parameterWithDefault = 12
```
可变参数
```swfit
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
```
输入输出参数
函数参数默认是常量。试图在函数体中更改参数值将会导致编译错误(compile-time error)。这意味着你不能错误地更改参数值。如果你想要一个函数可以修改参数的值，并且想要在这些修改在函数调用结束后仍然存在，那么就应该把这个参数定义为输入输出参数（In-Out Parameters）。
定义一个输入输出参数时，在参数定义前加 inout 关键字。
你只能传递变量给输入输出参数。你不能传入常量或者字面量，因为这些量是不能被修改的。当传入的参数作为输入输出参数时，需要在参数名前加 & 符，表示这个值可以被函数修改。
```swfit
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
swapTwoInts(&someInt, &anotherInt)  // 不能传入常量
```
函数嵌套函数
```swfit
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
```

逃逸闭包
当一个闭包作为参数传到一个函数中，但是这个闭包在函数返回之后才被执行，我们称该闭包从函数中逃逸。当你定义接受闭包作为参数的函数时，你可以在参数名之前标注 @escaping，用来指明这个闭包是允许“逃逸”出这个函数的。
一种能使闭包“逃逸”出函数的方法是，将这个闭包保存在一个函数外部定义的变量中。举个例子，很多启动异步操作的函数接受一个闭包参数作为 completion handler。这类函数会在异步操作开始之后立刻返回，但是闭包直到异步操作结束后才会被调用。在这种情况下，闭包需要“逃逸”出函数，因为闭包需要在函数返回之后被调用。例如：

```swfit
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
someFunctionWithEscapingClosure(_:) 函数接受一个闭包作为参数，该闭包被添加到一个函数外定义的数组中。如果你不将这个参数标记为 @escaping，就会得到一个编译错误。
```

Switch 大招
在 Swift 中，使用如下方式定义表示两种商品条形码的枚举：
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
以上代码可以这么理解：
“定义一个名为Barcode的枚举类型，它的一个成员值是具有(Int，Int，Int，Int)类型关联值的upc，另一个成员值是具有String类型关联值的qrCode。”
这个定义不提供任何Int或String类型的关联值，它只是定义了，当Barcode常量和变量等于Barcode.upc或Barcode.qrCode时，可以存储的关联值的类型。
然后可以使用任意一种条形码类型创建新的条形码，例如：
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
上面的例子创建了一个名为productBarcode的变量，并将Barcode.upc赋值给它，关联的元组值为(8, 85909, 51226, 3)。
同一个商品可以被分配一个不同类型的条形码，例如：
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
这时，原始的Barcode.upc和其整数关联值被新的Barcode.qrCode和其字符串关联值所替代。Barcode类型的常量和变量可以存储一个.upc或者一个.qrCode（连同它们的关联值），但是在同一时间只能存储这两个值中的一个。
像先前那样，可以使用一个 switch 语句来检查不同的条形码类型。然而，这一次，关联值可以被提取出来作为 switch 语句的一部分。你可以在switch的 case 分支代码中提取每个关联值作为一个常量（用let前缀）或者作为一个变量（用var前缀）来使用：
```swfit
    switch productBarcode {
    case .upc(let numberSystem, let manufacturer, let product, let check):
        print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
    case .qrCode(let productCode):
        print("QR code: \(productCode).")
    }
    // 打印 "QR code: ABCDEFGHIJKLMNOP."
```
    
    递归枚举
    递归枚举是一种枚举类型，它有一个或多个枚举成员使用该枚举类型的实例作为关联值。使用递归枚举时，编译器会插入一个间接层。你可以在枚举成员前加上indirect来表示该成员可递归。
    例如，下面的例子中，枚举类型存储了简单的算术表达式：
    
```swfit
    enum ArithmeticExpression {
        case number(Int)
        indirect case addition(ArithmeticExpression, ArithmeticExpression)
        indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
    }
    你也可以在枚举类型开头加上indirect关键字来表明它的所有成员都是可递归的：
    indirect enum ArithmeticExpression {
        case number(Int)
        case addition(ArithmeticExpression, ArithmeticExpression)
        case multiplication(ArithmeticExpression, ArithmeticExpression)
    }
    
    运用这两个运算符检测两个常量或者变量是否引用同一个实例：
    if tenEighty === alsoTenEighty {
        print("tenEighty and alsoTenEighty refer to the same Resolution instance.")
    }
```
    字符串、数组、和字典类型的赋值与复制行为
    Swift 中，许多基本类型，诸如String，Array和Dictionary类型均以结构体的形式实现。这意味着被赋值给新的常量或变量，或者被传入函数或方法中时，它们的值会被拷贝。
    Objective-C 中NSString，NSArray和NSDictionary类型均以类的形式实现，而并非结构体。它们在被赋值或者被传入函数或方法时，不会发生值拷贝，而是传递现有实例的引用。
