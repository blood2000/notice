# Class类

[toc]

## 定义类

>  类语法**不会**为JavaScript引入新的面向对象的继承模型。 

 类实际上是个“特殊的[函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions)”，就像你能够定义的[函数表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/function)和[函数声明](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function)一样，类语法有两个组成部分：[类表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/class)和[类声明](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class)。 

### **类声明**

#### 要声明一个类，你可以使用带有`class`关键字的类名 

 ```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
 ```

#### *提升*

 **函数声明**和**类声明**之间的一个重要区别是函数声明会[提升](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)，*类声明不会*。你首先需要声明你的类，然后访问它，否则像下面的代码会抛出一个[`ReferenceError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)： 

```javascript
let p = new Rectangle(); 
// ReferenceError
class Rectangle {}
```

### **构造函数**

[constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor)方法是一个特殊的方法，这种方法用于创建和初始化一个由`class`创建的对象。***一个类只能拥有一个名为 “constructor”的特殊方法***。如果类包含多个`constructor`的方法，则将抛出 一个[`SyntaxError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) 。

一个构造函数可以使用 `super` 关键字来调用一个父类的构造函数。

### **静态方法**

 `static` 关键字用来定义一个类的一个静态方法。调用静态方法不需要[实例化](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript#The_object_(class_instance))该类，**但不能通过一个类实例调用静态方法**。*静态方法通常用于为一个应用程序创建工具函数。* 

```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    static distance(a, b) {
        const dx = a.x - b.x;
        const dy = a.y - b.y;
        //Math.hypot求参数平方和的平方根(直角三角求斜边)
        return Math.hypot(dx, dy);
    }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);

console.log(Point.distance(p1, p2));
```

### **this指向**