# 1、如何理解 MVVM 原理？

> 架构模式是一个通用的、可重用的解决方案，用于在给定上下文中的软件体系结构中经常出现的问题。

## 什么是 MVC 架构

**MVC，全称 Model View Controller，即 模型-视图-控制器，**是一种常见的架构模式。

在Web开发中，通常将前端页面分为上中下三层，分别是：

- View：视图层。负责 UI 呈现。

- Controller：控制层。负责用户交互控制逻辑。从视图接收信息，进行相应逻辑操作，将处理后的信息发送给模型。

- Model: 模型层，又名数据层。负责存储数据信息。

*PS：简单来说，Controller 负责将 Model 的数据用 View 显示出来。*

优点：MVC 架构实现了模块化，使程序易于维护和扩展，也让程序部分功能复用成为可能。

## 什么是 MVVM 架构

**MVVM，全称 Model View ViewModel，本质上是 MVC 架构的改进版。**

随着业务需求的增加，程序越来越复杂，如果把 `View` 和 `Model` 对应的控制关系全部扔到 `Controller` 中，那么 `Controller` 将会变得非常庞大臃肿，好像盘互交错的树根。
而 MVVM 架构的出现很好地解决了上述的问题。

MVVM 分离了视图层和数据层 `Vie` 和 `Model` 不能直接通信，而必须通过 `ViewModel` 才能进行交互，当用户操作视图，`ViewModel` 自动感知变化，然后通知 `Model` 作出改变数据，而当数据变化时，`ViewModel` 同样能感知到，并使视图更新，这就是所谓的 **<a href="#?id=什么是数据响应式">双向绑定</a>**。

优点：MVVM 的响应式架构简化了原本繁琐的 dom 操作，提高了页面渲染性能。视图和数据自动同步，易于维护复杂变化的数据状态。

<!-- ## MVC 和 MVVM 区别

可以把 ViewModel 看作是对 Controller 的封装， -->

# 2、如何理解响应式数据原理？

> 利用 Object.defineProperty() 劫持数据，实现数据响应。

## 什么是数据响应式

**数据响应式，即 数据双向绑定。改变 Model 时，View 自动更新。改变 View 时，Model 也会自动更新。**

要实现数据双向绑定，关键是追踪变化和通知视图更新，这里涉及两个概念:

- [Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
- [观察者模式](https://www.runoob.com/design-pattern/observer-pattern.html)


### Object.defineProperty()

**Object.defineProperty() 是 ES5 中的方法，作用是在一个对象上定义一个新属性，或者修改一个已经存在的属性。**

该方法接收三个参数：`属性所在对象`、`属性名`、`描述符对象`。

ECMAScript 中对象有两种属性，分别是：**数据属性** 和 **访问器属性**。其中，访问器属性必须通过 Object.defineProperty() 才能定义。

访问器属性不包含数据值，它们包含一对 `getter` 和 `setter` 函数(非必须)。读取访问器属性时，会调用 `getter` 函数，负责返回有效的值；写入访问器属性时，会调用 `setter` 函数并传入新值，负责决定如何处理数据。

``` js
var people = {
  name: 'nanfs',
  _age: 20
}

// 访问器属性必须使用 Object.defineProperty() 定义。
Object.defineProperty(people, age, {
  get() {
    return this._age;
  },
  set() {
    this._age++;
  }
})

people.age = 10;
console.log(people.age);  // 11
```

当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，并使用 Object.defineProperty 把这些 property 全部转为 `getter/setter`。

### 观察者模式

**定义：对象间存在一对多关系时，当一个对象被修改时，则会自动通知依赖它的对象。**

举例来说，例如淘宝购物，对于某个产品，商家每次修改操作(上架、折扣等)，系统都会自动将变动通知给每个关注了该产品的用户。

# 3、Vue 中如何检测数组变化？

vue 重写了数组方法，并将当前数组变更通知给订阅者。[查看源码](https://github.com/vuejs/vue/blob/dev/src/core/observer/array.js)

# 4、Vue 为什么采用异步渲染？

如不采取异步更新，那么每次更新数据都会对当前组件进行重新渲染，为了性能考虑，Vue 会在本轮数据更新后，再去异步更新 dom。

# 5、nextTick() 实现原理？

nextTick() 作用是在 dom 更新完成后执行一次回调。其原理是**利用异步队列的方式来控制 dom 更新和 nextTick 回调先后执行。**Vue 在内部对异步队列尝试使用原生的 `Promise.then`、`MutationObserver` 和 `setImmediate`，如果执行环境不支持，则会采用 `setTimeout(fn, 0)` 代替。

# 6、Vue 组件的生命周期？

## beforeCreate

实例初始化之后被调用，`this` 指向创建实例，不能访问 `data`、`computed`、`watch`、`methods` 上的数据和方法。

## created

实例创建完成被调用。可以访问 `data`、`computed`、`watch`、`methods` 上的数据和方法，但尚未挂载至 dom，无法访问 `$el`。

## beforeMount

挂载开始前被调用。首次执行 render 函数。该钩子在服务器端渲染期间不被调用。

## mounted

`el` 被新创建的 `vm.$el` 替换。此时可通过 dom API 获取 dom 节点，`$ref` 属性可访问。

# 🏗️ 板块更新

建设中...