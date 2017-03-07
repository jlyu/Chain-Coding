https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2

blog:
http://onevcat.com/2014/06/walk-in-swift
http://blog.txx.im
http://top.jobbole.com/1012/

String / Character
String 不等于 OC 中的 NSString
支持以任意 Unicode 作为变量名，意味着我们以后可以使用中文、颜文字等进行声明。
一个目前能够想到的好处是，利用颜文字直观的表现形式来代替抽象的文字(?) 比如：

Collections
Array 和 Dictionary 同为 Structure，坑的地方在于：两者复制行为不一致。

Functions
 Function Types 的概念。Swift 中可以将返回类型放到了形参后面的写法，如 (Int, String) -> Bool 构成了 Function Types
这样就可以将某个 function 传给某个带 function type 的 variable。 var newFunction: (Int, String) -> Bool = someFunction ←坑 无法应用在 Closure
 Variadic Parameters 的概念，不能声明 inout 属性。func sumOfArray(ins: Int...) -> Int { } 当形参类型如为 Int ... 表示可传入 Int[]
调用时 sumOfArray(1,2,3,4) // 目前还未实现，以sumOfArray([1,2,3,4]) 代替。如下
func sumArray (numbers : Int[])  -> Int { ... }
func sumOf    (numbers : Int...) -> Int { sumArray (numbers) }
 External parameter name 倒是比较便利可读的用法，配合 # 使用更佳。
function 既可以作为形参传入，又可以作为返回类型传出。同时 Nested function 也OK，支持 Closure 
 Trailing Closures 

Class / Struct
没有了 .h 头文件及 .m 实现文件，全由 .swift 单文件组织
现阶段不具备 acess control mechanism，将来会有。
class 与 struct 的区别 
     1. struct 不能继承自别的 struct
     2. 值的传递方式：前者传 Refrence， 后者传 Value（因此回到Collections，Dictionary是传 Value，Array 内部实现成传 Refernce）
          因此即使 let 一个 class 的 instance， let window = Window(frame: frame) 也可以改变其 property ，window.title = "hello" ，但是无法改变引用的对象 window = Window(...) // constant ref
新声明的 class 不再需要从 NSObject 继承
Struct 的 Method 都是 Read-only， 因此在修改成员变量时声明成 mutating 
Type Method （静态方法？）Class 用 class 修饰，Struct 使用 static 修饰
Property Observers 对成员变量值变化的前一刻及改版后一款的 observer
     willSet { // newValue is available here }
     didSet { // oldValue is available here }



! 和 ?
当变量 ? 后，得到的其实还是 Optional Value 但这时候可以用 nil 进行判断了 
抱着必死决心  ! unwrap后才会得到原来实际的类型，也就是拿到 Optional 内部值
因此这里可能出现的一个坑就是 if 条件判断
if 只接受 true 和 false 这2种 Bool 类型，非(Optional<T>.Some true)
所以无法像以前一样
var myName: String = "chain"
if myName {...} // "Type 'String' does not conform to protocol 'LogicValue'"
但可以使用 if optional variable 来判断是否 nil
从图中代码的运行情况来看，得到预期相反的结果。
不负责任的推断 if (Optional<T>.Some V) 那么总是 true。当然 if(None) 是 false (通过 'LogicValue' protocol)
? 和 if 的组合使用称为 optional binding
optional banding 的用法，就是把 optional<T> 脱掉
t 回归到最初始的 Bool 类型 true
当需要多处链环 optional banding 的业务逻辑（if多层嵌套）时，引出 Optional Chaining 的概念。
同一条链上东西，只要其中一环是 nil，那么整条链就会返回 nil


隐式类型转换
extension Point {
     @conversion func __conversion() -> (Int, Int) {
          return (x,y)
     }
}
隐式类型转换标准： @conversion func __conversion<T>() -> T {　}




var 声明变量
let 声明常量  let myConstantValue: Double = 70.0
不支持隐式类型转换（Implicitly casting）
a let array is completely immutable, and a var array is completely mutable.

类型别名

	* typealias AudioSample = UInt16



元组
(Int, Double, _) = HTTP404Error
访问元素：HTTP404Error.0 ; HTTP404Error.1

声明array 和 Dictory 均使用[]
var myArray = ["1", "2", "3"]
let emptyArray = String()[]
let emptyDictionary = Dictionary<String, Float>()

使用 if let 组合判定常量
if let name = optionalName {
     ... }

声明 function
func greet(name: String, day: String) -> String {
     return "Hello, \(name), today is \(day)."
}
greet("Bob", "Tuesday")

返回多个值
func getGasPrices() -> (Double, Double, Double) {
     return (3.13, 3.14, 3.15)
}

函数嵌套
 func makeIncrementer() -> (Int -> Int) {
     func addOne(number: Int) -> Int {
         return 1 + number
     }
     return addOne
 }
 var increment = makeIncrementer()
 increment(7)



