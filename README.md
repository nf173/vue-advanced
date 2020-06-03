# 1、如何理解 MVVM 原理？

> 架构模式是一个通用的、可重用的解决方案，用于在给定上下文中的软件体系结构中经常出现的问题。

## 什么是 MVC 架构

**MVC，全称 Model View Controller，即 模型-视图-控制器，**是一种常见的架构模式。

在Web开发中，通常将前端页面分为上中下三层，分别是：

- View：视图层。负责 UI 呈现。

- Controller：控制层。负责用户交互控制逻辑。从视图接收信息，进行相应逻辑操作，将处理后的信息发送给模型。

- Model: 模型层，又名数据层。负责存储数据信息。

*PS：以上又可以用一句话概括，Controller 负责将 Model 的数据用 View 显示出来。*

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

要实现数据双向绑定，需要先理解两个概念:

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

<!-- ### 观察者模式 -->







