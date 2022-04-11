[TOC]



## this,call(),apply()

### *apply()*

apply() 是函数对象的一个方法，它的作用是改变函数的调用对象，它的第一个参数就表示改变后的调用这个函数的对象。因此，this 指的就是这第一个参数。    　　

```javascript
var x = 0;    　　
function test(){    　　　　
  alert(this.x);    　　
}    　　
var o = {};    　　
o.x = 1;    　　
o.m = test;    　　
o.m.apply(); // 0    
```

apply() 的参数为空时，**默认调用全局对象**。因此，这时的运行结果为 0，证明 this 指的是全局对象。     如果把最后一行代码修改为

```javascript
o.m.apply(o); // 1
```

运行结果就变成了 1，证明了这时 this 代表的是对象 o。

### ***apply 和 call  什么含义，什么区别 ？什么时候用 ？***

call，apply 都属于 Function.prototype 的一个方法，它是 JavaScript 引擎内在实现的.

```javascript
foo.call(this, arg1, arg2, arg3) == foo.apply(this,arguments) == this.foo(arg1, arg2, arg3);
```

- 相同点：两个方法产生的作用是完全一样的。
- 不同点：方法传递的参数不同。

每个函数对象会有一些方法可以去修改函数执行时里面的 this，比较常见得到就是 call 和 apply，通过 call 和 apply 可以重新定义函数的执行环境，即 this 的指向。

```javascript
function add(c, d) {
  console.log(this.a + this.b + c + d);
}

var o = { a: 1, b: 3 };
add.call(o, 5, 7);    //1+3+5+7=16
//传参的时候是扁平的把每个参数传进去

add.apply(o, [10, 20]);   //1+3+10+20=34
//传参的时候是把参数作为一个数组传进去

//什么时候使用 call 或者 apply
function bar() {
  console.log(Object.prototype.toString.call(this));
  // 用来调用一些无法直接调用的方法
}

bar.call(7); // "[object Number]"
```

