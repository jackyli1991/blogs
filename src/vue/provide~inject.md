# 【Vue】关于provide/inject的用法

## 简介

- 祖先组件向所有子孙组件提供依赖provide，子孙组件通过inject注入。通常用于高阶插件/组件库的开发。
- `provide和inject绑定并不是响应式的！！！`：可通过传入一个可监听的对象，其对象的 property 还是可响应的。

## 属性说明

- provide：`Object | () => Object`
```javascript
{
	provide () {
		return {
			foo: 'var1'
		}
	}
}
```

- inject：`Array<string> | { [key: string]: string | Symbol | Object }`

```javascript
{
  inject: ['foo'], // 通过数组注入
}

// Vue 2.2.1+版本时，可在props和data初始化时获取到
{
  inject: ['foo'],
  data () {
    return {
      foo1: this.foo
    }
  },
  props: {
    foo2: {
      default () {
        return this.foo
      }
    }
  }
}

// Vue 2.5.0+版本时，可设置默认值和改变属性名
{
  inject: {
    myFoo: {
      from: 'foo',
      default: () => [1, 2, 3]
    }
  }
}
```

## 怎样实现依赖/注入的响应式更新

父组件通过注入一个方法，该方法返回对应的数据；子孙组件通过调用该注入的方法获取响应式数据。

```javascript
// 父组件provide一个方法
{
  data () {
    return {
      list: [] // 异步更新
    }
  },
  provide () {
    return {
      getList: () => this.list
    }
  }
}

// 子孙组件调用方法获取响应式数据：父组件list更新，子孙组件也会收到更新
{
  inject: ['getList'],
  computed: {
    list () {
      return this.getList()
    }
  }
}
```
