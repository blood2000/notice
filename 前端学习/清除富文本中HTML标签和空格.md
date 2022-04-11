### 清除富文本中HTML标签和空格

```javascript
//清楚富文本中的html标签和空格
function matchReg(str) {
	//清楚标签
	let reg = /<\/?.+?\/?>/g;
	let rmLable = str.replace(reg, '');
	//清楚空格和&nbsp
	return rmLable.replace(/[ ]|[&nbsp;]/g, '');
}
```

### 替换小括号中的内容
```javascript
let reg = /[(](.*?)[)]/gm;
let str = 'this is (test content)new str(lasdflj)';
let newStr = str.replace(reg,'***');
console.log(newStr);//this is ***new str***
```

