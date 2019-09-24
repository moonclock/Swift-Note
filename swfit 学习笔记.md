# **swfit** 学习笔记

## 一 数值类型

1.类型别名 typealias, 为一个已知类型定义一个别名, typealias AudioSample = UInt8

2.在判断类型 ,布尔类型必须明确(if i == ture, if i 是无效的)

3.元组在初始化的时候可以进行类型指定 var error:(errorCode: Int, errorMessage:Any) = (errorCode: 1, errorMeassage: “没有权限”)

4.字符串中利用indices属性可以获得所有字符的索引值




##二 运算符

1.0 != nil

2.合并控制运算符(??) 相当于三元运算符作用于Optional的缩写 a!=nil ? a! : b

3.运算符是可以重载的, 分为:

- 二元运算重载, 如加减法

- 一元运算重载, 如正负号, 定义时, 使用 prefix 声明它为前缀

- 组合运算符重载, += 需要在左参数需要为 inout 类型

```swift
extension Vector2D {
    static prefix func -(vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y);
    }

    static func +(left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
    
    static func +=(left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

- 等价运算符必须遵守 Equatable 协议(存储型类的结构体,  枚举会自动合成实现)

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

4.自定义运算符



## 流程控制

1. forin 循环
   - 字典的forin循环, 可以直接获得元组
   ```swift
   let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
   for (animalName, legCount) in numberOfLegs {
    print("\(animalName) has \(legCount) legs")
    }
   ```

   
   
   - 不需要的值可以直接用 _ 去忽略
   ```swift
   for _ in numberOfLegs {
    print(numberOfLegs)
    }
   ```
   
2. switch

   - 在匹配到第一个case, 执行完毕后退出, 不需要显式的指明break. 如果想要C 风格的贯穿行为, 需要在该 case 后面使用 fallthrough (会按顺序继续运行，且不论条件是否满足都会执行。)

   - 一个case中匹配多个值可以用 逗号 分隔, 并且可以写出多行

   - case 中支持区间匹配  ( case 6...12: )

   - 支持值绑定, 并且支持 where

     ```swift
     let point = (1, -1)
     
     switch point {
     case(let x, let y) where x == y:
         print("point x = y")
     case(let x, let y) where x == -y:
         print("point x = -y")
     default:
         print("other")
     }
     ```

3. guard

   guard 总是有个 else, 执行顺序如图:

   ![image-20190923130217953](/Users/john/Desktop/swift/Swift-Note/img/image-20190923130217953.png)




## 模式匹配

1.表达式模式

只出现在 switch 的 case 标签中, 

2.类型转换模式

```swift

protocol Animal {
    var name: String { get }
}

struct Dog: Animal {
    var name: String {
        return "dog"
    }
    
    var runSpeed: Int
}

struct Fish: Animal {
    var name: String {
        return "fish"
    }
    
    var depth : Int
}

let animals: [Any] = [Dog(runSpeed: 20), Fish(depth: 800), Fish(depth: 20)]

for animal in animals {
    switch animal {
        
    case let dog as Dog:
        print("\(dog.name) run at \(dog.runSpeed)")
        
    case is Fish:
        print("Fish")
        
    default:
        break
    }
}
```

