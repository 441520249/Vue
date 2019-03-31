# Vue
Some research demonstrations on Vue

加油

# vue的原理：

- 1 核心是object.defineproperty，通过遍历劫持对象的所有属性并给所有属性绑定两个关键的回调函数，一个get的函数，一个set的函数，当你拿值就会触发get的回调函数，当你改值就会触发set的回调函数。

- 2 通过对节点（元素、属性、文本）的操作进行视图的编译，vue的指令就是处理元素节点时对该元素添加对应的属性并给该属性添加对应的方法。

- 3 利用发布订阅模式把M和V，数据劫持和视图编译建立关系。意思就是把emit的函数挂到set的函数，把on的函数挂到视图编译上，当数据发生变化，就会触发set()->emit()->on()->视图从新编译。

- 4 例子：addEventListener监听input输入框，当输入框的值发生变化，就会触发set的回调函数，通过发布订阅模式，经过emit()->on(),最后视图回从新进行编译，从而实现双向数据绑定。

- 5 生命周期就是在不同的时期触发对应的函数。

# 技术点目录
- 认识 Vue
- 认识数据驱动模式
- 认识 MVVM 模式
- 模版语法
- 样式绑定
- Vue 实例化时基本属性
- 修饰符
- 组件
- 组件通信
- 指令
- 自定义指令
- 动画和过度效果
- 路由
- vue-cli
- Vuex
  - Vuex 介绍和简单实现
  - state
  - mutation
  - getter
  - action
  - module
  
# 认识 Vue

关于 Vue 的描述有不少，不外乎都会拿来与 Angular 和 React 对比，同样头顶 MVVM 双向数据驱动设计模式光环的 Angular 自然被对比的最多，但到目前为止，Angular 在热度上已明显不及 Vue，性能已成为最大的诟病。

在我看来，Vue 和 Angular 的对比有种早些年 Java 和 ASP.NET 的对比，对于开发者而言，ASP.NET 官方本身已实现好了大量的框架和功能，使用起来非常的方便快捷，同时也提供了无限的可扩展性，对比起 Java 而言，后者在本身框架和功能上都不及 ASP.NET，但同样都拥有无限的可扩展性，相比之下，本来 ASP.NET 很有一统天下的可能，但现实终归现实，ASP.NET 本身的框架和功能实现并没有换来多少称赞，反在性能和安全性方面被诟病。回看 Vue 和 Angular 的阵营，我也总有这么一种感觉。

所以，在这个开源的年代，我认为一个框架功能不需要有多么强大，再强大再完善的功能都抵不上“适合”两字，反而轻量级且有无限可扩展性会成为所有开发者的追求。

关于 Vue、React 和 Angular，其实都是在原生 JS 基础上，对面向对象不一样的实现方式而已，所以要想使用这三者中的任意一种，首先要有一定的 JS 基础和对面向对象有一定的认识。

在代码层面，Vue 只是一个构造函数，整个 Vue 的实现都在实例化这个构造函数开始。

```
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
```
```
  <div id="app"></div>
```
```
  var vm = new Vue({
    el: '#app'// Vue 实例元素
    data: {
      //数据
    }
    ...
  })
```  
  
  
# 认识数据驱动模式

开始接触前端编程的基本上都是先学习 DOM 节点操作，jQuery 在这一领域上一度成为了标准，所以在前端编程的领域中，jQuery 几乎是每个开发者的标配。随着前后端分离的成熟，产品或项目都趋于分布式部署，开发者已不满足于操作 DOM 节点， 设计模式便慢慢的被前端化。

数据驱动的设计模式在思维逻辑上与 DOM 节点操作完全不一样。
```
  <div id="div1"></div>
  <div id="app">
    <span>{{message}}</span>
  </div>
```  
  //DOM 节点操作
  document.getElementById('div1').innerText = '节点被动改变'  
```
  //Vue 数据驱动： 当 message 发生改变的时候，span 会相应的发生改变，而不需要手动去改变 span。
  var vm = new Vue({
    el: '#app',
    data: {
      message: '我是通过映射显示的文本'
    }
  })
```  

# 认识 MVVM 模式
M：Model，称之为数据模型，在前端以对象的形式表现。
```
  var data = {message: '我就是一个数据模型'}
```  
V：View，视图，也就是 HTML
```
  <div id="app">
    <span>我是视图</span>
  </div>
```
VM：ViewModel，就是连接数据和视图的桥梁，当 Model 发生改变的时候，ViewModel 便将数据映射到视图。

那么数据驱动模式和 MVVM 模式有什么关系，换句话说，MVVM 是数据驱动模式的一种实现，Vue 是 MVVM 的一种实现。
