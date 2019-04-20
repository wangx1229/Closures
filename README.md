# Closures
闭包
## 概念
* MDN文档对闭包的解释：
  > **闭包** 是函数和声明该函数的词法环境的组合。
* 我的看法
 JS中的闭包，是通过函数，返回一个函数而形成的，他有这么几个特点：

 1.返回一个函数
 2.返回的函数，可以记录当前函数所在的作用域，所以可以访问作用域中的其他变量
 3.因为记录了所在的作用域，所以作用域中的内容不会被JS进行垃圾回收
## 应用
举一个最有用的例子，模块机制

  通过闭包，返回一个对象，包含闭包中所创建的变量，也可以看作是模块的api

``` javascript
var MyModules = (function Manger() {
  var modules = {}
  function define(name, mods, fn) {
    for (var i=0; i<mods.length, i++) {
      mods[i] = module[mods[i]]
    }
    modules[name] = fn.apply(fn, mods)
  }
  function get(name) {
    return modules[name]
  }
  return {
    define: define,
     get: get
  }
})() 
```
代码解析

  首先MyModules函数返回的对象包括define定义模块和get使用模块两个方法

  define方法，接收三个参数，name-模块名称，mods-其他模块名称组成的数组，fn-name模块的功能

  当我们调用define定义模块的时候，会首先通过mods查找别的模块并组成一个模块数组，接着将fn的功能返回给name对应的模块

  其中调用apply方法时候，出了设置了this的绑定之外，还将别的模块引入fn，进而可以调用别的模块。
