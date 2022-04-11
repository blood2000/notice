[TOC]

#### 1.Object.assign()基本用法：

Object.assign方法用来将源对象（source）的所有可枚举属性，复制到目标对象（target）。它至少需要两个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。

```javascript
let targetObj = {a:1};
let sourceObj1 = {b:2};
let sourceObj2 = {c:3};
Object.assign(targetObj,sourceObj1,sourceObj2);
console.log(targetObj)//{a:1,b:2,c:3};
```

_注：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。_
```javascript
let targetObj1 = {
    a: 1,
    b: 2
};
let sourceObj1 = {
    b: 1
};
let sourceObj11 = {
    c: 3
};
Object.assign(targetObj1, sourceObj1, sourceObj11);
console.log(targetObj1);//{a:1,b:1,c:3}
```
—注：该方法是对象的浅拷贝，深拷贝参考lodash的_.defaultsDeep方法，解决深拷贝问题
#### 2，用途
```java
1，给对象添加属性；
2，给对象添加方法；
3，克隆对象；
```
```javascript
function copyFnc(origin) {
    return Object.assign({}, origin)
}
var sur = { a: 1, b: 2 };
console.log(copyFnc(sur));
```
上面代码将原始对象拷贝到一个空对象，就得到了原始对象的克隆。
不过，采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值。如果想要保持继承链，可以采用下面的代码。
```javascript
function clone(origin) {
    let originProto = Object.getPrototypeOf(origin);
    return Object.assign(Object.create(originProto), origin);
}
```
在JS里子类利用Object.getPrototypeOf去调用父类方法,用来获取对象的原型。
