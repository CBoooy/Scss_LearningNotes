# ***0 说明：***

仅学习笔记，记了生产中的常用部分。完整部分看文档。
[Sass中文网](https://www.sass.hk/docs/)  
<br/>
# ***1 使用Scss***

## node环境下
### 安装
> npm i node-sass -g

<br/>

### 使用：
1. 单文件编译：
> node-sass old.css new.css

2. 文件监听模式：

> 语法：node-sass -w  原有的scss文件/文件目录 -o  生成的css文件目录

> 例子：node-sass -w ./scss -o ./css


<br/>

# ***2 CSS功能拓展***

# 2.1 允许嵌套

 ``` 
 #main p {
    color: #00ff00; 

    .redbox { 
      color: #000000;
    }
  } 

```

<br/>

# 2.2 嵌套内使用父选择器,用 & 代表嵌套规则外层的父选择器


## 写法一：编译后的 CSS 文件中 & 将被替换成嵌套外层的父选择器
```
a {
  font-weight: bold; 

    &:hover { text-decoration: underline; } 
}

编译为：

a {
  font-weight: bold;
}

a:hover {
  text-decoration: underline; 
}

```
## 写法二：& 必须作为选择器的第一个字符，其后可以跟随后缀生成复合的选择器

```
#main {
  color: black;
  &-sidebar { border: 1px solid; }
}

编译为：

#main{
  color:black;
}

#main-sidebar{
  border:1px solid ;
}
```

<br/>



# 2.3 注释/* */ 与 // 

## CSS 多行注释 /* */，以及单行注释 //，前者会 被完整输出到编译后的 CSS 文件中，而后者则不会

<br/>


# 2.4 SassScript

## 1 变量$

```
  $width:100px;

  >>>>

  #main{
    width:$width
  }
```

## 2 数据类型

1. 数字，1, 2, 13, 10px
2. 字符串，有引号字符串与无引号字符串，"foo", 'bar', baz
3. 颜色，blue, #04a3f9, rgba(255,0,0,0.5)
4. 布尔型，true, false
5. 空值，null
6. 数组 (list)，用空格或逗号作分隔符，1.5em 1em 0 2em, Helvetica, Arial, sans-serif
7. maps, 相当于 JavaScript 的 object，(key1: value1, key2: value2)

## 3 运算

### 数字运算 
```

支持(+, -, *, /, %)，如果必要会在不同单位间转换值

p {
  width: 1in + 8pt;
}

```

### 颜色值运算

```
p {
  color: #010203 + #040506;
  color: #010203 * 2;
  color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);

}
```
### 字符串运算

``` 
注意：
1 有 + 无 = 有
2 无 + 有 = 无

p:before {
  content: "Foo " + Bar;
  font-family: sans- + "serif";
}

>>>编译为

p:before {
  content: "Foo Bar";
  font-family: sans-serif; 
}
```
 

## 4 圆括号

```

圆括号可以用来影响运算的顺序：

p {
  width: 1em + (2em * 3);
}

```
## 5 函数

### 内置函数
### 自定义函数
## 6 插值语句 

```
通过 #{} 插值语句可以在选择器或属性名中使用变量：

$name: foo;
$attr: border;

p.#{$name} {
  #{$attr}-color: blue;
}

>>>编译为：

p.foo {
  border-color:blue;
}

```

<br/>


# 2.5 @ 

## 1 @import
## 2 @media

```
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}

>>>编译为

.sidebar {
  width: 300px; 
}

@media screen and (orientation: landscape) {
  .sidebar {
    width: 500px;
  } 
}
```
## 3 @extend 

```
作用：将一个选择器下的所有样式继承给另一个选择器

.one {
  color:red; 
}
.two {
  @extend .one ;
  border-width: 3px;
}

>>> 编译为：

.one {
  color:red; 
}
.two {
  color:red; 
  border-width: 3px;
}
```

<br/>


# 2.6 控制指令

## 1 @if

### 写法一：

```
p {
  @if 1 + 1 == 2 { border: 1px solid; }
  @if 5 < 3 { border: 2px dotted; }
  @if null  { border: 3px double; }
}
编译为

p {
  border: 1px solid; 
}
```
### 写法二：

```
$type: monster;
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
```
## 2 @for

```
注意：through 与 to 的含义：当使用 through 时，条件范围包含 <start> 与 <end> 的值

@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}

>>> 编译为

.item-1 {
  width: 2em; 
  }

.item-2 {
  width: 4em; 
  }

.item-3 {
  width: 6em; 
  }
``` 
## 3 @while

```
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}

>>> 编译为：

.item-6 {
  width: 12em; 
  }

.item-4 {
  width: 8em; 
  }

.item-2 {
  width: 4em; 
  }
```


<br/>

# 2.7 Mixin

```
Mixin用于定义可重复使用的样式

>>> 定义mixin：

@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}

>>> 使用 @include 指令引用mixin 
 
.page-title {
  @include large-text;
  padding: 4px;
  margin-top: 10px;
}

>>> 编译为:

.page-title {
  font-family: Arial;
  font-size: 20px;
  font-weight: bold;
  color: #ff0000;
  padding: 4px;
  margin-top: 10px; 
  }
```
 

### 扩展：mixin的参数

```
@mixin sexy-border($color, $width) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}
p { @include sexy-border(blue, 1px); }

>>> 编译为:

p {
  border-color: blue;
  border-width: 1px;
  border-style: dashed; 
  }
```
 
 

