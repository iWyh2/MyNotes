# CSS浮动



# 1.结构伪类选择器

* 作用：根据元素在HTML中的结构关系查找元素

* 好处：减少对HTML中的类的依赖，有利于保持代码整洁

* 场景：常用于查找某父级选择器中的子元素

* 选择器：

  |         选择器         |                    说明                    |
  | :--------------------: | :----------------------------------------: |
  |    E:first-child {}    |  匹配父元素中的第一个子元素，并且为E元素   |
  |    E:last-child {}     | 匹配父元素中的最后一个子元素，并且为E元素  |
  | **E:nth-child(n)** {}  |   匹配父元素中的第n个子元素，并且为E元素   |
  | E:nth-last-child(n) {} | 匹配父元素中的倒数第n个子元素，并且为E元素 |

  重点记**E:nth-child(n)**，可选任意位置，代替其余三种选择器

* n的注意点：

  * n为从零开始的（0,1,2...）
  * n可写成公式：
    * 偶数：2n、even
    * 奇数：2n+1、2n-1、odd
    * 找到前五个：-n+5
    * 从第五个开始：n+5（n必须在前面）





# 2.伪元素

* 伪元素：一般页面中的非主体内容可以使用伪元素，也就是无用的装饰性的图标之类的

* 区别：

  * 元素：HTML设置的标签
  * 伪元素：CSS模拟出来的标签效果

* 种类：（冒号一个两个都可以）

  |  伪元素  |               作用               |
  | :------: | :------------------------------: |
  | ::before | 在父元素内容的最前添加一个伪元素 |
  | ::after  | 在父元素内容的最后添加一个伪元素 |

* 格式：

  ```css
  	  .father::before {
          /* 内容 要有引号，无论单双 */
          content: "wo cao?";
          color: blue;
          width: 50px;
          height: 50px;
          background-color: black;
          /* 默认为行内元素 无法设置宽高 */
          display: inline-block;
        }
        .father::after {
          content: "666";
        }
  ```

* 注：

  * 必须设置content属性才能生效，哪怕不要内容，你也得留个引号在那儿摆着
  * 伪元素是默认的行内元素





# 3.标准流

* 又称文档流，是浏览器在渲染网页时默认使用的排版规则，固定应该以何种方式排列元素
* 常见标准流排版规则：
  * 块级元素：从上往下，垂直布局，独占一行
  * 行内元素/行内块元素：从左往右，水平布局，空间不够自动换行
* 也就是元素显示模式罢了





# 4.浮动

## 4.1 浮动的作用

* 早期作用：图文环绕
* 现在的作用：**网页布局**，也就是让块级标签完美的在一排排列



## 4.2 浮动的代码

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* img {
        float: left; 让图片被文字环绕显示，也就是图文环绕
      } */
      div {
        width: 100px;
        height: 100px;
      }
      .one {
        background-color: pink;
        float: left;
      }
      .two {
        background-color: skyblue;
        /* float: right; */
        /* 加上浮动之后让标签无缝隙的紧贴在一起 无需标签代码换行 */
        float: left;
      }
    </style>
  </head>
  <body>
    <!-- <img src="../image/window.png" alt="" />
    66666666666666666666666666666666666666666
    66666666666666666666666666666666666666666
    66666666666666666666666666666666666666666
    6666666666666666666666666666666666666 -->

    <div class="one">one</div>
    <div class="two">two</div>
  </body>
</html>
```



## 4.3 浮动的特点

* 浮动元素会脱离标准流（简称脱标），也就是在标准流中不占位置
  * 可以理解为从地面飘到了空中
* 浮动元素**比标准流高半个级别**，可以覆盖标准流中的元素，但不会覆盖文字内容
* 浮动找浮动，下一个浮动元素会在**上一个浮动元素后面**左右浮动
* 浮动元素的特殊显示效果：
  * 一行可以显示多个
  * 可以设置宽高
  * 也就是有着行内块特点，但比行内块高级一点
  * 同时，浮动元素在一行内是顶对齐的
* 注：浮动的元素不能通过text-align：center或者margin：0，auto来设置居中

* 一般设置了浮动也不会设置居中了
* 如果要两个元素左右浮动，那么最好两个都要各自浮动，不然有BUG



## 4.4 浮动的案例

* 网页布局案例

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
      <style>
        * {
          margin: 0;
          padding: 0;
        }
        .top {
          height: 40px;
          background-color: #333;
        }
        .header {
          width: 1226px;
          height: 100px;
          background-color: #ffc0cb;
          margin: 0 auto;
        }
        .content {
          width: 1226px;
          height: 460px;
          margin: 0 auto;
        }
        .one {
          width: 234px;
          height: 460px;
          background-color: #ffa500;
          float: left;
        }
        .two {
          width: 992px;
          height: 460px;
          background-color: #87ceeb;
          /* 最好都要设置浮动，只设置一个有莫名的BUG */
          float: right;
        }
      </style>
    </head>
    <body>
      <div class="top"></div>
      <div class="header"></div>
      <div class="content">
        <div class="one"></div>
        <div class="two"></div>
      </div>
    </body>
  </html>
  ```

* 小米模块布局案例

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
      <style>
        * {
          margin: 0;
          padding: 0;
        }
        .content {
          margin: 0 auto;
  
          width: 1226px;
          height: 614px;
          /* background-color: pink; */
        }
        .left {
          float: left;
          width: 234px;
          height: 614px;
          background-color: #800080;
        }
        .right {
          float: right;
          width: 978px;
          height: 614px;
          /* background-color: orange; */
        }
        .good {
          float: left;
          margin-left: 14px;
          width: 234px;
          height: 300px;
          background-color: #87ceeb;
        }
        ul {
          list-style: none;
        }
        /* 父级宽度不够也会自动换行 */
        .right li {
          float: left;
          margin-bottom: 14px;
          margin-right: 14px;
          width: 234px;
          height: 300px;
          background-color: #87ceeb;
        }
        li:nth-child(4n) {
          margin-right: 0;
        }
        /* li:nth-child(n + 4) {
          margin-bottom: 0;
        } *//*不需要清除下外边距，当这些小盒子与父级盒子边缘紧贴，父与子共享一个边缘，额外计算外边距*/
      </style>
    </head>
    <body>
      <div class="content">
        <div class="left"></div>
        <div class="right">
          <ul>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
          </ul>
        </div>
      </div>
    </body>
  </html>
  ```

* 网页导航案例

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
      <style>
        /* 首先清除所有默认内外边距 */
        * {
          margin: 0;
          padding: 0;
        }
        /* 设置主导航盒子区域 */
        .nav {
          margin: 50px;
          width: 640px;
          height: 50px;
          background-color: #ffc0cb;
        }
        /* 用li标签的前提为ul/ol ul去掉列表前默认的无需圆点样式 */
        ul {
          list-style: none;
        }
        /* 让主导航内的所有li标签靠左浮动 使之在主导航盒子内一行显示 */
        .nav li {
          float: left;
        }
        /* 给每个li包裹的a标签设置样式，会自动撑开li标签，要设置a的宽高背景色，则要转为行内块或块级元素，都可以，因为每个a都在一个li里面 */
        .nav li a {
          display: inline-block;
          width: 80px;
          height: 50px;
          /* background-color: green; */
          text-align: center;/*设置水平垂直都居中*/
          line-height: 50px;
          text-decoration: none;
          color: #fff;
        }
        a:hover {
          background-color: green;
        }
      </style>
    </head>
    <body>
      <!-- 网页的主导航，都是默认用li标签包裹一个a标签，因为浏览器解析八个连续的a标签会很慢，俗称，浏览器嫌弃这样写 -->
      <div class="nav">
        <ul>
          <li><a href="#">新闻1</a></li>
          <li><a href="#">新闻2</a></li>
          <li><a href="#">新闻3</a></li>
          <li><a href="#">新闻4</a></li>
          <li><a href="#">新闻5</a></li>
          <li><a href="#">新闻6</a></li>
          <li><a href="#">新闻7</a></li>
          <li><a href="#">新闻8</a></li>
        </ul>
      </div>
    </body>
  </html>
  ```
  
  
  
* CSS保存时的顺序：浏览器也能执行的更快
  * 先设置浮动/display/定位
  * 在设置盒子模型：margin padding border 宽高 背景色
  * 最后是文字样式
  * 可以自己写完后拖动





# 5.清除浮动

* 清除浮动的含义：清除浮动带来的影响
  * 影响：如果子元素浮动了，此时子元素不能撑开标准流的块级父元素
* 原因：子元素浮动后脱标了，也就是不占位置了
* 目的：需要父元素有高度，从而不影响其他网页元素的布局



## 5.1 清除浮动的方法1：直接设置父元素高度

* 本来给父级元素加上高度就可以解决的，但是实际场景内，我们不方便给父级元素加高度，比如京东网页布局很长，内容很多不固定，又比如新浪新闻显示新闻条数的位置不固定高度



## 5.2 清除浮动的方法2：额外标签法

* 操作：在父级元素内容的最后添加一个块级元素，再给添加的这个块级元素设置**clear：both**，也就是清除左和右浮动的影响

* 缺点：会在页面中额外添加标签，会让页面的HTML结构越来越复杂

* ```html
  /* 额外标签法 */
        .Clearfix {
          /* 清除左右的浮动 */
          clear: both;
        }
  <body>
      <div class="top">
        <div class="left"></div>
        <div class="right"></div>
        <div class="Clearfix"></div>
      </div>
      <div class="buttom"></div>
    </body>
  ```

  

## 5.3 清除浮动的方法3：单伪元素法

* 也就是方法二的变种，只不过用伪元素代替了额外标签

* 特点：一般项目中使用的都是这种，直接给标签加类即可清除浮动

* ```html
  /* 单伪元素法 */
        .clearfix::after {
          content: "";
          /* 清除左右的浮动 */
          clear: both;
          /* 伪元素默认为行内元素 */
          display: block;
          /* 让别人看不见这个伪元素 高浏览器版本没区别，只是为了兼容 */
          height: 0;
          visibility: hidden;
        }
  <body>
      <div class="top clearfix">
        <div class="left"></div>
        <div class="right"></div>
      </div>
      <div class="buttom"></div>
    </body>
  ```



## 5.4 清除浮动的方法4：双伪元素法

* 与单位元素类似，都是项目中使用的

* ```html
   /* 双伪元素法 */
        /* before是为了解决外边距塌陷问题 */
        /* 父子标签都是块级，子级加margin会产生塌陷，影响父级元素位置 */
        .clearfix::before,
        .clearfix::after {
          content: "";
          display: table;
        }
        .clearfix::after {
          /* 真正清除浮动 */
          clear: both;
        }
  <body>
      <div class="top clearfix">
        <div class="left"></div>
        <div class="right"></div>
      </div>
      <div class="buttom"></div>
    </body>
  ```



## 5.5 清除浮动的方法5：给父元素设置overflow：hidden

* 也就是直接给父元素设置overflow：hidden即可

* 方便得很

* ```html
  .top {/*给父级设置overflow*/
          margin: 0 auto;
          width: 1000px;
          /* height: 300px; */
          background-color: pink;
          /* overflow法 */
          overflow: hidden;
        }
  <body>
      <div class="top">
        <div class="left"></div>
        <div class="right"></div>
      </div>
      <div class="buttom"></div>
    </body>
  ```



> ©iWyh2