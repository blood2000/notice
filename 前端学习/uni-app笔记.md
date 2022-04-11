## uni-app笔记

[toc]

### 导航栏添加按钮

1，打开pages.json文件

2，在里面加上按钮

3，运行顶部就出现按钮

4，在对应的页面编写onNavigationBarButtonTap事件，名字要一样

5，通过index来判断是哪个按钮

6，最后如果多个按钮就可以从左到右加index值

### 页面传值

当前页面

```js
//先对需要传的值（此次为对象类型数据）
//encodeURIComponent加密传递的对象数据
let item = encodeURIComponent(JSON.stringify(this.targetObj))
uni.navigateTo({
	url: './editTicket?item=' + item
})
```

跳转页面

```js
onLoad(option) {
    // decodeURIComponent 解密传过来的对象字符串
	const item = JSON.parse(decodeURIComponent(option.item));
},
```

### uni-app选择器picker注意事项

*如果指定rang-key中的字符串，需要加上单引号才能生效。例子*

```vue
<-->rang-key中指定的键名需要加单引号</-->
<picker mode="selector" :range="dutys" :range-key="'name'"  :value="'value'"  @change="changeDuty">
	<view class="picker-btn">{{dutys[dutyIndex].name}}</view>
</picker>
```

### 修改表单的值

```js
setTimeout(() => {  
	this.value = e.detail.value
}, 0)
```

### uni-app input组件传参----$event

> @input事件想要传递一个参数到方法中 但是同时还要保留原来返回的event
>
> 解决方法：$event

```js
@input="onKeyInput($event,123)"
```

### 监听键盘回车

>  confirm-type设置键盘右下角按钮的文字，仅在 type="text" 时生效。 默认值为'done'，可以设为'search'

```js
<input confirm-type="search" @confirm="doSearch">
```

### 动态修改导航栏

> 使用原生导航栏，通过API 动态修改，pages.json 里面的配置原始的值不要改变，
> 如果需要在页面进入时设置标题，可以在onReady内执行，以避免被框架内的修改所覆盖。如果必须在onShow内执行需要延迟一小段时间，
> 在指定页面使用如下代码：

```js
uni.setNavigationBarTitle({
    title: '新的标题'
});
uni.setNavigationBarColor({
    frontColor: '#000000', //前景颜色值，包括按钮、标题、状态栏的颜色，仅支持 #ffffff 和 #000000
    backgroundColor: '#F0AD4E', //背景颜色值，有效值为十六进制颜色
    animation: {  //动画效果
        duration: 400,
        timingFunc: 'easeIn'
    }
})
```

### 页面后退传参

1，需要传参的页面：

```js
onShow() {
	var pages = getCurrentPages();
	var currPage = pages[pages.length - 1]; //当前页面
	let res = currPage.data;   //为传过来的值
}
```

2，传参页面：

```js
//methods中使用
confirm() {
	//回传参数到上一页
	var pages = getCurrentPages();
	var prevPage = pages[pages.length - 2]; //上一个页面
	prevPage.data = this.param;  //参数
	uni.navigateBack({
		delta: 1
	})
},
```

### 复制与粘贴 

复制内容到剪贴板

```js
uni.setClipboardData({
	data: copyContent,  //需要复制的内容
	success: function () {
		console.log('success');
	}
});
```

粘贴（获取剪贴板内容）

```js
uni.getClipboardData({
    success: function (res) {
        //res.data为剪贴板内容
        console.log(res.data);
    }
});
```

### 下拉刷新

```js
//和生命周期同级别: 监听下拉刷新
onPullDownRefresh() {
	//doSomething...
	setTimeout(function() {
		uni.stopPullDownRefresh();  //停止下拉刷新监听
	}, 1000);
			
},
```

### uni-app设置动态入口

由于uni-app只能是固定入口文件，在app启动时判断场景然后再跳转到相应的页面。但是uni-app不论如何判断都会进入到固定的入口文件中开始渲染页面，待到逻辑判断结束再跳转到相应的页面，这种体验非常不好。

**解决方案**: **通过延长启动页停留时间，等到onLaunch方法执行完毕之后再关闭启动页**，这样就不会出现页面闪烁和跳转的痕迹。

1.mainfest.json：设置启动页关闭方式为手动

```json
"splashscreen" : {
    "alwaysShowBeforeRender" : false,
    "waiting" : false,
    "autoclose" : false, // 设置为手动关闭启动
    "delay" : 0
}
```

2.App.vue

```js
// 判断登陆状态
const token = uni.getStorageSync('token')
if(token) { // 有登陆状态
    plus.navigator.closeSplashscreen()
}else { // 未登陆
    uni.reLaunch({
        url: 'pages/login/login',
        success() {
    // 手动关闭启动页(由于App.onLaunch方法和页面渲染是同时进行的，只有通过延长启动页的方法才能避免页面闪烁)
            plus.navigator.closeSplashscreen()
        }
    })
}
```



### 日期格式化在ios真机bug

由于ios的js中Date类问题，日期格式为`yyyy-mm-dd`时，new Date(time)返回空对象，需要将分隔符换位`/`即可

```js
let date = '2021-11-15 14:33';
date = date.replace(/-/g, '/');
date = new Date(date);
```

### uni-app修饰符组织时间冒泡

@click.stop在uni-app中失效，可以使用：`@click.native.stop`

```vue
<uni-icons type="clear" @click.native.stop="clearOption('delete')"></uni-icons>
```

