# 【Vue】关于过滤器filter的一些认识

## 简介

添加在JavaScript表达式的尾部，由管道符'|'表示。常用于文本格式化。

```JavaScript
{{ message | capitalize }}
<div v-bind:id="rawId | formatId"></div>
```

## 定义

组件内的本地过滤器：

```JavaScript
filters: {
	capitalize (v) {
	// 一通处理之后...
	return '处理后的值'
	}
}
```

全局过滤器：

```JavaScript
Vue.filter('capitalize', function (v) {
	// 同上
})
```

## 链式调用和函数参数

过滤器函数总是接收表达式的值或之前的操作链的结果 作为第一个参数，第二个参数开始接收自定义参数。

```JavaScript
// filterA接收一个参数message
// filterB接收三个参数：第一个参数为filterA的返回值，第二个参数为字符串arg1，第三个参数为表达式arg2
{{ message | filterA | filterB('arg1', arg2) }}
```

## 过滤器中的this问题

过滤器中的this为undefined。vue作者关于filter的说明：

![vue-filter](https://cdn.jsdelivr.net/gh/jackyli1991/Image-Hosting/img/vue-filter.png)

vue中的过滤器更偏向于对文本数据的转化，而不能依赖this上下文，如果需要使用到上下文this我们应该使用computed计算属性的或者一个method方法。