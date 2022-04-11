## VUE技巧

[toc]

### 一、Vue.observable手写一个状态管理

非大型项目，一般不用vuex进行状态管理，可以用新的API--Vue.observable手动打造一个Vuex

#### 1，创建store

```javascript
import Vue from 'vue'
// 通过Vue.observable创建一个可响应的对象
export const store = Vue.observable({
  userInfo: {},
  roleIds: []
})
// 定义 mutations, 修改属性
export const mutations = {
   setUserInfo(userInfo) {
     store.userInfo = userInfo
   },
   setRoleIds(roleIds) {
     store.roleIds = roleIds
   }
}
```

#### 2，在组件中引用

```vue
<template>
   <div>
     {{ userInfo.name }}
   </div>
</template>
<script>
  import { store, mutations } from '../store'
  export default {
    computed: {
      userInfo() {
        return store.userInfo 
      }
   },
   created() {
     mutations.setUserInfo({
       name: 'hello'
     })
   }
}
</script>
```

### 二、watch监听

#### 1，基础用法

比如一个列表页，我们希望用户在搜索框输入搜索关键字的时候，可以自动触发搜索,此时除了监听搜索框的`change`事件之外，我们也可以通过`watch`监听搜索关键字的变化

```vue
<template>
  <!--此处示例使用了element-ui-->
  <div>
    <div>
      <span>搜索</span>
      <input v-model="searchValue" />
    </div>
    <!--列表，代码省略-->
  </div>
</template>
<script>
export default {
  data() {
    return {
      searchValue: ''
    }
  },
  watch: {
    // 在值发生变化之后，重新加载数据
    searchValue(newValue, oldValue) {
      // 判断搜索
      if (newValue !== oldValue) {
        this.$_loadData()
      }
    }
  },
  methods: {
    $_loadData() {
      // 重新加载数据，此处需要通过函数防抖
    }
  }
}
</script>
```

#### 2，立即触发

通过上面的代码，现在已经可以在值发生变化的时候触发加载数据了，但是如果要在页面初始化时候加载数据，我们还需要在`created`或者`mounted`生命周期钩子里面再次调用`$_loadData`方法。不过，现在可以不用这样写了，通过配置`watch`的立即触发属性，就可以满足需求了

```javascript
// 改造watch
export default {
  watch: {
    // 在值发生变化之后，重新加载数据
    searchValue: {
    // 通过handler来监听属性变化, 初次调用 newValue为""空字符串， oldValue为 undefined
      handler(newValue, oldValue) {
        if (newValue !== oldValue) {
          this.$_loadData()
        }
      },
      // 配置立即执行属性
      immediate: true
    }
  }
}
```

#### 3、深度监听deep

一个表单页面，需求希望用户在修改表单的任意一项之后，表单页面就需要变更为被修改状态。如果按照上例中`watch`的写法，那么我们就需要去监听表单每一个属性，太麻烦了，这时候就需要用到`watch`的深度监听`deep`

```javascript
export default {
  data() {
    return {
      formData: {
        name: '',
        sex: '',
        age: 0,
        deptId: ''
      }
    }
  },
  watch: {
    // 在值发生变化之后，重新加载数据
    formData: {
      // 需要注意，因为对象引用的原因， newValue和oldValue的值一直相等
      handler(newValue, oldValue) {
        // 在这里标记页面编辑状态
      },
      // 通过指定deep属性为true, watch会监听对象里面每一个值的变化
      deep: true
    }
  }
}
```

#### 4，随时监听，随时取消----$watch

有这样一个需求，有一个表单，在编辑的时候需要监听表单的变化，如果发生变化则保存按钮启用，否则保存按钮禁用。这时候对于新增表单来说，可以直接通过`watch`去监听表单数据(假设是`formData`),如上例所述，但对于编辑表单来说，表单需要回填数据，这时候会修改`formData`的值，会触发`watch`,无法准确的判断是否启用保存按钮。现在你就需要了解一下`$watch`

```javascript
export default {
  data() {
    return {
      formData: {
        name: '',
        age: 0
      }
    }
  },
  created() {
    this.$_loadData()
  },
  methods: {
    // 模拟异步请求数据
    $_loadData() {
      setTimeout(() => {
        // 先赋值
        this.formData = {
          name: '子君',
          age: 18
        }
        // 等表单数据回填之后，监听数据是否发生变化
        const unwatch = this.$watch(
          'formData',
          () => {
            console.log('数据发生了变化')
          },
          {
            deep: true
          }
        )
        // 模拟数据发生了变化
        setTimeout(() => {
          this.formData.name = '张三'
        }, 1000)
      }, 1000)
    }
  }
}
```

根据上例可以看到，我们可以在需要的时候通过`this.$watch`来监听数据变化。那么如何取消监听呢，上例中`this.$watch`返回了一个值`unwatch`,是一个函数，在需要取消的时候，执行 `unwatch()`即可取消

### 三、函数式组件

什么是函数式组件？函数式组件就是函数是组件，感觉在玩文字游戏。使用过`React`的同学，应该不会对函数式组件感到陌生。函数式组件，我们可以理解为没有内部状态，没有生命周期钩子函数，没有`this`(不需要实例化的组件)。

在日常写bug的过程中，经常会开发一些纯展示性的业务组件，比如一些详情页面，列表界面等，它们有一个共同的特点是只需要将外部传入的数据进行展现，不需要有内部状态，不需要在生命周期钩子函数里面做处理，这时候你就可以考虑使用函数式组件。

#### 1，函数式组件代码

```javascript
export default {
  // 通过配置functional属性指定组件为函数式组件
  functional: true,
  // 组件接收的外部属性
  props: {
    avatar: {
      type: String
    }
  },
  /**
   * 渲染函数
   * @param {*} h
   * @param {*} context 函数式组件没有this, props, slots等都在context上面挂着
   */
  render(h, context) {
    const { props } = context
    if (props.avatar) {
      return <img src={props.avatar}></img>
    }
    return <img src="default-avatar.png"></img>
  }
}
```

在上例中，我们定义了一个头像组件，如果外部传入头像，则显示传入的头像，否则显示默认头像。上面的代码中大家看到有一个render函数，这个是`Vue`使用`JSX`的写法。

#### 2，为什么使用函数式组件

- 最主要最关键的原因是函数式组件不需要实例化，无状态，没有生命周期，所以渲染性能要好于普通组件
- 函数式组件结构比较简单，代码结构更清晰

#### 3，函数式组件与普通组件区别

1. 函数式组件需要在声明组件是指定functional
2. 函数式组件不需要实例化，所以没有`this`,`this`通过`render`函数的第二个参数来代替
3. 函数式组件没有生命周期钩子函数，不能使用计算属性，watch等等
4. 函数式组件不能通过$emit对外暴露事件，调用事件只能通过`context.listeners.click`的方式调用外部传入的事件
5. 因为函数式组件是没有实例化的，所以在外部通过`ref`去引用组件时，实际引用的是`HTMLElement`
6. 函数式组件的`props`可以不用显示声明，所以没有在`props`里面声明的属性都会被自动隐式解析为`prop`,而普通组件所有未声明的属性都被解析到`$attrs`里面，并自动挂载到组件根元素上面(可以通过`inheritAttrs`属性禁止)

#### 4，不用jsx的函数式组件

在`Vue2.5`之前，使用函数式组件只能通过`JSX`的方式，在之后，可以通过模板语法来生命函数式组件

```vue
<!--在template 上面添加 functional属性-->
<template functional>
  <img :src="props.avatar ? props.avatar : 'default-avatar.png'" />
</template>
<!--根据上一节第六条，可以省略声明props-->
```

### 四、Vue.use()

需要使用vue.use()安装的插件，要含有`install`

Vue.use()可以对Vue添加全局功能

全局功能包括：

1. 添加全局方法或者 property。如：[vue-custom-element](https://github.com/karol-f/vue-custom-element)
2. 添加全局资源：指令/过滤器/过渡等。如 [vue-touch](https://github.com/vuejs/vue-touch)
3. 通过全局混入来添加一些组件选项。如 [vue-router](https://github.com/vuejs/vue-router)
4. 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现。
5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 [vue-router](https://github.com/vuejs/vue-router)

例子：

```javascript
//总结目录：
|-components
　　|-loading
　　　　|-index.js	导出组件，并且install
　　　　|-loading.vue	定义Loading组件，这里面基本的style ,script中之前讲的methods,data方法都可以使用
```

1. 文件loading.vue 中定义一个组件

   ```vue
   <template>
       <div class="loading-box">
           Loading...
       </div>
   </template>
   ```

2. 在index.js 中 引入 loading.vue ，并导出

   ```javascript
   import LoadingComponent from './loading.vue'
   const Loading={
   　　　　install:function(Vue){ //核心部分，在我们使用Vue.use()时，自动调用的是install，而install导出的必须是的组件
   　　　　Vue.component('loading',LoadingComponent);
   　　}
   };
   export default Loading;
   ```

3. 在 main.js 中引入 loading 文件下的 index.js

   ```javascript
   // 其中‘./components/loading/index‘ 的 /index 可以不写，webpack会自动找到并加载 index 。如果是其他的名字就需要写上。
   import Loading from './components/loading/index'
   Vue.use(Loading)
   ```

4. ,在其他vue文件中引用Vue组件，写入定义好的组件标签`<Loading></Loading>`

   ```vue
   <template>
     <div id="app">
       <h1>vue-loading</h1>
       <Loading></Loading>
     </div>
   </template>
   ```

   

### 五、获取页面高度

```javascript
clientHeight = `${document.documentElement.clientHeight}`;
```

```html
<div v-bind:style="{ height: contentHeight + 'px'}">
</div>
```

###  六、vue变量的闪现问题

 在使用vue绑定页面加载数据时，因为是页面先加载，但是数据还没有加载完，所以会出现vue变量的闪现问题，例如： 

```html
<div id="app">
  <p>{{message}}</p>
</div>
```

 在加载时会见到： 

```html
 {{message}}
```

 vue里面提供了一个解决的指令，就是**v-cloak**；在vue挂载的对象上加上这个属性, 

```html
<div v-cloak>
  <p>{{message}}</p>
</div>
```

另外还要在样式表里对这个属性进行定义，把下面的代码加到你的css里：

```css
[v-cloak]{
   display:none;
}
```

### 七、VUE输入框change事件并获取值

**1.html**

```html
<input type="text" @change="specifiName($event)" />
<input type="text" @input="specifiName($event)" />
<input type="text" @keyup.enter="specifiName($event)"/>
```

 分别对应输入框的change事件、输入框的输入事件、输入框的回车事件

**2.js**

```javascript
var vm = new Vue({
    el: "#app",
    methods: {
      specifiName(e) {
        var that = this;
        var val = e.target.value;
        console.log(val);
      },
    }
});
```

也可以用**v-modal**

```html
<input id="my-input" 
       type='text' 
       v-model="searchText" 
       @input="onInputChange()"/>
```

### 八、VUE图片上传生成src

> 利用event.raw

```jade
var localURL = null;
if (window.createObjectURL != undefined) {
	// basic
	localURL = window.createObjectURL(event.raw);
} else if (window.URL != undefined) {
	// mozilla(firefox)
	localURL = window.URL.createObjectURL(event.raw);
} else if (window.webkitURL != undefined) {
	// webkit or chrome
	localURL = window.webkitURL.createObjectURL(event.raw);
}
```

### 九、阻止浏览器后退

```
npm install vue-prevent-browser-back --save
```

在需要阻止的组件单独引入，然后注入...

```vue
<template>
    <div>
    </div>
</template>
<script>
    import Home from "@/components/common/Home.vue";
    import preventBack from 'vue-prevent-browser-back';//组件内单独引入
    export default {
        mixins: [preventBack],//注入
        data() {
            return {};
        }
    };
</script>
```

### 十、vue将内容复制到剪贴板

1. 使用插件，先安装

   ```js
   npm install vue-clipboard2 --save
   ```

2. 全局注入(main.js)

   ```vue
   import VueClipboard from 'vue-clipboard2'
   ```

   ```javascript
   Vue.use(VueClipboard)
   ```

3. 使用

   ```js
   methods:{
   	copyFun() {
   		this.$copyText(this.publishUrl).then(() => {
    			//copy successed
           })
   	}
   }
   ```

   

### 十一、vue触发隐藏input file的方法、vue保存文件

1、vue触发隐藏input file的方法

> 使用`this.$refs.$el.dispatchEvent()`,$el为需要触发元素的ref

```vue
<div class="load-temp" @click="importTem">导入模板</div>
 <input type="file" @change="fileChange" ref="import" style="display: none;">
```

```js
importTem() {
	this.$refs.import.dispatchEvent(new MouseEvent('click'))
},
//选取文件
fileChange(e) {
	console.log(e)
    let file = e.currentTarget.files[0];
	let reader = new FileReader();//读取文件
	reader.readAsText(file);
	reader.onload = function(){                    
		let json = JSON.parse(this.result);                    
	};
},
```

2、vue保存文件

```
 cnpm install file-saver --save
```

```js
import FileSaver from 'file-saver';

methods: {
	exportFile() {
		let configString = JSON.stringify({
			a: 123,
			b: 'Tom'
		});
		let blob = new Blob([configString ], {type: "application/json;charset=utf-8"});
        FileSaver.saveAs(blob, "temp.json");
	}
}
```



### 十二、vue禁用滚轮默认事件

```vue
@wheel.prevent="scaleContainer"   //触发scaleContainer方法并禁止滚轮滚动页面
```

> `@wheel.prevent`为禁用滚轮默认事件--即页面滚动，`scaleContainer`为需要滚轮触发的事件处理函数。

### 十三、vue开发和打包内存溢出(JavaScript heap out of memory)

npm输入以下：

```js
export NODE_OPTIONS=--max_old_space_size=4096
```

package.json的scripts中build添加以下：

```js
"scripts": {
    "build": "node --max_old_space_size=4096 build/build.js"
}
```

### 十四、vue获取页面尺寸

```js
document.documentElement.clientWidth;
```

### 十五、vue下载excel文件

```js
this.$http({
	url: "",
    method: 'get',
    responseType:'blob',    
}).then(({data}) => {
	var blob = new Blob([data], {type: 'application/vnd.openxmlformats-		officedocument.spreadsheetml.sheet;charset=utf-8'}); //application/vnd.openxmlformats-officedocument.spreadsheetml.sheet这里表示xlsx类型
	var downloadElement = document.createElement('a');
	var href = window.URL.createObjectURL(blob); //创建下载的链接
	downloadElement.href = href;
	downloadElement.download = '***.xls'; //下载后文件名
	document.body.appendChild(downloadElement);
	downloadElement.click(); //点击下载
	document.body.removeChild(downloadElement); //下载完成移除元素
	window.URL.revokeObjectURL(href); //释放掉blob对象
})
```

