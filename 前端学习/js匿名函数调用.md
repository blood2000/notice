### JS中立即执行函数的理解
1.匿名函数不能单独定义，必须进行赋值操作或者立即执行，否则会被JS引擎定义为语法错误
```javascript
function(){
    console.log(dada);
}
//报错:SyntaxError: Unexpected token
```

2.在函数体后面加括号就能立即调用，这个函数形式必须是函数表达式，不能是函数声明
```javascript
function(){
    console.log(123);
}();
//报错:SyntaxError: Unexpected token (
```

3.可以在函数前面加符号，或者用括号将函数包住来消除函数声明
```javascript
(function(){
    console.log(123);
})();
//123
```

**4.消除函数声明最安全的做法是加括号，因为运算符号还会和函数的返回值进行运算，造成不必要麻烦**

5.包住函数表达式的括号可以括住参数，也可以不括住，效果是一样的
```javascript
(function(){
    console.log(123);
}());
```
