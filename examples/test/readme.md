## 1. 调用文件顺序

### 1.1 src/platforms/web/entry-runtime-with-compiler.js

入口文件，覆盖$mount，获取template，执行模板解析和编译工作

### 1.2 src/platforms/web/runtime/index.js

定义$mount方法

### 1.3 src/core/index.js

继续查找Vue构造函数，定义全局API，比如set，nextTick，use等方法，/src/core/global-api/index.js

### 1.4 src/core/instance/index.js

定义Vue构造函数，执行this._init(options)

### 1.5 src/core/instance/init.js

定义初始化方法_init()，供Vue构造函数使用，核心代码如下：

```
initLifecycle(vm)  // $parent, $root, $children, $refs初始化定义
initEvents(vm)  //对父组件添加监听，传入监听函数
initRender(vm)  // 声明$slots, $createElement()
callHook(vm, 'beforeCreate') // 调用beforeCreate钩子
initInjections(vm) // resolve injections before data/props，注入数据
initState(vm) // 数据初始化，响应式
initProvide(vm) // resolve provide after data/props，提供数据
callHook(vm, 'created')  // 调用created钩子
```


### 2. 流程总结

new Vue() => this._init(options) => $mount => mountComponent() =>_render() => _update()

- new Vue()：调用this._init(options)
- this._init(options)：初始化各种属性
- $mount：调用mountComponent()
- _render()：声明updateComponent()，创建Watcher，生成vnode虚拟dom
- _update()：将虚拟dom转成真实dom

