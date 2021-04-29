# 动机

随着 JavaScript 单页应用开发日趋复杂，**JavaScript 需要管理比任何时候都要多的 state （状态）**。

复杂性很大程度上来自于：**我们总是将两个难以理清的概念混淆在一起：变化和异步**。 我称它们为[曼妥思和可乐](https://en.wikipedia.org/wiki/Diet_Coke_and_Mentos_eruption)。如果把二者分开，能做的很好，但混到一起，就变得一团糟。一些库如 [React](http://facebook.github.io/react) 试图在视图层禁止异步和直接操作 DOM 来解决这个问题。美中不足的是，React 依旧把处理 state 中数据的问题留给了你。Redux 就是为了帮你解决这个问题。

**Redux 试图让 state 的变化变得可预测**。



# 三个原则

## 单一数据源

整个应用的 [state](https://cn.redux.js.org/docs/Glossary.html#state) 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 [store](https://cn.redux.js.org/docs/Glossary.html#store) 中。受益于单一的 state tree ，以前难以实现的如“撤销/重做”这类功能也变得轻而易举。

## State 只读

唯一改变 state 的方法就是触发 [action](https://cn.redux.js.org/docs/Glossary.html#action)，action 是一个用于描述已发生事件的普通对象。确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化处理，且严格按照一个接一个的顺序执行，因此不用担心竞态条件（race condition）的出现。

## 使用纯函数来执行修改

为了描述 action 如何改变 state tree ，你需要编写 [reducers](https://cn.redux.js.org/docs/Glossary.html#reducer)。





# Store

```js
type Store = {
  dispatch: Dispatch
  getState: () => State
  subscribe: (listener: () => void) => () => void
  replaceReducer: (reducer: Reducer) => void
}
```

Store 维持着应用的 state tree 对象。 因为应用的构建发生于 reducer，所以一个 Redux 应用中应当只有一个 Store。

# State

*State* (也称为 *state tree*) 是一个宽泛的概念，但是在 Redux API 中，通常是指一个唯一的 state 值，由 store 管理且由 [`getState()`](https://cn.redux.js.org/docs/api/Store.html#getState) 方法获得。它表示了 Redux 应用的全部状态，通常为一个多层嵌套的对象。

约定俗成，顶层 state 或为一个对象，或像 Map 那样的键-值集合，也可以是任意的数据类型。然而你应尽可能确保 state 可以被序列化，而且不要把什么数据都放进去，导致无法轻松地把 state 转换成 JSON。

# Dispatch

```js
type BaseDispatch = (a: Action) => Action
type Dispatch = (a: Action | AsyncAction) => any
```

*dispatching function* (或简言之 *dispatch function*) 是一个接收 action 或者[异步 action](https://cn.redux.js.org/docs/Glossary.html#异步-action)的函数，该函数要么往 store 分发一个或多个 action，要么不分发任何 action。

我们必须分清一般的 dispatch function 以及由 store 实例提供的没有 middleware 的 base [`dispatch`](https://cn.redux.js.org/docs/api/Store.html#dispatch) function 之间的区别。

Base dispatch function **总是**同步地把 action 与上一次从 store 返回的 state 发往 reducer，然后计算出新的 state。它期望 action 会是一个可以被 reducer 消费的普通对象。

[Middleware](https://cn.redux.js.org/docs/Glossary.html#middleware) 封装了 base dispatch function，允许 dispatch function 处理 action 之外的[异步 action](https://cn.redux.js.org/docs/Glossary.html#异步-action)。 Middleware 可以改变、延迟、忽略 action 或异步 action，也可以在传递给下一个 middleware 之前对它们进行解释 

# Reducer

规范创建state的过程 （就是把 action 和 state 串起来）reducer 只是一个接收 state 和 action，并返回新的 state 的函数。 

reducer 一定要保持纯净。**只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。**

## initState

# Action

强制使用 action 来描述所有变化带来的好处是可以清晰地知道应用中到底发生了什么。如果一些东西改变了，就可以知道为什么变。action 就是描述了本次的变动。

## type

## payload

# Selector

# Connect

# Provider

