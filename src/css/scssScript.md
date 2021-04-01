# 【Sass】二、scss编程功能

## 变量

使用`$`开头定义变量，支持块级作用域；可使用`!global`声明将局部变量转换为全局变量

```scss
$fontSize: 12px; // 全局变量

#main {
  $color: 16px; // 局部变量
 	$width: 10px !global; // 局部变量转换为全局变量
  font-size: $fontSize; // 使用
}
```

## 数据类型

- 数字：`1,10px`
- 字符串：`bold`，分为有引号字符串[例如`'http://www.imgUrl.com'`]和无引号字符串[例如`bold`]
  - 在编译时不会改变其类型
  - 一种类型除外：使用 `#{}`时，有引号字符串将被编译为无引号字符串，这样便于在 mixin 中引用选择器名
- 颜色：`blue, #04a3f9, rgba(255,0,0,0.5)`
- 布尔型：`true,false`
- 空值：`null`
- 数组：用空格或逗号作分隔符，`1.5em 1em 0 2em`
- maps：相当于 JavaScript 的 object，`(key1: value1, key2: value2)`

## 运算

- 相等运算：所有数据类型均支持相等运算 `==` 或 `!=`

- 数学运算：加、减、乘、除、取整等运算 `+, -, *, /, %`，必要时会在不同单位间转换值

   在css中，`/`通常用于分隔数字。除此之外在scssScript中，以下三种情况将作为除法运算符号使用：

    - 如果值或值的一部分，是变量或者函数的返回值
    - 如果值被圆括号包裹
    - 如果值是算数表达式的一部分

```scss
p {
  font: 10px/8px; // css分隔符
  $width: 1000px;
  // 以下情况，'/'作为除法运算符使用
  width: $width/2; // 变量
  width: round(1.5)/2; // 函数返回值
  height: (500px/2); // 值被圆括号包裹
  margin-left: 5px + 8px/2px; // 值是算数表达式的一部分
}
```

​		如果需要使用变量，同时又要确保 `/` 不做除法运算而是完整地编译到 CSS 文件中，只需要用 `#{}` 插值语句将变量包裹：

```scss
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height}; // /将会保留
}
```

- 颜色运算：
  - 分段计算
  - 如果颜色值包含 alpha channel（rgba 或 hsla 两种颜色值），必须拥有相等的 alpha 值才能进行运算。

- 字符串运算：
  - 使用`+`连接字符串
  - 如果有引号字符串（位于 `+` 左侧）连接无引号字符串，运算结果是有引号的；相反，无引号字符串（位于 `+` 左侧）连接有引号字符串，运算结果则没有引号

```scss
p::after {
  content: "Foo " + Bar; // => "Foo Bar"
  font-family: sans- + "serif"; // => sans-serif
}
```

- 布尔运算：支持布尔型的 `and` `or` 以及 `not` 运算
- 数组运算：数组不支持任何运算方式，只能使用 `list functions` 控制
- 圆括号可以影响运算顺序

```scss
p {
  width: 1em + (2em * 3); // 7em
}
```

- 插值语句`#{}`：在选择器、属性名和属性值中使用变量；在属性值中使用`#{}`可以避免Sass进行运算，而是直接编译 CSS

```scss
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}

// 编译为
p.foo {
  border-color: blue;
}
```


