# 【Vue】关于watch监听的一些处理方式


##  deep：深度监听对象内部值的变化

```javascript
watch: {
	obj: {
		handler () {
			// obj发生改变时执行
		},
		deep: true
	}
}
```

## immediate：立即执行watch监听

```javascript
watch: {
	obj: {
		handler () {
			// 立即以obj当前值执行
		},
		immediate: true
	}
}
```

## 监听对象的某个字段

```javascript
watch: {
	'obj.key.key1': {
		handler () {
			// obj.key.key1发生变化时执行
		}
	}
}
```

## 监听路由变化

```javascript
watch: {
	'$route': {
		handler () {
			// $route发生变化时执行
		}
	}
}
```

## 监听一个对象时，watch打印出来的newVal和oldVal为何一样

这与深度无关。由于对象是引用类型，newVal和oldVal都是指向同一个对象的引用，因此实际打印出来的这两个值都是一样。

解决方式：

1. 替换为一个新的对象，而不是修改对象的某个属性
```javascript
// 对象整体替换
obj = Object.assign({}, obj, { ... })

watch: {
	obj (newVal, oldVal) {
		// 此时newVal将指向一个新的对象
	}
}
```

2. 利用计算属性、深拷贝来生成新的对象
```javascript
computed: {
	objCopy () {
		return JSON.parse(JSON.stringify(obj))
	}
},
watch: {
	// 通过watch objCopy来代替obj
	objCopy (newVal, oldVal) {
		// newVal，oldVal指向不同的obj拷贝
	}
}
```