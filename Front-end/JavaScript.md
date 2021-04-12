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



# 深浅拷贝

## 浅拷贝的原理和实现

> 创建一个新的对象，来接受要重新复制或引用的对象值。如果对象属性是`基本的数据类型`，复制的就是基本类型的`值`给新对象；但如果属性是`引用数据类型`，复制的就是`内存中的地址`，如果其中一个对象改变了这个内存中的地址，肯定会影响到另一个对象。

### 方法一：object.assign

object.assign 是 ES6 中 object 的一个方法，该方法可以用于 JS 对象的合并等多个用途，其中一个用途就是可以进行浅拷贝。

```
let target = {};
let source = { a: { b: 2 } };
Object.assign(target, source);
console.log(target); // { a: { b: 10 } }; 
source.a.b = 10; 
console.log(source); // { a: { b: 10 } }; 
console.log(target); // { a: { b: 10 } };
```

首先通过 Object.assign 将 source 拷贝到 target 对象中，然后我们尝试将 source 对象中的 b 属性由 2 修改为 10。通过控制台可以发现，打印结果中，三个 target 里的 b 属性都变为 10 了，证明 Object.assign 暂时实现了我们想要的拷贝效果。

使用 object.assign 方法有几点需要注意：

- ​	它不会拷贝对象的继承属性；

- ​	它不会拷贝对象的不可枚举的属性；

- ​	可以拷贝 Symbol 类型的属性。


可以简单理解为：Object.assign 循环遍历原对象的属性，通过复制的方式将其赋值给目标对象的相应属性，来看一下这段代码，以验证它可以拷贝 Symbol 类型的对象。

```
let obj1 = { a:{ b:1 }, sym:Symbol(1)}; 
Object.defineProperty(obj1, 'innumerable' ,{
    value:'不可枚举属性',
    enumerable:false
});
let obj2 = {};
Object.assign(obj2,obj1)
obj1.a.b = 2;
console.log('obj1',obj1);
console.log('obj2',obj2);
```

![image-20210408151922045](C:\SAPShare\Chainnotes\Assets\image-20210408151922045.png)

从上面的样例代码中可以看到，利用 object.assign 也可以拷贝 Symbol 类型的对象，但是如果到了对象的第二层属性 obj1.a.b 这里的时候，前者值的改变也会影响后者的第二层属性的值，说明其中依旧存在着访问共同堆内存的问题，也就是说这种方法还不能进一步复制，而只是完成了浅拷贝的功能。



### 方法二：扩展运算符方式 {...} 

```
/* 对象的拷贝 */
let obj = {a:1, b:{c: 1}}
let obj2 = {...obj}
obj.a = 2
console.log(obj)  //{a:2,b:{c:1}} 
console.log(obj2); //{a:1,b:{c:1}}
obj.b.c = 2
console.log(obj)  //{a:2,b:{c:2}} 
console.log(obj2); //{a:1,b:{c:2}}

/* 数组的拷贝 */
let arr = [1, 2, 3];
let newArr = [...arr]; //跟arr.slice()是一样的效果
```

扩展运算符 和 object.assign 有同样的缺陷，也就是实现的浅拷贝的功能差不多，但是如果属性都是基本类型的值，使用扩展运算符进行浅拷贝会更加方便。

### 方法三：concat 拷贝数组



### 方法四：slice 拷贝数组 



## 手工实现一个浅拷贝 

## 深拷贝的原理和实现

### 方法一：乞丐版（JSON.stringfy）

### 方法二：基础版（手写递归实现）

### 方法三：改进版（改进后递归实现）

