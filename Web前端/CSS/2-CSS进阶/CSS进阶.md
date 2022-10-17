# CSS进阶

# 一. 选择器进阶

## 1.复合选择器

### 1.1 后代选择器：空格

* 作用：根据HTML标签的嵌套关系，选择父元素**后代**中满足条件的元素
* 语法：选择器1 选择器2{css}
* 结果：在选择器1所找到标签的后代中（儿、孙、重孙..），找到满足选择器2的标签，设置样式
* 注意：选择器1与2之间用空格隔开

### 1.2 子代选择器：>

* 作用：根据HTML标签的嵌套关系，选择父元素**子代**中满足条件的元素
* 语法：选择器1>选择器2{css}
* 结果：在选择器1中的子代（儿子）中找到满足条件的标签，设置样式

## 2.并集选择器

### 2.1 并集选择器：，

* 作用：同时选择多组标签，设置相同的样式
* 语法：选择器1,选择器2{css}
* 结果：找到选择器1和选择器2选中的标签，设置样式
* 注意：
  * 并集选择器的每组选择器可以是基础选择器或者复合选择器

## 3.交集选择器

### 3.1 交集选择器：紧挨着的

* 作用：选中页面中的同时满足多个选择器的标签
* 语法：选择器1选择器2{css}
* 结果：找到页面中既能被选择器1选中，又能被选择器2选中的标签，设置样式
* 注意：两个选择器之间没有分隔，紧挨着的，除非你是类选择器（.类属性名）

## 4.hover伪类选择器

### 4.1 hover伪类选择器

* 作用：选中鼠标悬停在元素上的状态，设置样式
* 语法：选择器:hover{css}
* 注意：
  * 伪类选择器选中元素的某种状态

# 二. 背景相关属性

## 1.背景颜色

### 1.1 背景颜色

* 属性名：background-color
* 取值：关键字、rgb、rgba、十六进制
* 注意：
  * 背景颜色默认透明（rgba（0,0,0,0）、transparent）
  * 颜色不影响盒子大小
  * 布局先给盒子上颜色。方便看清

## 2.背景图片

### 2.1 背景图片

* 属性名：background-image
* 取值：url('图片的路径')
* 注意：
  * 引号可省略
  * 图片默认水平垂直平铺，装满
  * 撑不开盒子，改不了大小

## 3.背景平铺

### 3.1 背景平铺

* 属性名：background-repeat
* 取值：
  * repeat（默认）：水平垂直平铺
  * **no-repeat**：不平铺
  * repeat-x：沿x轴平铺
  * repeat-y：沿y

## 4.背景位置

### 4.1 背景位置

* 属性名：background-position
* 取值：
  * 方位名词：
    * 水平方向
      * left
      * right
      * center
    * 垂直方向
      * top
      * center
      * bottom
  * 数字+px（坐标）：
    * 原点（0,0）：盒子的左上角
    * x轴：水平向右
    * y轴：垂直向下
* 注意：两者可以混用，负数向左和上延伸

## 5.背景相关属性连写

### 5.1 连写

* 复合属性： background
* 取值：color image repeat position
* 按要求省略，不分顺序，空格隔开
* position取值为单词可以调换顺序不影响，数字不可以

## 6.(拓展)img标签和背景图片的区别

* 在网页中直接展示一张图片
  * 直接img标签
  * 或者div+背景图片（但要设置div的宽高，因为图片撑不开盒子）
  * 装饰为背景图，商品展示为img

# 三. 元素显示模式

## 1. 块级元素

* 显示特点：
  * 独占一行，一行只显示一个
  * 默认宽度是父元素的宽度100%，高度默认由内容撑开
  * 可以设置宽高
  * 代表标签：div、p、h系列、ul、li、dl、dt、dd、form、header、nav、footer等

## 2. 行内元素

* 显示特点：
  * **一行可以显示多个**
  * **宽度和高度默认由内容撑开**
  * **不可以设置宽高**
  * 代表标签：a、span、b、u、i、s、strong、ins、em、del等

## 3. 行内块元素

* 显示特点：
  * 一行可以显示多个
  * 可以设置宽高
  * 代表：input、textarea、button、select、img（有行内块特点，Chrome显示inline）等

## 4. 元素显示模式的转换

* 目的：改变元素默认的显示特点，让元素符合布局要求
* 语法：
  * 属性：display：block ——转为块级元素，用的多
  * 属性：display：inline-block——转为行内块元素，用的多
  * 属性：display：inline——转为行内元素。用得少

## 5. HTML嵌套规范

* 块级元素一般作为大容器，可以嵌套：
  * 文本
  * 块级元素
  * 行内元素
  * 行内块元素等
  * p不可以套div、p、h等块级
* a标签可以嵌套任意元素
  * 但是，a不可以套a



# 四. CSS三大特征

## 1. 继承性

### 1.1 介绍

* 特性：子元素默认继承父元素样式的特点
* 可继承的常见属性（文字控制属性都可以继承）
  * color
  * font系列
  * text-indent、text-align
  * line-height
  * 等
* 注：可以**通过调试工具判断样式是否可以继承**（比如a标签不能继承父元素的宽高）
* 应用场景：给body标签加上通用属性，其内所有标签都继承上这些属性

### 1.2 继承失效情况

* 元素有浏览器默认样式，此时继承性依然存在。但是优先显示浏览器的默认样
  * a标签的color
  * h标签的字体大小

## 2. 层叠性

### 2.1 介绍

* 特性：
  * 给同一个标签**设置不同的样式**，样式会层层**叠加** => 会**共同作用**在这个标签上
  * 给同一个标签**设置相同的样式**，样式会层层**覆盖** => **最后**写在这个标签上**的样式会生效**
* 注：样式冲突时，只有当选择器优先级相同时，才能通过层叠性判断结果

## 3.优先级

### 3.1 介绍

* 特性：不同的选择器具有不同的优先级，优先级高的选择器样式会覆盖优先级低的选择器的样式
* 优先级别：
  * 继承 < 通配符选择器 < 标签选择器 < 类选择器 < id选择器 < 行内样式 < !important
* 注：
  * !important写在属性值的后面，分号的前面
  * !important不能提升继承的优先级，只要是继承优先级就最低
  * 实际开发不建议用!important

### 3.2 权重叠加计算

* 用于复合选择器，判断哪个选择器优先级最高会生效

* 公式：

  * **（0,0,0,0）=> （行内样式的个数<第一级>，id选择器的个数<第二级>，类选择器的个数<第三级>，标签选择器的个数<第四级>）**

* 比较规则：

  * 先比较第一级数字，谁大谁优先级高，比较出来了就不用看后面的了
  * 第一级相同则依次比较第二级，以此类推
  * 最终数字都相同，那么比较层叠性，后写的生效
  * !important只要不是继承的，那么他就是最高的优先级

* 注：单个选择器与复合选择器比较时，依然用权重叠加计算

  ​		都是继承时，就近原则，看上一级父级是什么属性，就继承什么

### 3.3 拓展：要学会Chrome浏览器排错

* 遇到错要打开浏览器打开开发者面板看错在哪儿

* css上一层代码出错之后的都不会生效



# 五. 综合案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS进阶-综合案例1</title>
  <style>
      a:hover{/*伪类选择器,鼠标悬停时元素会怎么样*/
          background: orange;/*悬停时背景为橙色*/
      }
    a{
      display: inline-block;/*a标签默认为行内元素,无法设置宽高,转为行内块元素,即可多个放置且能设置宽高*/
        background: red;/*背景色*/
        width: 50px;/*元素宽高*/
        height: 50px;
        text-align: center;/*元素内文字水平居中*/
        line-height: 50px;/*元素内文字垂直居中 设置高度与文字父元素高度一致即可*/
        text-decoration: none;/*去掉超链接的默认下划线*/
        color: white;/*设置元素内字体颜色*/
    }
  </style>
</head>
<body>
<a href="#">导航1</a>
<a href="#">导航2</a>
<a href="#">导航3</a>
<a href="#">导航4</a>
<a href="#">导航5</a>
</body>
</html>
```



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>CSS进阶-综合案例2</title>
    <style>
      a:hover {
        background-image: url("../image/bg1.png");
      }
      .bki2 {
        background-image: url("../image/bg2.png");
      }
      .bki3 {
        background-image: url("../image/bg3.png");
      }
      .bki4 {
        background-image: url("../image/bg4.png");
      }
      .bki5 {
        background-image: url("../image/bg5.png");
      }
      .bki6 {
        background-image: url("../image/bg6.png");
      }
      .bki7 {
        background-image: url("../image/bg7.png");
      }
      a {
        background: pink;
        color: white;
        display: inline-block;
        width: 125px;
        height: 58px;
        text-decoration: none;
        text-align: center;
        line-height: 48px;
      }
    </style>
  </head>
  <body>
    <a href="#" class="bki2">navigation2</a>
    <a href="#" class="bki3">navigation3</a>
    <a href="#" class="bki4">navigation4</a>
    <a href="#" class="bki5">navigation5</a>
    <a href="#" class="bki6">navigation6</a>
    <a href="#" class="bki7">navigation7</a>
  </body>
</html>
```



> iWyh2
