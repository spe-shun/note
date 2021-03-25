# css 规范

## 基本规范

* [google](https://google.github.io/styleguide/htmlcssguide.html#CSS)
* [drupal](https://www.drupal.org/docs/develop/standards/css/css-coding-standards)
* [CKAN](https://docs.ckan.org/en/ckan-2.7.3/contributing/css.html)
* [ali AlloyTeam](https://github.com/AlloyTeam/CodeGuide)
* [bootcss](https://codeguide.bootcss.com/)
* [wordPress](https://make.wordpress.org/core/handbook/best-practices/coding-standards/css/)

### google

#### 编写规则

> 使用有意义的或通用的命名

```css
/* Not recommended */
#a-1 {
}
.b-2 {
}
/* Recommended */
#nav {
}
.button {
}
```

> 使用尽可能短但必要的命名，但是不要产生误解

```css
/* Not recommended */
#navigation {
}
.atr {
}
/* Recommended */
#nav {
}
.author {
}
```

> 避免使用类型选择器来限定

```css
/* Not recommended */
div {
}
img {
}
```

> 当有缩写属性时，尽量使用缩写

```css
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
/* 改为以下写法 */
font: 100%/1.6 palatino, georgia, serif;

padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;
/* 改为以下写法 */
padding: 0 1em 2em;

flex-grow: 0;
flex-shrink: 1;
flex-basis: auto;
/* 改为以下写法 */
flex: none | [ < 'flex-grow' > < 'flex-shrink' >? || < 'flex-basis' > ];

color: #eebbcc;
/* 改为以下写法 */
color: #ebc;
```

> -1 到 1 不加 0，0 不加单位

```css
font-size: 0.8em;
padding: 0;
```

> 使用连字符分割

```css
/* Not recommended: does not separate the words “demo” and “image” */
.demoimage {
}

/* Not recommended: uses underscore instead of hyphen */
.error_status {
}
/* Recommended */
#video-id {
}
.ads-sample {
}
```

#### 格式规则

* 按字母顺序排列
* 缩进 2-4 字符
* 每次声明后都使用分号
* 在属性名称的冒号后面使用空格
* 在最后一个选择器和声明块之间使用空格
* 用新行将选择器和声明分开
* 使用单括号代替双括号

```css
h1,
h2,
h3 {
    background: fuchsia;
    border: 1px solid;
    -moz-border-radius: 4px;
    -webkit-border-radius: 4px;
    border-radius: 4px;
    color: black;
    font-family: 'open sans', arial, sans-serif;
    text-align: center;
    text-indent: 2em;
}
```

## 工具

* [stylelint](https://stylelint.io/)

## CSS 规范

[https://www.leemunroe.com/css-sass-scss-bem-less/](https://www.leemunroe.com/css-sass-scss-bem-less/)

* [ACSS](https://acss.io/)
* [BEM](http://getbem.com/naming/)
  * [https://en.bem.info/methodology/](https://en.bem.info/methodology/)
  * [https://km.sankuai.com/page/28150473](https://km.sankuai.com/page/28150473)
* [OOCSS](https://github.com/stubbornella/oocss/wiki)
* [SMACSS](http://smacss.com/)

### BEM

Bem 是块（block）、元素（element）、修饰符（modifier）的简写，由 Yandex 团队提出的一种前端 CSS 命名方法论。

**BLOCK** 可以挪窝，可以复用

![bem](../.gitbook/assets/bem.png)

**ELEMENT** BLOCK 的一部分

![ele](../.gitbook/assets/element.png)

**MODIFIER** 修饰及状态，修饰符本质上类似于 HTML 属性

#### 命名规则

* 全小写
* 用 `-` 分隔单词
* block 相当于命名空间
* element 和 block 用 `__` 连接
* modifier 和 element 用 `_` 连接
* 可存在多个 modifier
* For boolean modifiers, the value is not included in the name.

#### 实例分析

```markup
<ul class="menu">
    <li class="menu__item">...</li>
    <li class="menu__item menu__item_state_current">...</li>
    <li class="menu__item menu__item_state_disabled menu__item_deleted">...</li>
</ul>
```

```css
.menu {
}
.menu__item {
}
.menu__item_state_current {
}
.menu__item_state_disabled {
}
.menu__item_deleted {
}
```

优势：结构清晰 看 css 就知道是什么用的 劣势：名字很长

### ACSS

ACSS 的原则是把 CSS 样式打散成尽可能小的部分，每个 CSS 类只对应一条样式规则，从而达到最大化的可复用性。

#### 实例分析

[https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.css](https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.css)

```css
.m-10 {
    margin: 10px;
}
.w-50 {
    width: 50%;
}

.col-lg-4 {
    flex: 0 0 auto;
    width: 33.333333%;
}
.col-lg-5 {
    flex: 0 0 auto;
    width: 41.666667%;
}
.col-lg-6 {
    flex: 0 0 auto;
    width: 50%;
}
.col-lg-7 {
    flex: 0 0 auto;
    width: 58.333333%;
}
.col-lg-8 {
    flex: 0 0 auto;
    width: 66.666667%;
}
```

优势：代码复用度高 劣势：代码冗余，不适合个人开发，适合组件库

### OOCSS

抽象 HTML，CSS 以及 JavaScript 的独立代码段。然后可以在整个站点中重复使用该对象

1. 独立的结构和样式
2. 独立的容器和内容

```markup
<div class="item">
    <ul class="item-list">
        <li class="item-list--item">
            <h3 class="item-heading">...</h3>
        </li>
    </ul>
</div>
```

### SMACSS

**Base** 只有标签选择器 着眼于规范全站风格

```css
body,
form {
    margin: 0;
    padding: 0;
}
```

**Layout** 使用 id 选择器来定义页面的基本样式

```css
#header {
}
#sidebar {
}
#main {
}
```

**Module**

```css
.pod {
    width: 100%;
}
.pod input[type='text'] {
    width: 50%;
}
#sidebar .pod input[type='text'] {
    width: 100%;
}

.pod-callout {
    width: 200px;
}
#sidebar .pod-callout input[type='text'],
.pod-callout input[type='text'] {
    width: 180px;
}
```

**State**

```css
.is-error {
    background-color: purple;
    color: white;
}

.is-tab-active {
    background-color: white;
    color: black;
}
```

**Theme**

```css
// in module-name.css
.mod {
    border: 1px solid;
}

// in theme.css
.mod {
    border-color: blue;
}
```

## React CSS 模式

### inline

```jsx
<div style={{ fontSize: 24 }}>
    <p>Get started with inline style</p>
</div>
```

### element class

```jsx
<div style={{ fontSize: 24 }}>
    <p>Get started with inline style</p>
</div>
```

### webpack modules

```javascript
// webpack.config.js
{
  test: /\.module\.(less)$/,
  use: [
    'style-loader',
    'less-loader'
  ]
},
```

```jsx
import React from 'react'
import styles from './index.less'

const DashedBox = () => (
    <div className={styles.container}>
        <p className={styles.content}>Get started with CSS Modules style</p>
    </div>
)

export default DashedBox
```

### CSS-in-JS

```jsx
import React from 'react'
import styled from 'styled-components'

const Div = styled.div`
    margin: 40px;
    border: 5px outset pink;
    &:hover {
        background-color: yellow;
    }
`

const Paragraph = styled.p`
    font-size: 15px;
    text-align: center;
`

const OutsetBox = () => (
    <Div>
        <Paragraph>Get started with styled-components 💅</Paragraph>
    </Div>
)

export default OutsetBox
```

优势：真组件化，jsx、tsx 文件无外部依赖 劣势：项目大的时候不利于管理，编写时没有 auto-complete

