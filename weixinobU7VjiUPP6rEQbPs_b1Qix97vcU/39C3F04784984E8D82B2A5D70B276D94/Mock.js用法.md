

[TOC]



# Mock.js用法

## <u>***安装***</u>

### Node (CommonJS)

```javascript
# 安装
npm install mockjs
// 使用 Mock
var Mock = require('mockjs')
var data = Mock.mock({
    // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id|+1': 1
    }]
})
```

### Bower

```javascript
# 安装
npm install -g bower
bower install --save mockjs
<script type="text/javascript" src="./bower_components/mockjs/dist/mock.js"></script>
```

### 标签引入

```javascript
<script type="text/javascript" src="http://mockjs.com/dist/mock.js"></script>
```

## <u>***使用***</u>

### 一般场景配置两个参数够用了Mock.mock( rurl, template )

- Mock.mock( rurl?, rtype?, template|function( options ) )
- rurl（可选）。表示需要拦截的 URL，可以是 URL 字符串或 URL 正则。例如 //domain/list.json/、'/domian/list.json'。
- rtype（可选）。表示需要拦截的 Ajax 请求类型。例如 GET、POST、PUT、DELETE 等。
- template （可选）。表示数据模板，可以是对象或字符串。例如 { 'data|1-10':[{}] }、'@EMAIL'。
- function(options)（可选）。表示用于生成响应数据的函数。
- options 指向本次请求的 Ajax 选项集，含有 url、type 和 body 三个属性

```javascript
  /*
   * Mock.mock( rurl, template )
   * rurl:表示需要拦截的 URL，可以是 URL 字符串或 URL 正则。
   * template：表示数据模板，可以是对象或字符串
   * 
   */
Mock.mock(/\.json/, {
	'code':'000',
    'message':'message信息',
    // 属性 list 的值是一个数组，随机生成1-10个元素
    'list|1-10': [{
    	'id|+1': 1, // 属性id是一个自增1的数，起始值为1
        'email': '@EMAIL',
        'name': '@name'
	}]
});
$.ajax({
	url:'list.json',
    contentType: 'application/json',
    type: 'POST',
    timeout: 5000,
    dataType:'JSON',
    data: JSON.stringify({schoolId: 123}),
    success: function (res) {
       console.log(res);
    }
});
```

***输出结果***：每次打开页面或刷新一次页面数据会随机改变

```javascript
{
   "code": "000",
   "message": "message信息",
   "list": [
       {
           "id": 1,
           "email": "u.skubdwsbs@nfuzlwgd.et",
           "name": "Michael Clark"
       },
       {
           "id": 2,
           "email": "u.sokjrkoja@lnmxlkx.kp",
           "name": "Brian Davis"
       },
       {
           "id": 3,
           "email": "t.chpjw@jnidn.se",
           "name": "Margaret Allen"
       }
   ]
}
```

### 可选配置：拦截ajax请求时的行为

- Mock.setup( settings );
- settings必选，支持的配置项有：timeout;
- 指定被拦截的 Ajax 请求的响应时间，单位是毫秒;
- 例如：配置`Mock.setup({timeout: 4000})`之后网页打开4000毫秒之后才会返回数据;

```javascript
Mock.setup({
	timeout: 4000
})
```

### 数据模板中的每个属性由 3 部分构成：属性名、生成规则、属性值：

```
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

**注意：**

- *属性名* 和 *生成规则* 之间用竖线 `|` 分隔。

- *生成规则* 是可选的。

- 生成规则：

  ​        有 7 种格式

  1. `'name|min-max': value`
  2. `'name|count': value`
  3. `'name|min-max.dmin-dmax': value`
  4. `'name|min-max.dcount': value`
  5. `'name|count.dmin-dmax': value`
  6. `'name|count.dcount': value`
  7. `'name|+step': value`

- **生成规则的含义需要依赖属性值的类型才能确定。**

- *属性值* 中可以含有 `@占位符`。

- *属性值* 还指定了最终值的初始值和类型。

```javascript
'list|1-10': [{
	'id|+1': 1, // 属性id是一个自增1的数，起始值为1
	'email': '@EMAIL',  //@占位符
	'name': '@name'
}]
```

- *占位符* 只是在属性值字符串中占个位置，并不出现在最终的属性值中。

  *占位符* 的格式为：

  ```
  @占位符
  @占位符(参数 [, 参数])
  ```

  **注意：**

  1. 用 `@` 来标识其后的字符串是 *占位符*。
  2. *占位符* 引用的是 `Mock.Random` 中的方法。
  3. 通过 `Mock.Random.extend()` 来扩展自定义占位符。
  4. *占位符* 也可以引用 ***数据模板* 中的属性**。
  5. *占位符* 会优先引用 ***数据模板* 中的属性**。
  6. *占位符* 支持 *相对路径* 和 *绝对路径*。

```javascript
Mock.mock({
    name: {
        first: '@FIRST',
        middle: '@FIRST',
        last: '@LAST',
        full: '@first @middle @last'
    }
})
// =>
{
    "name": {
        "first": "Charles",
        "middle": "Brenda",
        "last": "Lopez",
        "full": "Charles Brenda Lopez"
    }
}
```

-   Mock.Random 是一个工具类，用于生成各种随机数据。

  - Mock.Random 提供的完整方法（占位符）如下：
  - 基本：boolean, natural, integer, float, character, string, range,
  - 时间：date, time, datetime, now；
  - 图片：image, dataImage；
  - 颜色：color，hex，rgb，rgba，hsl；
  - 文本：paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle；
  - 名字：first, last, name, cfirst, clast, cname；
  - web：url, domain, email, ip, tld；
  - 地址：region，province，city，county，zip；
  - Helper：capitalize, upper, lower, pick, shuffle；
  - Miscellaneous：guid, id, increment;

  使用方法：在模板数据中定义参数,值为@加上对应的占位符用引号引起来，比如`'nowTime':'@now'`,请求返回的数据就是当前发送请求时的时间 "2017-09-02 11:42:06".

  ## 想了解更详细的使用，请参考[官方文档](https://github.com/nuysoft/Mock/wiki)

