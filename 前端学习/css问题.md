### css问题

[TOC]

### １、常见兼容性问题？

- 浏览器默认的 margin 和 padding 不同。解决方案是加一个全局的 *{margin: 0; padding: 0;} 来统一。
- IE下 event 对象有 event.x，event.y 属性，而 Firefox 下没有。Firefox 下有 event.pageX，event.PageY 属性，而 IE 下没有。 解决办法：var mx = event.x ? event.x : event.pageX;
- Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示, 可通过加入 CSS 属性 -webkit-text-size-adjust: none; 解决.
- 超链接访问过后 hover 样式就不出现了，被点击访问过的超链接样式不在具有 hover 和 active 了，解决方法是改变 CSS 属性的排列顺序: L-V-H-A : a:link {} a:visited {} a:hover {} a:active {}

### ２、如果让你来制作一个访问量很高的大型网站，你会如何来管理所有 CSS 文件、JS 与图片？

 回答：涉及到人手、分工、同步；

- 先期团队必须确定好全局样式（globe.css），编码模式 (utf-8) 等
- 编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；
- 标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；
- 页面进行标注（例如页面模块开始和结束）；
- CSS 跟 HTML 分文件夹并行存放，命名都得统一（例如 style.css）
- JS 分文件夹存放，命名以该 JS 功能为准
- 图片采用整合的 png8 格式文件使用，尽量整合在一起使用，方便将来的管理。

### ３、视差滚动效果 ？

视差滚动（Parallax Scrolling）通过在网页向下滚动的时候，`控制背景的移动速度比前景的移动速度慢` 来创建出令人惊叹的 3D 效果。

- CSS3 实现。 优点：开发时间短、性能和开发效率比较好，缺点是不能兼容到低版本的浏览器
- jQuery 实现。 通过控制不同层滚动速度，计算每一层的时间，控制滚动效果。优点：能兼容到各个版本的，效果可控性好。缺点：开发起来对制作者要求高。
- 插件实现方式。 例如：parallax-scrolling，兼容性十分好。

### ４、怎么让 Chrome 支持小于 12px 的文字 ？

```css
p {
  font-size: 10px;
  -webkit-transform: scale(0.8);  // 0.8 是缩放比例
} 
```

### ５、有一个高度自适应的 div，里面有两个 div，一个高度 100px，如何让另一个填满剩下的高度 ？

- 外层 div 使用 position：relative；
- 高度要求自适应的 div 使用 position: absolute; top: 100px; bottom: 0; left: 0

### 6、文字太长显示省略号

```css
//单行显示
width: 110px;
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
//双行显示
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
word-break: break-all;
```



### 7、奇数行、偶数行样式

```css
.list ul li:nth-child(even){ ...}    /*li的偶数行样式*/
.list ul li:nth-child(odd){ ...}    /*li的奇数行样式*/
/** 部分情况下的解决方案**/
.list li:nth-of-type(odd){ margin-left: 20px;}奇数行   
.list li:nth-of-type(even){margin-left: 10px;}偶数行   
```

### 8、滚动条设置

```css
::-webkit-scrollbar {
	width: 0;
	height: 20px;
	background-color: #0E1013;
}
::-webkit-scrollbar-thumb {
	background: #333;
}
```

