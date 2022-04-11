# js寄生组合式继承

[TOC]

### extend函数有效扩展

```javascript
function extend(subClass,superClass) {
    var F = function(){};
    F.prototype = superClass.prototype;
    subClass.prototype = new F(); //重写了subClass的原型，此时subClass.prototype.constructor是F，因此：
    subClass.prototype.constructor = subClass;
    //以下这句只是为了加强子类，并无太大用处
  	subClass.superclass = superClass.prototype;
    if(superClass.prototype.constructor == Object.prototype.constructor) {
    superClass.prototype.constructor = superClass;
  }
}
```

### 寄生组合式继承

最终,原型链继承可以这么实现,例如:

```javascript
function Father(name) {
    this.name = name;
    this.color = ['red','blue','green'];
}
Father.prototype.sayName = function(){
	console.log(this.name);
};

function Son(name,age) {
    Father.call(this,name);//继承实例属性，第一次调用Father()
    this.age = age;
}
extend(Son,Father); //继承父类方法,此处并不会第二次调用Father()
Son.prototype.sayAge = function(){
	console.log(this.age);
}
var instance1 = new Son("louis",5);
instance1.colors.push("black");
console.log(instance1.colors);//"red,blue,green,black"
instance1.sayName();//louis
instance1.sayAge();//5

var instance1 = new Son("zhai",10);
console.log(instance1.colors);//"red,blue,green"
instance1.sayName();//zhai
instance1.sayAge();//10
```

