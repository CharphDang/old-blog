---
title: es6Class
date: 2019-03-04 10:06:36
tags: 
- class
- prototype
categories:
- class
- prototype
comments: false
---

# es6中class的基本使用

- 有几个关键字的使用：

  - class： 用来定义'类'； 
  - constructor： 用来初始化'类'；
  - extends： 用来子类继承父类；
  - super： 调用父类方法
    - 区分一： constructor 中，直接调用super()并传参，相当于调用父级的constructor；
    - 区分二：普通方法中（原型方法），super指向父类的原型对象，调用super.xx()， xx指代父级的某个方法；

- 示例代码：

  ```	js
  // 基类-动物
  class Animal {
      constructor(name) {
          this.name = name;
      }
      sayName() {
          console.log(this.name, '--------基类>>>>>>>>Animal中的方法');
      }
  }
  
  let animal = new Animal('小动物');
  animal.sayName();
  
  // 子类，狗--继承于动物类
  class Dog extends Animal {
      constructor(name, age) {
          super(name);
          this.age = age;
      }
      sayName() {
          // 调用父类方法
          super.sayName();
          // 写子类自己的方法
          console.log(`${this.name}(${this.age})----------子类>>>>>>>>>>>Dog类中的方法`);
      }
  }
  
  let dog = new Dog('大黄', '7');
  dog.sayName();
  
  ```
  简不简单，惊不惊喜，意不意外，开不开心？

  > class使用就是这么简单，但是要知道这只是语法糖，es5中继承是如何实现呢？

# es5中继承的基本实现

- 几个关键字的使用：

  - function：类就是使用函数创建出来的；
  - prototype：原型属性；
  - Object.create(`__proto__`, `obj`)：用来创建对象
    - 参数一`__proto__`：必需，被创建对象的原型对象。
    - 参数二`obj`：可选，包含一个或多个属性描述符的 JavaScript 对象，用来规定要被添加到对象的属性，可以分为两类，数据属性和访问器属性

- 简单使用示例代码(从基本示例中引发思考，如何调用父级方法)：

  ```js
  // 基类-动物
  function Animal(name) {
      this.name = name;
  }
  
  Animal.prototype.sayName = function() {
      console.log(this.name, '--------基类>>>>>>>>Animal中的方法');
  }
  
  // 子类，狗--继承于动物类
  function Dog(name, age) {
      this.name = name;
      this.age = age;
  }
  
  // 将Dog的原型指向Animal的一个实例
  Dog.prototype = new Animal();
  // 因为Animal的实例可以调用Animal原型中的方法, 所以Dog的实例也可以调用Animal原型中的所有属性。
  Dog.prototype.sayName = function(argument) {
      console.log(`${this.name}(${this.age})----------子类>>>>>>>>>>>Dog类中的方法`);
  }
  var dog = new Dog('大黄', '7');
  dog.sayName();
  ```

# es6class创建和es5中function创建，异同点在哪里？

- 相同点：

  - 都分为声明式和表达式
  - 对待name,length表现一致
  - 对待创建变量结果一致***
  - fn对待相同，都可以用call,apply.bind来改变this指向

- 不同点：

  - 声明式，es5中的fn是存在变量提升的，es6中的class没有
  - Dog类， fn创建可以使用call调用，class创建的不能
  - 在全局中创建Dog类，fn可以在window中访问Dog,class生成的不能
  - 武昌补充。。。我没琢磨粗来


> 那在es6还没出来的时候，当初的开发人员是如何写类的继承呢？别天真了，人没有那么傻，别个早已经封装实现了此类方法。
  <video id="video" controls="" preload="none" poster="laugh.mp4">
    sorry,您的浏览器不支持该视频类型!
    <source id="mp4" src="laugh.mp4" type="video/mp4">
  </video>  
> 看看es5的时候，别个是如何使用继承的

```js
// 基类-动物
var Animal = Class.extend({
    // init是构造函数，起到contructor的作用
    init: function(name) {
        this.name = name;
    },
    sayName: function() {
        console.log(this.name, '--------基类>>>>>>>>Animal中的方法');
    }
});

var animal = new Animal('小动物');
animal.sayName();

// 子类，狗--继承于动物类
var Dog = Animal.extend({
    // init是构造函数
    init: function(name, age) {
        //  在构造函数中调用父类的构造函数
        this._super(name);
        this.age = age;
    },
    sayName: function() {
        // 调用父类方法
        this._super();
        // 写子类自己的方法
        console.log(`${this.name}(${this.age})----------子类>>>>>>>>>>>Dog类中的方法`);
    }
});

var dog = new Dog('大黄', '7');
dog.sayName();
```



# 看过别人的继承使用效果后，我们来实现一个类似的手动实现原型继承

- 示例看基本的继承

  ```js
  // 基类-动物
  function Animal(name) {
      this.name = name;
  }
  
  Animal.prototype.sayName = function() {
      console.log(this.name, '--------基类>>>>>>>>Animal中的方法');
  }
  // 子类，狗--继承于动物类
  function Dog(name, age) {
      this.name = name;
      this.age = age;
  }
  
  // 将Dog的原型指向Animal的一个实例
  Dog.prototype = new Animal();
  // 因为Animal的实例可以调用Animal原型中的方法, 所以Dog的实例也可以调用Animal原型中的所有属性。
  Dog.prototype.sayName = function() {
      console.log(`${this.name}(${this.age})----------子类>>>>>>>>>>>Dog类中的方法`);
  }
  var dog = new Dog('大黄', '7');
  dog.sayName();
  ```

- 上述示例的缺陷：

  1. 在创建Dog构造函数和原型时，就对Animal进行了实例化，这是不合适的。
  2. Dog的构造函数没法调用父类Animal的构造函数，导致在Dog构造函数中对name属性的重复赋值。
  3. Dog中的函数会覆盖Animal中的同名函数，没有重载的机制（和上一条是一个类型的问题）。
  4. 实现中有constructor属性的指向错误。
  5. 单一职责，不够通用。 

- 我们将其一个一个缺陷进行解决，最后便可以得到我们想要的封装结构,下面是完整代码

  ```js
  var Class = (function() {
    // 标识类的初始化是否完成，如果完成，便可以调用init进行初始化构造函数
    var initCompleteStatus = false;
    // 定义正则，边界为_super完整的字符串则是真
    var reg = /\b_super\b/;
  
    function parentClass() {} // 父级，初始化基类是空对象
  
    parentClass.extend = function(props) {
      function childClass() {
        if (initCompleteStatus) {
          this.init.apply(this, arguments);
        }
      }
      
      var _super = this.prototype; // 父级类的原型
      // 保存父级的所有属性为子类的原型，并重置constructor
        
      initCompleteStatus = false;
      // 此时的实例化不会调用init
      childClass.prototype = new this();
      initCompleteStatus = true;
        
      Object.defineProperty(childClass.prototype, "constructor", {
        value: childClass
      });
      
      // 自身属性的赋值
      for (var name in props) {
        if (typeof props[name] === "function" && reg.test(props[name])) {
          childClass.prototype[name] = (function(name, fn) {
            return function() {
              console.dir(Object.toString.call(fn));
              var temp = this._super;
              this._super = _super[name];
              // 拿到函数返回值
              var result = fn.apply(this, arguments);
              this._super = temp;
              // 将返回值返回
              return result;
            };
          })(name, props[name]);
        } else {
          childClass.prototype[name] = props[name];
        }
      }
      childClass.extend = arguments.callee;
      return childClass;
    };
    return parentClass;
  })();
  ```
