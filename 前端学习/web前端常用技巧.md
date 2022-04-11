## web前端常用技巧

[TOC]

### 1，过10s自动关闭页面

```javascript
function closeit() {
	setTimeout(function(){
		self.close();     
    },2000)
}
closeit();
```

###  2，css解决纯数字或字母不自动换行 

```css
word-break: break-all;
```

### 3，返回

#### a、返回上一个页面

```javascript
window.history.back(-1); 
```

#### b、返回上一页面并刷新

```javascript
//方法一
self.location = document.referrer;
```

```javascript
//方法二
//当前页面js
window.history.go(-1);//返回
//返回的页面
//浏览页面的时候，如果跳往其他页面，然后返回上一页，有时候并不会刷新（应该是缓存机制的原因），此时我们可以通过添加一段 js 代码强制刷新页面。
window.onpageshow = function (event) {
    if (event.persisted) {
        window.location.reload();
    }
};
```

>  [onpageshow](http://www.runoob.com/jsref/event-onpageshow.html) 事件在每次加载页面的时候都会触发，而 `event.persisted` 则用于判断页面是否从缓存中读取，若是则为 `true`。 

#### c、关闭当前页面

```
window.history.back(-1);
```

#### d、返回上一页面

```
window.history.go(-1);
//返回上上一页面
window.history.go(-2);
```

### 4，刷新页面

- history.go(0) 
- location.reload( [bForceGet] ) : *bForceGet， 可选参数， 默认为 false，从客户端缓存里取当前页。true, 则以 GET 方式，从服务端取最新的页面, 相当于客户端点击 F5("刷新")* 
- location=location 
- location.assign(location) 
- document.execCommand('Refresh') 
- window.navigate(location) 
- location.replace(location) 
- document.URL=location.href 

### 5，数组

#### 数组排序Array.sort()

1. 根据数组中的对象的某个属性值排序(值可以转为数值)

   ```javascript
   arr.sort(function(a,b){
       return a.id - b.id;
   })
   ```

2. 中文排序:  a.name.**localeCompare**(b.name);

   ```javascript
   arr.sort(function(a,b){
       return  a.name.localeCompare(b.name);
   });
   ```

3.  多个属性值排序，多条件排序

   ```javascript
   var arr6 = [{id:10,age:2},{id:5,age:4},{id:6,age:10},{id:9,age:6},{id:2,age:8},{id:10,age:9}];
   arr6.sort(function(a,b){
       if(a.id === b.id) {//如果id相同，按照age的降序
           return b.age - a.age
       }else{
           return a.id - b.id
       }
   });
   console.log(arr6);
   //输出新的排序:
   //{id: 2, age: 8}
   //{id: 5, age: 4}
   //{id: 6, age: 10}
   //{id: 9, age: 6}
   //{id: 10, age: 9}
   //{id: 10, age: 2}
   
   ```

#### 数组去重

```javascript
function noRepeat(arr) {
  return [...new Set(arr)];
}
```

#### 数组最大最小值

```javascript
//最大值
function arrayMax(arr) {
  return Math.max(...arr);
}
//最小值
function arrayMin(arr) {
  return Math.min(...arr);
}
```

#### 以size为长度的数组分割

```javascript
function chunk(arr, size = 1) {
  return Array.from(
    {
      length: Math.ceil(arr.length / size),
    },
    (v, i) => arr.slice(i * size, i * size + size)
  );
}
```

#### 检查数组某元素出现次数

```javascript
function countOccurrences(arr, value) {
  return arr.reduce((a, v) => (v === value ? a + 1 : a + 0), 0);
}
```

#### 对比两数组其中返回不同的元素

```javascript
function diffrence(arrA, arrB) {
  return arrA.filter((v) => !arrB.includes(v));
}
```

#### 

### 6，获取事件对象和防止冒泡

例子：

```html
<div onclick="handleClick(this)">
    <div onclick="chooseItem(this)">本月</div>
</div>
```

```javascript
//获取事件对象
function chooseItem(el) {
    //this.event获取事件对象
	this.event.stopPropagation();  //阻止事件冒泡
	let optValue = $(el).text();
}
```

### 7，查找字符串

####  indexOf() 

- 定义：indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。

- 语法：`stringObject.indexOf(searchvalue,fromindex)`

- 参数：

  | 参数        | 描述                                                         |
  | ----------- | ------------------------------------------------------------ |
  | searchvalue | 必需。规定需检索的字符串值。                                 |
  | fromindex   | 可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 stringObject.length - 1。如省略该参数，则将从字符串的首字符开始检索。 |

- 返回： 该方法将从头到尾地检索字符串 stringObject，看它是否含有子串 searchvalue。开始检索的位置在字符串的 fromindex 处或字符串的开头。如果找到一个 searchvalue，则返回 searchvalue 的第一次出现的位置。stringObject 中的字符位置是从 0 开始的。 

#### lastIndexOf() 

- 定义：lastIndexOf() 方法可返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。  如果要检索的字符串值没有出现，则该方法返回 -1。 

- 语法：`stringObject.lastIndexOf(searchvalue,fromindex)`

- 参数：

  | 参数          | 描述                                                         |
  | :------------ | :----------------------------------------------------------- |
  | *searchvalue* | 必需。规定需检索的字符串值。                                 |
  | *fromindex*   | 可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 *stringObject*.length - 1。如省略该参数，则将从字符串的最后一个字符处开始检索。 |

- 返回： 如果在 *stringObject* 中的 *fromindex* 位置之前存在 *searchvalue*，则返回的是出现的最后一个 *searchvalue* 的位置。  如果要检索的字符串值没有出现，则该方法返回 -1。 

### 8，input上传图片

- 选取相册或调用摄像头

  ```html
   <label for="uploadImg" 
          id="choose-file"
          onclick="chooseFile();">
  	<input  type="file"     	//读取文件
  			accept="image/*"    //接收图片
  			name="uploadImg" 
  			id="uploadImg" 
  			style="display: none;"
          	multiple="multiple" />  //多张上传
  </label>
  ```

  ```javascript
  //选取图片的来源，拍照和相册  
  function chooseFile() {
  	var input = document.getElementById('uploadImg');
  	input.value = "";  //解决onchange调用两次
  	input.onchange = function(e) {		
  		//读取图片
  		readFile(this);
  	}
  }
  ```

> **在onchange之前将input的value清空:  解决onchange调用两次问题**

- 读取图片

  ```javascript
  function readFile(obj) {
  	let files = obj.files;
  	for (let i = 0; i<files.length; i++) {
  		var file = files[i];
  		//保存到图片数组
  		saveFile(file);	
  		//压缩图片并展示
  		var imgsrc;
  		var fileType = file.type; //图片类型
  		var reader = new FileReader();
  		reader.readAsDataURL(file);
  		if (!/^image\/\w+$/.test(fileType)) {
  			alert('文件格式错误,请选择图片');
  			return;
  		}
  		reader.onload = function(e) {
  			imgsrc = this.result; //输出结果
  			// 压缩
  			var image = new Image(); //新建图片
  			image.src = imgsrc;
  			image.onload = function(e) {
  				var cvs = document.createElement('canvas'); //画布
  				var cvx = cvs.getContext('2d'); 
  				cvs.width = this.width;
  				cvs.height = this.height;
  				cvx.drawImage(this, 0, 0, this.width, this.height); //画图
                  //这是压缩，具体的看.toDataURL api 输出base64
  				var newImageData = cvs.toDataURL(fileType, 0.2); 
                  //展示压缩图片
  				showImg(newImageData, imgIndex);
  				imgIndex++;
  			};
  		}
  	}	
  }
  ```

- 保存图片

  ```javascript
  //保存图片数组
  var imgFiles = [];
  var imgIndex = 0;
  function saveFile(file) {
  	imgFiles.push(file);
  }
  ```

- 展示压缩图片

  ```java
  //展示压缩图片
  function showImg(src, index) {
  	var html = '';
  	html +=
  		`
  		<div id="Img${index}" class="image-show">
  			<img src="${src}"/>
          	<!-- 用于删除图片 -->
  			<span class="del" onclick = "delImg(this)"> 
  			</span>
  		</div>	
  	`;
  	$("#show-img-box").append(html);
  }
  ```

- 删除图片

  ```javascript
  //删除图片
  function delImg(dom) {
  	let divs = $('#show-img-box').find('.image-show');
      //获取删除图标元素的父元素
  	let delDiv = $(dom).parent();
      //获取删除图标元素的父元素对应索引
  	let index = divs.index(delDiv);
  	//删除图片数组相应索引的元素
  	imgFiles.splice(index, 1);
  	//删除展示区压缩图片
  	delDiv.remove();
  }
  ```

- 上传图片

  ```javascript
  var formData = new FormData(); // 创建form对象
  for (var i = 0; i < imgFiles.length; i++) {
      // 通过append向form对象图片数组数据
  	formData.append('file', imgFiles[i]); 
  }
  formData.append('other', other); // 传替其他参数
  //上传服务器
  $.ajax({
  	url: '***',
  	type: 'POST',
  	cache: false,//上传文件不需要缓存
  	data: formData,
  	processData: false,//不要去处理发送的数据
  	contentType: false,//不要去设置Content-Type请求头
      success: function(data) {
          //dosomething...
      }
  })
  ```


### 9，AES加解密

### 10，ajax请求成功，但是跳到error的一种情况

> 后端返回String，前端ajax需要用text接收

```javascript
$.ajax(API_host.path_join('/user/login'), {
	data: loginInfo,
	dataType: 'text', //服务器返回String格式数据,前端用text
	type: 'post', //HTTP请求类型
	success: function(obj) {
		//dosomething...
	}
});
```

### 11，正则替换字符串

```javascript
//将括号及括号内字符串替换
let titleName = '临川区饲料（单一饲料、浓缩饲料、配合饲料、精料补充料）生产企业委托生产备案';
let reg = /[(（](.*?)[)）]/gm;
let newStr = titleName.replace(reg,'');
```

> replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。
>
> ```
> stringObject.replace(regexp/substr,replacement)
> ```

### 12，CORS预检请求触发条件

1. ​	使用了下面任一HTTP 方法：	

   ```javascript
   PUT/DELETE/CONNECT/OPTIONS/TRACE/PATCH	 
   ```

2. 人为设置了以下集合之外首部字段：

   ```javascript
   Accept/Accept-Language/Content-Language/Content-Type/DPR/Downlink/Save-Data/Viewport-Width/Width	 
   ```

3. Content-Type 的值不属于下列之一:	 

   ```javascript
   application/x-www-form-urlencoded、multipart/form-data、text/plain
   ```

   

### 13，二维码生成(使用jquery.qrcode)

```html
<div id="qrcode-container" style="visibility: hidden;"></div>
<div id="qrcode"></div>
```

```javascript
function createQr(qrcode) {
    document.createElement('canvas').getContext('2d');
    $('#qrcode-container').qrcode({
        render: "canvas",
        height: 60,
        width: 60,
        correctLevel: 0,
        text: qrcode
    });
    //获取网页中的canvas对象
    var mycanvas1 = document.getElementsByTagName('canvas')[0];
    //将转换后的img标签插入到html中
    var img = convertCanvasToImage(mycanvas1);
    $('#qrcode').append(img); //imgDiv表示你要插入的容器id
}
//从canvas中提取图片image
function convertCanvasToImage(canvas) {
    //新Image对象，可以理解为DOM
    var image = new Image();
    // canvas.toDataURL 返回的是一串Base64编码的URL
    // 指定格式PNG
    image.src = canvas.toDataURL("image/png");
    return image;
}
```

### 14，window.location对象

- window.location对象包含的属性

  | 属性     | 描述                              |
  | -------- | --------------------------------- |
  | hash     | 从井号 (#) 开始的 URL（锚）       |
  | host     | 主机名和当前 URL 的端口号         |
  | hostname | 当前 URL 的主机名                 |
  | href     | 完整的 URL                        |
  | pathname | 当前 URL 的路径部分               |
  | port     | 当前 URL 的端口号                 |
  | protocol | 当前 URL 的协议                   |
  | search   | 从问号 (?) 开始的 URL（查询部分） |

- window.location.search用法

  ```javascript
  //'http://localhost:8081/***/***?id=12345';  当前页面的href
  console.log(window.location.search);//"?id=12345"
  //获取href参数
   window.location.search.substring(1);
  ```

  

### 15，小程序、uni-app使用formData传值

- 普通键值对:  只需需改请求header中content-type

  ```javascript
  header: {
      'content-type': 'application/x-www-form-urlencoded' //修改此处即可
    }
  ```

- 上传文件: 使用uploadFile

  ```javascript
  wx.uploadFile({
  	url: this.baseUrl + '/api/real/photoUpload',
  	name: 'file',
  	headers: {
  		'Content-Type': 'multipart/form-data'
  	},
  	formData: {
  		'phoneNum': phoneNum,
          //...
  	},
  	success: (res) => {}
  })
  ```

  

### 16，HTML标签上标、下标

- 上标: 在HTML语言中， <sup> 标签可定义上标文本。

  例如表示m²：

  ```html
  <th class="table-td1">流量m<sup>2</sup>/s</th>  
  ```

  > 包含在  `<sup>`标签和其结束标签 `</sup>`中的内容将会以当前文本流中字符高度的一半来显示，但是与当前文本流中文字的字体和字号都是一样的。
  > 提示： 这个标签在向文档添加脚注以及表示方程式中的指数值时非常有用。如果和 <a> 标签结合起来使用，就可以创建出很好的超链接脚注。

- 下标:  在HTML语言中， <SUB>标签可定义下标文本。

  例如表示h<SUB>2</SUB>O:

  ```html
  <div>h<SUB>2</SUB></div>
  ```


### 17，数字

#### 数字千分位分割

```javascript
function commafy(num) {
  return num.toString().indexOf(".") !== -1
    ? num.toLocaleString()
    : num.toString().replace(/(\d)(?=(?:\d{3})+$)/g, "$1,");
}
```



### 18，web Socket

```javascript
var ws = new WebSocket("ws://192.168.3.150:8082/kuping/imserver/10");//地址用ws协议
//连接成功
ws.onopen = function(){
	//连接成功并向服务器发送信息
	ws.send('hello server');
};

//监听服务器消息
ws.onmessage = function(e){
	var msg = e.data;     /*服务器发送一条消息*/
	console.log('收到服务器发来的消息：' + msg);     
};
//关闭连接
ws.onclose = function(){
	console.log('关闭与服务器的连接！');
};
//连接错误
ws.onerror = function(e) {
	/*出错了*/
};
```



### 19，单行写评级组件

"★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);

> 定义一个变量rate，取值从1-5即可，1为一星，5为五星



### 20，每个元素加框线

```javascript
[].forEach.call($$("*"),function(a){
    a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)
})
```

翻译成js语言

```js
Array.prototype.forEach.call(document.querySelectorAll('*'), 
dom => dom.style.outline = `1px solid #${parseInt(Math.random() *
Math.pow(2,24)).toString(16)}`)
```

### 21，优雅取整

```javascript
var a = ~~2.33
var b= 2.33 | 0
var c= 2.33 >> 0
```

### 22，金钱格式化

```javascript
var test1 = '1234567890'
var format = test1.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
console.log(format) // 1,234,567,890
```

### 23，数组去重

```js
[...new Set([1, "1", 2, 1, 1, 3])]//[1,"1",2,3]
```

### 24，实现一个长度为m,且值都为n的数组

```js
Array(m).fill(n)
```

### 25，取出数组中的最大值和最小值

```js
var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; 
var maxInNumbers = Math.max.apply(Math, numbers); //122205
var minInNumbers = Math.min.apply(Math, numbers); //-85411
```

