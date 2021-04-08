# 数据类型 

JS 中有六种简单数据类型：undefined、null、boolean、string、number、symbol、BigInt，以及引用类型：object



<img src="C:\SAPShare\Chainnotes\Assets\image-20210407151222226.png" alt="image-20210407151222226" style="zoom:80%;" />

- `基础类型`存储在栈内存，被引用或拷贝时，会创建一个完全相等的变量；

- `引用类型`存储在堆内存，存储的是地址，多个引用指向同一个地址，这里会涉及一个“共享”的概念。

## 数据类型检测

- 第一种判断方法：`typeof`

- 第二种判断方法：`instanceof`

  instanceof 可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型；

  而 typeof 也存在弊端，它虽然可以判断基础数据类型（null 除外），但是引用数据类型中，除了 function 类型以外，其他的也无法判断。

- 第三种判断方法：`Object.prototype.toString`

```
Object.prototype.toString({})       // "[object Object]"
Object.prototype.toString.call({})  // 同上结果，加上call也ok
Object.prototype.toString.call(1)    // "[object Number]"
Object.prototype.toString.call('1')  // "[object String]"
Object.prototype.toString.call(true)  // "[object Boolean]"
Object.prototype.toString.call(function(){})  // "[object Function]"
Object.prototype.toString.call(null)   //"[object Null]"
Object.prototype.toString.call(undefined) //"[object Undefined]"
Object.prototype.toString.call(/123/g)    //"[object RegExp]"
Object.prototype.toString.call(new Date()) //"[object Date]"
Object.prototype.toString.call([])       //"[object Array]"
Object.prototype.toString.call(document)  //"[object HTMLDocument]"
Object.prototype.toString.call(window)   //"[object Window]"

// 最后返回统一字符串格式为 "[object Xxx]" ，而这里字符串里面的 "Xxx" ，第一个首字母要大写（注意：使用 typeof 返回的是小写）
```

## 数据类型转换

强制转换（显示转换）包括 Number()、parseInt()、parseFloat()、toString()、String()、Boolean()
自动转换（隐式转换）

- 比较运算（`==`、`!=`、`>`、`<`）、`if`、`while`需要布尔值地方
- 算术运算（`+`、`-`、`*`、`/`、`%`）
- 逻辑运算 (`&&`、 `||`、 `!`)

除了上面的场景，还要求运算符两边的操作数不是同一类型

在+运算中，一旦存在字符串，则会进行字符串拼接操作，除了+有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值

