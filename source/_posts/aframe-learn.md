---
title: aframe是什么？
date: 2017-07-30 13:40:21
tags:
    - aframe
    - webvr
    - react
---

### 介绍
aframe是由[mozilla VR team](https://mozvr.com/)推出的开发webvr的开源框架，个人理解是在three.js和声明式标记语言（DOM）的桥梁，事实证明`ECS`系统搭配标记语言的套路十分强大，另外背后的DOM也将传统web生态无痛移植了过来，比如react，不过react只适合作为视图状态管理的工具，组件的开发注册交还给aframe，原因在于[多维环境的切换](https://github.com/aframevr/aframe-react#entity-component-meets-react)

##### 特性
- 声明式HTML
- 跨平台
- 实体组件架构 Entity-Component Architecture
    - A-Frame是一个功能强大的three.js框架，提供了一个声明性，可组合的，可重复使用的实体组件结构。开发人员可以无限制地访问JavaScript，DOM API，three.js，WebVR和WebGL。
- 性能
    - 基于WebVR优化了A-Frame。当A-Frame使用DOM时，其元素不会触及浏览器布局引擎。 3D对象更新都是在内存中完成的，只需一个requestAnimationFrame调用即可。
- 配合
    - 由于Web建立在HTML的概念之上，A-Frame与大多数库，框架和工具（包括React，Preact，Vue.js，d3.js，Ember.js，jQuery）兼容。
- 工具
    - [A-Frame Inspector](https://github.com/aframevr/aframe-inspector)
    - [Motion Capture](https://github.com/dmarcos/aframe-motion-capture-components)
- 核心&社区组件
    - [the A-Frame Registry](https://aframe.io/aframe-registry/)
    - [Entity...](https://aframe.io/docs/0.6.0/core/entity.html)

### 安装

##### 在线编辑

- glitch 
提供在线代码编辑器，即时部署和托管网站。[demo](https://glitch.com/~aframe-aincraft)
- CodePen
[demo](https://codepen.io/mozvr/pen/BjygdO)

##### 本地开发
可以clone我的[git分支项目](https://github.com/yangbin1994/h5-startKit/tree/aframe)，安装完成依赖后执行start就可以在本机ip的80端口访问了，这个项目是基于aframe-react开发的。

实际上，依赖只有一行代码
```html
<head>
  <script src="https://aframe.io/releases/0.6.1/aframe.min.js"></script>
</head>
```

### HTML & Primitives
Primitives是一种有语义的名称（如`<a-box>`）,充当复杂，但常见类型的实体的简写。
```html
<a-box color="red" width="3"></a-box>
```
=>
```html
<a-entity geometry="primitive: box; width: 3" material="color: red"></a-entity>
```

##### 注册Primitives
将html属性和背后的元数据进行关联，实时保持更新
[https://aframe.io/docs/0.6.0/introduction/html-and-primitives.html#registering-a-primitive](more)

### Entity-Component-System

> A-Frame是一个具有实体组件系统（ECS）架构的three.js框架。 ECS架构是3D和游戏开发中常见的理想模式，遵循继承和层次原理的组合。 ECS的好处包括： 通过混合和匹配可重复使用的部件来定义对象时的更大灵活性。 消除具有复杂交织功能的长遗传链的问题。 通过解耦，封装，模块化，可重用性促进清洁设计。 在复杂性方面构建VR应用程序的最具可扩展性的方法。 经验证的3D和VR开发架构。 允许扩展新功能（可能共享为社区组件）。 在2D网页上，我们列出了在层次结构中具有固定行为的元素。 3D和VR是不同的;有无限类型的可能的对象具有无界行为。 ECS提供了可管理的模式来构造对象类型。 以下是ECS架构的介绍材料。我们建议您通过他们来撷取，以更好地掌握好处。 ECS非常适合VR开发，而A-Frame完全基于这一范式：

### Declarative DOM-Based ECS

A-Frame通过使其声明性和基于DOM的方式使ECS进入另一个级别。传统上，基于ECS的引擎将创建实体，附加组件，更新组件，全部通过代码删除组件。但是A-Frame具有HTML和DOM，使ECS符合人体工程学，并解决了许多缺点。以下是DOM为ECS提供的能力：
- 使用查询选择器引用其他实体：DOM提供了一个强大的查询选择器系统，可以让我们查询场景图并选择与条件匹配的实体。我们可以通过ID，类或数据属性来获取对实体的引用。因为A-Frame是基于HTML的，所以我们可以使用开箱即用的查询选择器。 document.querySelector（'...'）。 
- 与事件的解耦交叉实体通信：DOM提供监听和发出事件的能力。这提供了实体之间的发布 - 订阅通信系统。组件不需要彼此了解，它们只能发出一个事件（可能会起泡），而其他组件可以在不相互调用的情况下收听这些事件。 ball.emit（ '...'）。 
- 使用DOM API生命周期管理的API：DOM提供了API来更新HTML元素和树，包括.setAttribute，.removeAttribute，.createElement和.removeChild。这些可以像正常的Web开发一样使用。 
- 使用属性选择器实体过滤：DOM提供属性选择器，允许我们查询具有或不具有某些HTML属性的实体或实体。这意味着我们可以要求具有或没有一定组件的实体。 document.querySelector（ '[敌人]：不包括[活着的]'）。 
- 声明性：最后，DOM提供HTML。 ECS和HTML之间的A-Frame桥接保证其声明性，可读性，做到可复制和粘贴。

### JavaScript, Events, DOM APIs
你可以使用dom的常见操作方法
- .querySelector()
- .querySelectorAll()
- .createElement()
- .appendChild()
- .removeChild()
- .setAttribute()
- .removeAttribute()
- .emit()
- .addEventListener()
- .removeEventListener()

### 使用three.js开发
自定义Primitives时候，你就可以发现了，其实Primitives背后是关联着threejs世界的实体，那么只要获得到就可以轻松使用threejs的方法进行开发[传送门](https://aframe.io/docs/0.6.0/introduction/developing-with-threejs.html)

### 定义组件
请注意，在`<a-scene>`之前，注册组件。

```html
<a-entity hello-word />
```
or use JS
```javascript
document.querySelector('a-scene').setAttribute('hello-world', '');
```

##### 使用Schema定义属性
如果我们将一个组件视为一个函数，那么一个组件的属性就像它的函数参数。属性具有名称（如果组件具有多个属性），默认值和属性类型。
```javascript
AFRAME.registerComponent('log', {
  schema: {
    message: {type: 'string', default: 'Hello, World!'}
  },
  // ...
});
```

可以通过data和oldData获取schema注册过，通过html属性传递进来的值

##### Handling Property Init
- .init
    当组件首次插入其实体时，它被调用一次；仅在实体准备就绪后才被调用

##### Handling Property Updates
- .update
    和init一样会首先调用一次，当`setAttribute`时候也会调用。

##### Handling Component Removal
- .remove
    组件从实体拔出时`removeEventListener`出调用

##### Allowing Multiple Instances of a Component
允许将多个日志组件附加到同一个实体。为此，我们使用.multiple标志启用多个实例。让我们把它设置为true：
语法表示为`<COMPONENTNAME>__<ID>`
```html
<a-scene>
  <a-entity log__helloworld="message: Hello, World!"
            log__metaverse="message: Hello, Metaverse!"></a-entity>
</a-scene>
```
or fromJS
```javascript
var el = document.querySelector('a-entity');
el.setAttribute('log__helloworld', {message: 'Hello, World!'});
el.setAttribute('log__metaverse', {message: 'Hello, Metaverse!'});
```

在组件中，我们可以使用this.id和this.attrName来区分不同的实例。给定`log__helloworld`，this.id将是helloworld，而this.attrName将是完整的`log__helloworld`。 

##### Handling Component Tick
- .tick
    处理程序将在每个帧（例如每秒90次）上被调用

2点优化
- 组件的所有实例之间共享。这样做会减少内存使用和垃圾收集
```javascript
AFRAME.registerComponent('foo', {
  tick: function () {
    this.doSomething();
  },
  doSomething: (function () {
    var helperVector = new THREE.Vector3();
    var helperQuaternion = new THREE.Quaternion();
    return function () {
      helperVector.copy(this.el.object3D.position);
      helperQuaternion.copy(this.el.object3D.quaternion);
    })();
  }
});
```
- 如果我们不断修改tick中的一个组件，请确保我们传递相同的对象来更新属性。 A-Frame将跟踪最新传递的对象，并在后续调用中禁用类型检查以提高额外的速度。这是一个建议的tick功能的示例，可以修改每个刻度上的旋转
```javascript
AFRAME.registerComponent('foo', {
  tick: function () {
    var el = this.el;
    var rotationTmp = this.rotationTmp = this.rotationTmp || {x: 0, y: 0, z: 0};
    var rotation = el.getAttribute('rotation');
    rotationTmp.x = rotation.x + 0.1;
    rotationTmp.y = rotation.y + 0.1;
    rotationTmp.z = rotation.z + 0.1;
    el.setAttribute('rotation', rotationTmp);
  }
});
```

##### 从里哪里学习获取组件
- [A-Frame Registry](https://aframe.io/aframe-registry/) - Curated community components.
- [A-Frame core components](https://github.com/aframevr/aframe/tree/master/src/components) - Source code of A-Frame’s standard components.
- [awesome-aframe components](https://github.com/aframevr/awesome-aframe#components) - Giant list of community components.
- [A-Painter components](https://github.com/aframevr/a-painter/tree/master/src/components) - Application-specific components for A-Painter.

##### 发布一个组件
使用angle来初始化组件模板
```bash
npm install -g angle && angle initcomponent
```

### 交互

就像2DWeb一样，A-Frame依赖于事件和事件监听器的交互性和动态性。但是，由于A框架是一个JavaScript框架，一切都在WebGL中完成，所以A-Frame的事件是可以由描述任何事件的任何组件发出的合成自定义事件。

一个常见的疑惑是，我们可以将一个点击事件侦听器添加到A-Frame实体，并希望通过使用鼠标直接点击该实体来工作。在WebGL中，我们必须提供提供这种点击事件的输入和交互。浏览器并不会帮我们去做这些事。

[event-set component](https://github.com/ngokevin/kframe/tree/master/components/event-set)，可以让实体获得监听某些事件的能力


### 相关参考

[A-Frame Introduction](https://aframe.io/docs/0.6.0/introduction/)