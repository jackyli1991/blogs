# 【animate.css】用法

## 安装

```javascript
// npm
npm install animate.css --save

// yarn
yarn install animate.css
```

```html
// cdn
<head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"
  />
</head>
```

## 基本使用

1. 添加基础类名`animate__animated`
2. 添加动画类型的名称，需要加上前缀`animate__`

```html
<h1 class="animate__animated animate__bounce">An animated element</h1>
```

3. 直接使用动画名，而不必为html元素添加类名

```css
.self-css-name {
  animation: bounce; /* 直接引用animate.css的动画名 */
  animation-duration: 2s; /* 不要忘记设置动画时长 */
}
```

4. animate.css使用css变量定义动画的`duration` 、`delay`、`iteractions`等，可以自定义改变全局或者指定的动画信息：

```css
/* 改变指定动画bounce的时长 */
.animate__animated.animate__bounce {
  --animate-duration: 2s;
}
```

```css
/* 改变全局所有动画 */
:root {
  --animate-duration: 800ms;
  --animate-delay: 0.9s;
}
```

5. 和JS一起使用

```javascript
// 为元素动态添加类名
element.classList.add('animate__animated', 'animate__bounceOutLeft')

// 设置元素的动画时长
element.style.setProperty('--animate-duration', '0.5s')
```
官网示例：利用JS自动添加和移除动画：

```javascript
const animateCSS = (element, animation, prefix = 'animate__') => {
  // We create a Promise and return it
  new Promise((resolve, reject) => {
    const animationName = `${prefix}${animation}`;
    const node = document.querySelector(element);

    node.classList.add(`${prefix}animated`, animationName);

    // When the animation ends, we clean the classes and resolve the Promise
    function handleAnimationEnd(event) {
      event.stopPropagation();
      node.classList.remove(`${prefix}animated`, animationName);
      resolve('Animation ended');
    }

    // 监听动画结束
  	node.addEventListener('animationend', handleAnimationEnd, {once: true});
	});
}
```

## 内置其他类名

内置了一些css类名，使用更便捷。

1. delay相关，例如：`animate__delay-2s`
2. duration相关：Slow, slower, fast, and Faster，例如：`animate__fast`
3. iteration-count相关：例如：`animate__repeat-3`

## 注意的地方

- 不能对行内元素使用动画，请先设置`display: inline-block`
- 大多数动画会超出屏幕视图区域，可能式父元素产生滚动条，请为父元素设置`overflow: hidden`
- 单靠css实现不了动画重复播放之间的间隔时间，需要借助JavaScript的编程能力
- 明确动画类型的具体使用场景，避免滥用
- 避免在大的html元素上使用
- 避免在跟元素上使用
- 避免无限循环动画