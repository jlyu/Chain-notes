```js
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? []: {};
    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
        cloneTarget[prop] = target[prop];
      }
    }
    return cloneTarget;
  }
  return target;
}
```

```js
const isComplexDataType = obj => (typeof obj === 'object' || typeof obj === 'function') && (obj !== null)

const deepClone = function (obj, hash = new WeakMap()) {
  if (obj.constructor === Date) {
    return new Date(obj); // 日期对象直接返回一个新的日期对象
  }
  
  if (obj.constructor === RegExp) {
      return new RegExp(obj); //正则对象直接返回一个新的正则对象
  }

  //如果循环引用了就用 weakMap 来解决
  if (hash.has(obj)) { return hash.get(obj); }
  let allDesc = Object.getOwnPropertyDescriptors(obj);
  //遍历传入参数所有键的特性
  let cloneObj = Object.create(Object.getPrototypeOf(obj), allDesc);
  //继承原型链
  hash.set(obj, cloneObj)
  for (let key of Reflect.ownKeys(obj)) { 
    cloneObj[key] = (isComplexDataType(obj[key]) && typeof obj[key] !== 'function') ? deepClone(obj[key], hash) : obj[key];
  }
  return cloneObj;
}
```

```js
// new 被调用后大致做了哪几件事情。
// 1.让实例可以访问到私有属性；
// 2.让实例可以访问构造函数原型（constructor.prototype）所在原型链上的属性；
// 3.构造函数返回的最后结果是引用数据类型。

function _new(ctor, ...args) {
  if(typeof ctor !== 'function') {
    throw 'ctor must be a function';
  }
  let obj = new Object();
  obj.__proto__ = Object.create(ctor.prototype);
  let res = ctor.apply(obj,  [...args]);

  let isObject = typeof res === 'object' && res !== null;
  let isFunction = typeof res === 'function';
  return isObject || isFunction ? res : obj;
};
```

