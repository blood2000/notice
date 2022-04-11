## js数组对象方法

[toc]

###  **Array.find** 

- 定义：

  find() 方法返回通过测试（函数内判断）的数组的第一个元素的值。

  find() 方法为数组中的每个元素都调用一次函数执行：

  - 当数组中的元素在测试条件时返回 *true* 时, find() 返回符合条件的元素，之后的值不会再调用执行函数。
  - 如果没有符合条件的元素返回 undefined

  ***注意: find() 对于空数组，函数是不会执行的。***

  ***注意: find() 并没有改变数组的原始值。***

- 用法：array.find(function(currentValue, index, arr),thisValue)

- 参数：

  | 参数                                  | 描述                                                         |
  | :------------------------------------ | ------------------------------------------------------------ |
  | **function(currentValue, index,arr)** | 必需。数组每个元素需要执行的函数                             |
  | *thisValue*                           | 可选。 传递给函数的值一般用 "this" 值。如果这个参数为空， "undefined" 会传递给 "this" 值 |

  ****

  ***函数参数:***  

  | 参数             | 描述                         |
  | ---------------- | ---------------------------- |
  | **currentValue** | 必须，当前元素               |
  | *index*          | 可选，当前元素索引           |
  | *arr*            | 可选，当前元素所属的数组对象 |

  ****

  ***返回值：***

  | **返回值**          | 返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回 **undefined**。 |
  | :------------------ | :----------------------------------------------------------- |
  | **JavaScript版本:** | ECMAScript 6                                                 |

- 实例：

  ```javascript
  const pets = [
   { type: 'Dog', name: 'Max'},
   { type: 'Cat', name: 'Karl'},
   { type: 'Dog', name: 'Tommy'},
  ]
  let pet = pets.find(function(item){
        if (item.type ==='Dog' && item.name === 'Tommy') {
            return item;
        }
      });
   console.log(pet);  //{ type: 'Dog', name: 'Tommy' } 
  ```

### **JavaScript findIndex**方法

- 定义：

  findIndex() 方法返回传入一个测试条件（函数）符合条件的数组第一个元素位置。

  findIndex() 方法为数组中的每个元素都调用一次函数执行：

  - 当数组中的元素在测试条件时返回 *true* 时, findIndex() 返回符合条件的元素的索引位置，之后的值不会再调用执行函数。
  - 如果没有符合条件的元素返回 -1

  **注意:** findIndex() 对于空数组，函数是不会执行的。

  **注意:** findIndex() 并没有改变数组的原始值。

- 语法：

  ```javascript
  array.findIndex(function(currentValue, index, arr), thisValue)
  ```

- 参数：

  | 参数                                | 描述                                                         |
  | ----------------------------------- | ------------------------------------------------------------ |
  | *function(currentValue, index,arr)* | 必须。数组每个元素需要执行的函数                             |
  | *thisValue*                         | 可选。 传递给函数的值一般用 "this" 值。如果这个参数为空， "undefined" 会传递给 "this" 值 |

  ****

  ***函数参数：***

  | 参数           | 描述                         |
  | -------------- | ---------------------------- |
  | *currentValue* | 必须，当前元素               |
  | *index*        | 可选，当前元素索引           |
  | *arr*          | 可选。当前元素所属的数组对象 |

  ****

  ***返回值：***

  | 返回值               | 返回符合测试条件的第一个数组元素索引，如果没有符合条件的则返回 -1。 |
  | -------------------- | ------------------------------------------------------------ |
  | **JavaScript 版本:** | ECMAScript 6                                                 |

- 实例：

  ```javascript
  //返回符合大于指定数字的数组索引：
  var ages = [4, 12, 16, 20];
  function checkAdult(age) {
      return age >= 10
  }
   
  function myFunction() {
      return ages.findIndex(checkAdult);
  }
  myFunction();  //返回：1
  ```

  