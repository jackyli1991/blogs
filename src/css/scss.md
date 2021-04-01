# 【scss】一、基本使用说明

Sass 是一款强化 CSS 的辅助工具，它在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能，这些拓展令 CSS 更加强大与优雅。

## 两种语法格式

- scss：在 CSS3 语法的基础上进行拓展并加入 Sass 的特色功能，以 `.scss` 作为拓展名
- sass：使用“缩进”代替“花括号”，用“换行”代替“分号”分隔属性，以`.sass`作为拓展名
- 任何一种格式可以直接 `导入@import`到另一种格式中使用，或者通过 `sass-convert` 命令行工具转换成另一种格式

```javascript
# Convert Sass to SCSS
$ sass-convert style.sass style.scss

# Convert SCSS to Sass
$ sass-convert style.scss style.sass
```

## css功能拓展

1. 嵌套规则（Nested Rules）：允许将一套 CSS 样式嵌套进另一套样式中，外层作为父选择器。避免了重复输入父选择器，令CSS 结构更易于管理。

```scss
#main {
  width: 97%;
  p, div {
    font-size: 2em;
    a {
      font-weight: bold;
    }
  }
}
```

2. 父选择器`&`：直接使用嵌套外层的父选择器

```scss
.main {
  font-weight: bold;
  &:hover {
    text-decoration: underline;
  }
  body.firefox & {
    font-weight: normal;
  }
  &-sidebar {
    border: 1px solid;
  }
}
```

3. 属性嵌套：Sass 允许将属性嵌套在命名空间，避免重复输入

```scss
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```

4. 特殊的占位符选择器`%`：必须通过 `@extend`指令调用

