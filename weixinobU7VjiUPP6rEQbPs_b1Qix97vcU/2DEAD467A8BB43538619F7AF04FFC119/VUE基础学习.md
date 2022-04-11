## VUE基础学习

[TOC]

### 一、数据绑定

#### 1，渲染一次数据

有时候只需渲染一次数据，后续数据变化不再关心，可以通过“*”实现，代码示例如下：

```vue
<span>Text: {{*text}} </span>
```

#### 2，渲染html片段

如果值是HTML片段，则可以使用***三个大括号***来绑定，代码示例如下：

```vue
<div>Logo: {{{logo}}}</div>
logo : '<span>DDFE</span>'
```

#### 3，管道过滤

```vue
{{example | toUpperCase}}
```

这里toUpperCase就是过滤器，其本质是一个JS函数，返回字符串的全大写形式。Vue.js允许过滤器串联，代码示例如下：

```vue
{{example | filterA | filterB}}
```

过滤器还支持传入参数，代码示例如下：

```vue
{{example | filter a b}}
```