![image-20210414172111991](C:\SAPShare\Chainnotes\Assets\image-20210414172111991.png)



# 考察基准

- [ ] 手写深浅拷贝
- [ ] 理解原型链，6种继承实现
- [ ] 手写new / call / apply / bind 底层逻辑实现
- [ ] 理解闭包
- [ ] 手写 JSON.stringify 方法





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

```javascript
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

```javascript
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

```javascript
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

```javascript
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











# 继承实现

在 JavaScript 中，对象的用途很是广泛，因为它的值既可以是原始类型（number、string、boolean、null、undefined、bigint和symbol），还可以是对象和函数。

不管是对象，还是函数和数组，它们都是Object的实例，也就是说**在 JavaScript 中，除了原始类型以外，其余都是对象。**每个对象还都拥有一个原型对象，并可以从中继承方法和属性。

函数也是一种特殊的对象，它同样拥有属性和值。所有的函数会有一个特别的属性 `prototype`，该属性的值是一个对象，这个对象便是我们常说的“原型对象”。

```js
function Person(name) {
  this.name = name;
}

console.log(Person.prototype);
```

![Drawing 0.png](C:\SAPShare\Chainnotes\Assets\CioPOWBwCzyAM-CAAAAKDg-SVug894.png)

可以看到，该原型对象有两个属性：`constructor`和`__proto__`。

在 JavaScript 中，`__proto__`属性指向对象的原型对象，对于函数来说，它的原型对象便是`prototype`。函数的原型对象`prototype`有以下特点：

- 默认情况下，所有函数的原型对象（`prototype`）都拥有`constructor`属性，该属性指向与之关联的构造函数，在这里构造函数便是`Person`函数；
- `Person`函数的原型对象（`prototype`）同样拥有自己的原型对象，用`__proto__`属性表示。前面说过，函数是`Object`的实例，因此`Person.prototype`的原型对象为`Object.prototype。`

我们可以用这样一张图来描述`prototype`、`__proto__`和`constructor`三个属性的关系：

<img src="C:\SAPShare\Chainnotes\Assets\image-20210419150258405.png" alt="image-20210419150258405" style="zoom:67%;" />

- 在 JavaScript 中，`__proto__`属性指向对象的原型对象；
- 对于函数来说，每个函数都有一个`prototype`属性，该属性为该函数的原型对象。

## 使用 prototype 和 **proto** 实现继承

前面说过，对象之所以使用广泛，是因为对象的属性值可以为任意类型。因此，属性的值同样可以为另外一个对象，这意味着 JavaScript 可以这么做：通过将对象 A 的`__proto__`属性赋值为对象 B，即`A.__proto__ = B`，此时使用`A.__proto__`便可以访问 B 的属性和方法。

这样，JavaScript 可以在两个对象之间创建一个关联，使得一个对象可以访问另一个对象的属性和方法，从而实现了继承。

那么，JavaScript 又是怎样使用`prototype`和`__proto__`实现继承的呢？

继续以`Person`为例，当我们使用`new Person()`创建对象时，JavaScript 就会创建构造函数`Person`的实例，比如这里我们创建了一个叫“Lily”的`Person`：

```
var lily = new Person("Lily");
```

上述这段代码在运行时，JavaScript 引擎通过将`Person`的原型对象`prototype`赋值给实例对象`lily`的`__proto__`属性，实现了`lily`对`Person`的继承，即执行了以下代码：

```
// 实际上 JavaScript 引擎执行了以下代码
var lily = {};
lily.__proto__ = Person.prototype;
Person.call(lily, "Lily");
```

我们来打印一下`lily`实例：

![Drawing 3.png](C:\SAPShare\Chainnotes\Assets\Cgp9HWBwC56AVE8iAAAQagv5qXA279.png)

可以看到，`lily`作为`Person`的实例对象，它的`__proto__`指向了`Person`的原型对象，即`Person.prototype`。

<img src="C:\SAPShare\Chainnotes\Assets\image-20210419151325806.png" alt="image-20210419151325806" style="zoom: 67%;" />

从这幅图中，我们可以清晰地看到构造函数和`constructor`属性、原型对象（`prototype`）和`__proto__`、实例对象之间的关系，这是很多初学者容易搞混的。根据这张图，我们可以得到以下的关系：

1. 每个函数的原型对象（`Person.prototype`）都拥有`constructor`属性，指向该原型对象的构造函数（`Person`）；
2. 使用构造函数（`new Person()`）可以创建对象，创建的对象称为实例对象（`lily`）；
3. 实例对象通过将`__proto__`属性指向构造函数的原型对象（`Person.prototype`），实现了该原型对象的继承。

那么现在，`__proto__`和`prototype`的关系，可以得到这样的答案：

- 每个对象都有`__proto__`属性来标识自己所继承的原型对象，但**只有函数才有`prototype`属性**；
- 对于函数来说，每个函数都有一个`prototype`属性，该属性为该函数的原型对象；
- 通过将实例对象的`__proto__`属性赋值为其构造函数的原型对象`prototype`，JavaScript 可以使用构造函数创建对象的方式，来实现继承。

现在我们知道，一个对象可通过`__proto__`访问原型对象上的属性和方法，而该原型同样也可通过`__proto__`访问它的原型对象，这样我们就在实例和原型之间构造了一条原型链。这里我用红色的线将`lily`实例的原型链标了出来。

<img src="C:\SAPShare\Chainnotes\Assets\image-20210419151722755.png" alt="image-20210419151722755" style="zoom:67%;" />

在 JavaScript 中，是通过遍历原型链的方式，来访问对象的方法和属性。

## 通过原型链访问对象的方法和属性

当 JavaScript 试图访问一个对象的属性时，会基于原型链进行查找。查找的过程是这样的：

- 首先会优先在该对象上搜寻。如果找不到，还会依次层层向上搜索该对象的原型对象、该对象的原型对象的原型对象等（套娃告警）；
- JavaScript 中的所有对象都来自`Object`，`Object.prototype.__proto__ === null`。`null`没有原型，并作为这个原型链中的最后一个环节；
- JavaScript 会遍历访问对象的整个原型链，如果最终依然找不到，此时会认为该对象的属性值为`undefined`。

我们可以通过一个具体的例子，来表示基于原型链的对象属性的访问过程，在该例子中我们构建了一条对象的原型链，并进行属性值的访问：

```
// 让我们假设我们有一个对象 o, 其有自己的属性 a 和 b：
var o = {a: 1, b: 2};
// o 的原型 o.__proto__有属性 b 和 c：
o.__proto__ = {b: 3, c: 4};
// 最后, o.__proto__.__proto__ 是 null.
// 这就是原型链的末尾，即 null，
// 根据定义，null 没有__proto__.
// 综上，整个原型链如下:
{a:1, b:2} ---> {b:3, c:4} ---> null
// 当我们在获取属性值的时候，就会触发原型链的查找：
console.log(o.a); // o.a => 1
console.log(o.b); // o.b => 2
console.log(o.c); // o.c => o.__proto__.c => 4
console.log(o.d); // o.c => o.__proto__.d => o.__proto__.__proto__ == null => undefined
```

可以看到，当我们对对象进行属性值的获取时，会触发该对象的原型链查找过程。

既然 JavaScript 中会通过遍历原型链来访问对象的属性，那么我们可以通过原型链的方式进行继承。

也就是说，可以通过原型链去访问原型对象上的属性和方法，我们不需要在创建对象的时候给该对象重新赋值/添加方法。比如，我们调用`lily.toString()`时，JavaScript 引擎会进行以下操作：

1. 先检查`lily`对象是否具有可用的`toString()`方法；
2. 如果没有，则``检查`lily`的原型对象（`Person.prototype`）是否具有可用的`toString()`方法；
3. 如果也没有，则检查`Person()`构造函数的`prototype`属性所指向的对象的原型对象（即`Object.prototype`）是否具有可用的`toString()`方法，于是该方法被调用。

由于通过原型链进行属性的查找，需要层层遍历各个原型对象，此时可能会带来性能问题：

- 当试图访问不存在的属性时，会遍历整个原型链；
- 在原型链上查找属性比较耗时，对性能有副作用，这在性能要求苛刻的情况下很重要。

因此，我们在设计对象的时候，需要注意代码中原型链的长度。当原型链过长时，可以选择进行分解，来避免可能带来的性能问题。

除了通过原型链的方式实现 JavaScript 继承，JavaScript 中实现继承的方式还包括经典继承(盗用构造函数)、组合继承、原型式继承、寄生式继承，等等。

- 原型链继承方式中引用类型的属性被所有实例共享，无法做到实例私有；
- 经典继承方式可以实现实例属性私有，但要求类型只能通过构造函数来定义；
- 组合继承融合原型链继承和构造函数的优点，它的实现如下：

```
function Parent(name) {
  // 私有属性，不共享
  this.name = name;
}
// 需要复用、共享的方法定义在父类原型上
Parent.prototype.speak = function() {
  console.log("hello");
};
function Child(name) {
  Parent.call(this, name);
}
// 将子类的 __proto__ 指向父类原型
Child.prototype = Parent.prototype;
```

组合继承模式通过将共享属性定义在父类原型上、将私有属性通过构造函数赋值的方式，实现了按需共享对象和方法，是 JavaScript 中最常用的继承模式。

虽然在继承的实现方式上有很多种，但实际上都离不开原型对象和原型链的内容，因此掌握`__proto__`和`prototype`、对象的继承等这些知识，是我们实现各种继承方式的前提。



##  实现继承的6种方式

### 第一种：原型链继承

原型链继承是比较常见的继承方式之一，涉及的构造函数、原型和实例，三者之间存在着一定的关系，即每一个构造函数都有一个原型对象，原型对象又包含一个指向构造函数的指针，而实例则包含一个原型对象的指针。

```js
  function Parent1() {
    this.name = 'parent1';
    this.play = [1, 2, 3]
  }
  function Child1() {
    this.type = 'child2';
  }
  Child1.prototype = new Parent1();
  console.log(new Child1());
```

上面的代码看似没有问题，虽然父类的方法和属性都能够访问，但其实有一个潜在的问题，我再举个例子来说明这个问题。

```js
  var s1 = new Child1();
  var s2 = new Child2();
  s1.play.push(4);
  console.log(s1.play, s2.play);
```


这段代码在控制台执行之后，可以看到结果如下：

```
▶(4) [1, 2, 3, 4]
▶(4) [1, 2, 3, 4]
```

明明只改变了 s1 的 play 属性，为什么 s2 也跟着变了呢？

原因很简单，因为两个实例使用的是同一个原型对象。它们的内存空间是共享的，当一个发生变化的时候，另外一个也随之进行了变化，这就是使用原型链继承方式的一个缺点。

那么要解决这个问题的话，我们就得再看看其他的继承方式，下面我们看看能解决原型属性共享问题的第二种方法。

### 第二种：构造函数继承（借助 call）

```js
function Parent1() {
    this.name = 'parent1';
}

Parent1.prototype.getName = function () {
    return this.name;
}

function Child1() {
  Parent1.call(this);
  this.type = 'child1'
}

let child = new Child1();
console.log(child);  // 没问题
console.log(child.getName());  // 会报错


//执行上面的这段代码，可以得到这样的结果。
▼Child1 { name: "parent1", type: "child1" }
	name: "parent1"
	type: "child1"
▶__proto__: Object
```

可以看到最后打印的 child 在控制台显示，除了 Child1 的属性 type 之外，也继承了 Parent1 的属性 name。这样写的时候，子类虽然能够拿到父类的属性值，解决了第一种继承方式的弊端，但**问题是，父类原型对象中一旦存在父类之前自己定义的方法，那么子类将无法继承这些方法。**这种情况的控制台执行结果所示。

```
▼Uncaught TypeError: child.getName is not a function at <~~~>
```

因此，从上面的结果就可以看到构造函数实现继承的优缺点，它使父类的引用属性不会被共享，优化了第一种继承方式的弊端；但是随之而来的缺点也比较明显——**只能继承父类的实例属性和方法，不能继承原型属性或者方法。**

上面的两种继承方式各有优缺点，那么结合二者的优点，于是就产生了下面这种组合的继承方式。



### 第三种：组合继承（前两种组合）

```js
function Parent3 () {
	this.name = 'parent3';
	this.play = [1, 2, 3];
}

Parent3.prototype.getName = function () {
	return this.name;
}

function Child3() {
	// 第二次调用 Parent3()
	Parent3.call(this);
	this.type = 'child3';
}

// 第一次调用 Parent3()
Child3.prototype = new Parent3();
// 手动挂上构造器，指向自己的构造函数
Child3.prototype.constructor = Child3;
var s3 = new Child3();
var s4 = new Child3();
s3.play.push(4);
console.log(s3.play, s4.play);  // 不互相影响
console.log(s3.getName()); // 正常输出'parent3'
console.log(s4.getName()); // 正常输出'parent3'
```


执行上面的代码，可以看到控制台的输出结果，之前方法一和方法二的问题都得以解决。

```
▶(4) [1, 2, 3, 4]
▶(3) [1, 2, 3]
parent3
parent3
```

但是这里又增加了一个新问题：**通过注释我们可以看到 Parent3 执行了两次**，第一次是改变Child3 的 prototype 的时候，第二次是通过 call 方法调用 Parent3 的时候，那么 Parent3 多构造一次就多进行了一次性能开销，这是我们不愿看到的。

那么是否有更好的办法解决这个问题呢？请你再往下学习，下面的第六种继承方式可以更好地解决这里的问题。

上面介绍的更多是围绕着构造函数的方式，那么对于 JavaScript 的普通对象，怎么实现继承呢？

### 第四种：原型式继承

这里不得不提到的就是 ES5 里面的 `Object.create` 方法，这个方法接收两个参数：一是用作新对象原型的对象、二是为新对象定义额外属性的对象（可选参数）。

```js
let parent4 = {
    name: "parent4",
    friends: ["p1", "p2", "p3"],
    getName: function() {
        return this.name;
    }
};

let person4 = Object.create(parent4);
person4.name = "tom";
person4.friends.push("jerry");

let person5 = Object.create(parent4);
person5.friends.push("lucy");

console.log(person4.name);
console.log(person4.name === person4.getName());
console.log(person5.name);
console.log(person4.friends);
console.log(person5.friends);
```

从上面的代码中可以看到，通过 Object.create 这个方法可以实现普通对象的继承，不仅仅能继承属性，同样也可以继承 getName 的方法，请看这段代码的执行结果。

```
tom
true
parent4
▶(5) ["p1", "p2", "p3", "jerry", "luck"]
▶(5) ["p1", "p2", "p3", "jerry", "luck"]
```

第一个结果“tom”，比较容易理解，person4 继承了 parent4 的 name 属性，但是在这个基础上又进行了自定义。

第二个是继承过来的 getName 方法检查自己的 name 是否和属性里面的值一样，答案是 true。

第三个结果“parent4”也比较容易理解，person5 继承了 parent4 的 name 属性，没有进行覆盖，因此输出父对象的属性。

最后两个输出结果是一样的，讲到这里你应该可以联想到 02 讲中浅拷贝的知识点，关于引用数据类型“共享”的问题，其实 Object.create 方法是可以为一些对象实现浅拷贝的。

那么关于这种继承方式的缺点也很明显，多个实例的引用类型属性指向相同的内存，存在篡改的可能，接下来我们看一下在这个继承基础上进行优化之后的另一种继承方式——寄生式继承。

### 第五种：寄生式继承

使用原型式继承可以获得一份目标对象的浅拷贝，然后利用这个浅拷贝的能力再进行增强，添加一些方法，这样的继承方式就叫作**寄生式继承**。

虽然其优缺点和原型式继承一样，但是对于普通对象的继承方式来说，寄生式继承相比于原型式继承，还在父类基础上添加了更多的方法。 

```js
let parent5 = {
    name: "parent5",
    friends: ["p1", "p2", "p3"],
    getName: function() {
        return this.name;
    }
};

function clone(original) {
    let clone = Object.create(original);
    clone.getFriends = function() {
        return this.friends;
    };
    return clone;
}

let person5 = clone(parent5);

console.log(person5.getName());
console.log(person5.getFriends());
```

通过上面这段代码，我们可以看到 person5 是通过寄生式继承生成的实例，它不仅仅有 getName 的方法，而且可以看到它最后也拥有了 getFriends 的方法，结果如下图所示。

```
parent5
▶(3) ["p1", "p2", "p3"]
```

从最后的输出结果中可以看到，person5 通过 clone 的方法，增加了 getFriends 的方法，从而使 person5 这个普通对象在继承过程中又增加了一个方法，这样的继承方式就是寄生式继承。

我在上面第三种组合继承方式中提到了一些弊端，即两次调用父类的构造函数造成浪费，下面要介绍的寄生组合继承就可以解决这个问题。

### 第六种：寄生组合式继承

结合第四种中提及的继承方式，解决普通对象的继承问题的 Object.create 方法，我们在前面这几种继承方式的优缺点基础上进行改造，得出了寄生组合式的继承方式，这也是所有继承方式里面相对最优的继承方式。

```js
function clone (parent, child) {
    // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
    child.prototype = Object.create(parent.prototype);
    child.prototype.constructor = child;
}

function Parent6() {
    this.name = 'parent6';
    this.play = [1, 2, 3];
}
Parent6.prototype.getName = function () {
    return this.name;
}
function Child6() {
    Parent6.call(this);
    this.friends = 'child5';
}

clone(Parent6, Child6);

Child6.prototype.getFriends = function () {
    return this.friends;
}

let person6 = new Child6();
console.log(person6);
console.log(person6.getName());
console.log(person6.getFriends());
```

通过这段代码可以看出来，这种寄生组合式继承方式，基本可以解决前几种继承方式的缺点，*较好地实现了继承想要的结果，同时也减少了构造次数，减少了性能的开销，*我们来看一下上面这一段代码的执行结果。

```
parent6 {name: "parent6", play: Array(3), friends: "child5" }
	friends: "child5"
	name: "parent6"
	▶play: (3) [1, 2, 3]
	▶__proto__: Parent6
parent6
child5
```

可以看到 person6 打印出来的结果，*属性都得到了继承*，方法也没问题，可以输出预期的结果。

整体看下来，这六种继承方式中，寄生组合式继承是这六种里面最优的继承方式。另外，ES6 还提供了继承的关键字 extends，我们再看下 extends 的底层实现继承的逻辑。



|                              | 继承实现                                   | 实例代码                                                     | 缺点                                                         | 优点         |
| ---------------------------- | ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------ |
| 1.  原型链继承               | 通过子类的 `prototype` 属性关联到 父类     | Child1.prototype = new Parent1();                            | 两个实例使用的是同一个原型对象。内存空间共享                 |              |
| 2. 构造函数继承（借助 call） | 通过在子类构造函数内执行 `call` 函数       | function Child1() { Parent1.call(this); }                    | **只能继承父类的实例属性和方法，不能继承原型属性或者方法。** | 解决1缺点    |
| 3. 组合继承（前两种组合）    | 通过子类手动挂上构造器指向自己的构造函数   | Child3.prototype.constructor = Child3;                       | 父类被执行构造了2次，会有额外的性能开销                      | 解决1和2缺点 |
| 4. 原型式继承                | 通过 Object.create                         | child4 = Object.create(parent4);                             | 两个实例使用的是同一个原型对象。内存空间共享                 |              |
| 5. 寄生式继承                | 通过 Object.create生成浅拷贝对象上增加方法 | let clone = Object.create(original); <br>clone.getFriends = function() { ... } | 父类被执行构造了2次，会有额外的性能开销                      |              |
| 6. 寄生组合式继承            | 通过 Object.create                         | child.prototype = Object.create(parent.prototype);<br/>child.prototype.constructor = child; | 无                                                           | 解决1-5缺点  |

<img src="C:\SAPShare\Chainnotes\Assets\image-20210420173900760.png" alt="image-20210420173900760" style="zoom: 80%;" />

通过 Object.create 来划分不同的继承方式，最后的寄生式组合继承方式是通过组合继承改造之后的最优继承方式，而 extends 的语法糖和寄生组合继承的方式基本类似。

### ES6 的 extends 关键字实现逻辑

我们可以利用 ES6 里的 extends 的语法糖，使用关键词很容易直接实现 JavaScript 的继承，但是如果想深入了解 extends 语法糖是怎么实现的，就得深入研究 extends 的底层逻辑。

```js
class Person {
  constructor(name) {
    this.name = name
  }
  // 原型方法
  // 即 Person.prototype.getName = function() { }
  // 下面可以简写为 getName() {...}
  getName = function () {
    console.log('Person:', this.name)
  }
}
class Gamer extends Person {
  constructor(name, age) {
    // 子类中存在构造函数，则需要在使用“this”之前首先调用 super()。
    super(name)
    this.age = age
  }
}
const asuna = new Gamer('Asuna', 20)
asuna.getName() // 成功访问到父类的方法
```

因为浏览器的兼容性问题，如果遇到不支持 ES6 的浏览器，那么就得利用 babel 这个编译工具，将 ES6 的代码编译成 ES5，让一些不支持新语法的浏览器也能运行。

那么最后 extends 编译成了什么样子呢？从编译完成的源码中可以看到，它采用的也是**寄生组合继承方式**。

```js
function _possibleConstructorReturn (self, call) { 
		// ...
		return call && (typeof call === 'object' || typeof call === 'function') ? call : self; 
}
function _inherits (subClass, superClass) { 
    // 这里可以看到
	subClass.prototype = Object.create(superClass && superClass.prototype, { 
		constructor: { 
			value: subClass, 
			enumerable: false, 
			writable: true, 
			configurable: true 
		} 
	}); 
	if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass; 
}

var Parent = function Parent () {
	// 验证是否是 Parent 构造出来的 this
	_classCallCheck(this, Parent);
};
var Child = (function (_Parent) {
	_inherits(Child, _Parent);
	function Child () {
		_classCallCheck(this, Child);
		return _possibleConstructorReturn(this, (Child.__proto__ || Object.getPrototypeOf(Child)).apply(this, arguments));
}
	return Child;
}(Parent));
```



