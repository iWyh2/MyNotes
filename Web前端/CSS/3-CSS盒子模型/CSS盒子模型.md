# CSS盒子模型

## 1. PxCook的基本使用

### 1.1 介绍

* 像素大厨
* 作用：能**测量尺寸，吸取颜色**，对于psd格式文件，还**能加快开发，直接生成css代码**

## 2. 盒子模型

### 2.1 盒子模型的介绍

* 概念：
  * 盒子就是标签，以盒子视角进行布局
  * 浏览器渲染时，将元素看做一个个矩形区域，所以叫盒子
* 盒子模型：
  * CSS规定：盒子由 **内容区域（content）**、**内边距区域（padding）**、**边框区域（border）**、**外边距区域（margin）**组成
  * 四层
  * 想象成包装盒（展示物品-content，物品外的填充泡沫做保护-padding，盒子的边框-border，这个包装盒与其余包装盒之间的距离-margin）

### 2.2 内容区域的宽度和高度

* width和height属性默认设置内容区域的大小
* 取值：数字+px

### 2.3 边框（border）

* 属性名：border(默认四个方向都有边框)
* 取值：是个复合属性，取值之间空格隔开，没有顺序
  * border: 10px solid(实线) red;
* 单方向设置边框：border-方位名词，取值一样
* border可以设置单个属性，但很少用

#### 2.3.1 案例-新浪导航

* 布局时，从外到内，先宽高背景色，再调位置，控制细节

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>综合案例-新浪导航</title>
      <style>
        .box {
          height: 40px;
          border-top: 3px solid #ff8500;
          border-bottom: 1px solid #edeef0;
        }
        .box a {
          text-decoration: none;
          font-size: 12px;
          width: 80px;
          height: 40px;
          /* background-color: #edeef0; */
          display: inline-block;
          color: #4c4c4c;
          text-align: center;
          line-height: 40px;
        }
        .box a:hover {
          background-color: #edeef0;
          color: #ff8500;
        }
      </style>
    </head>
    <body>
      <div class="box">
        <a href="#">新浪导航</a>
        <a href="#">新浪导航</a>
        <a href="#">新浪导航</a>
        <a href="#">新浪导航</a>
        <a href="#">新浪导航</a>
      </div>
    </body>
  </html>
  ```


### 2.4 内边距（padding）

* 属性名：padding(默认四个方向都有内边距)
* 取值：是个复合属性，取值之间空格隔开，顺序为上右下左（顺时针），数字+px
* 单方向设置内边距：padding-方位名词，取值一样
* padding设置一个值，为上下左右都是这个值
* padding设置两个值，为上下 左右 分别为这两个值
* padding设置三个值值，为上 左右 下 分别为这三个值
* padding设置四个值，为 上 右 下 左分别为这四个值

### 2.5 外边距（margin）

* margin与padding取值方式一样
* 只不过一个设置在盒子内部
* 这个设置在盒子外部而已



## 3.盒子实际大小计算

### 3.1 盒子实际大小初级计算公式

* 盒子实际大小是：
  * width = 内容的width + 边框线粗细 x 2
  * height= 内容的height+ 边框线粗细 x 2
* border会撑大盒子尺寸



### 3.2 盒子实际大小终极计算公式

* 盒子实际大小是：
  * width = 内容的width + 边框线粗细 x 2 + padding大小 x 2
  * height= 内容的height+ 边框线粗细 x 2 + padding大小 x 2
* border、padding会撑大盒子尺寸



## 4. CSS3盒子模型

* 可以自动内减，省去自己手动计算
* 操作：给盒子设置属性(**box-sizing：border-box;**)即可
* 浏览器自动计算多余大小，自动在内容中减去



## 5. 清除默认内外边距

* 浏览器会给一些标签设置默认的padding和margin，我们需要一开始就清除掉

* 然后后续自己设置padding和margin

* 比如：

  * body有margin：8px
  * p有上下margin
  * ul有上下margin和左padding
  * 等等

* 解决方法：通配选择器选择所有标签，设置内外边距为0

  ```css
  * {
      margin: 0px;
      padding: 0px;
  }
  ```

  或者把要去除的标签选择出来去除



## 6. 版心居中

让标签在浏览器居中，网页有效内容居中

* margin：0 auto;

* 网页新闻列表案例：

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
      <style>
        * {/*去除所有的内外边距*/
          margin: 0;
          padding: 0;
          box-sizing: border-box; /*内减模式*/
        }
        .news {
          width: 500px;
          height: 400px;
          /* background-color: pink; */
  
          border-top: 1px solid #ccc;
          border-left: 1px solid #ccc;
          border-right: 1px solid #ccc;
          margin: 50px auto;
          padding: 42px 30px 0;
        }
        .news > h2 {/*给新闻的标题上字体大小*/
          font-size: 30px;
          line-height: 1; /*行高为一倍，也就是字体的大小*/
          border-bottom: 1px solid #ccc;
          padding-bottom: 9px;
        }
        .news li {/*给无序列表的每个单元容器设置无原点符号 容器行高 内边距 行内成垂直居中*/
          list-style: none; /*去掉无序列表默认符号*/
          height: 50px;
          border-bottom: 1px dashed #ccc;
          /* text-align: center; */
          line-height: 50px; /*行内垂直居中*/
          /* color: #ccc; */
          padding-left: 28px;
        }
        .news a {/*设置a标签内的文本格式：下划线 颜色 字体大小*/
          text-decoration: none;
          color: #666;
          font-size: 18px;
        }
      </style>
    </head>
    <body>
      <div class="news">
        <h2>最新文章/New Articles</h2>
        <ul>
          <li><a href="#">阿里巴巴Java后端开发招聘</a></li>
          <li><a href="#">字节跳动Web前端开发招聘</a></li>
          <li><a href="#">腾讯Java后端开发招聘</a></li>
          <li><a href="#">百度Java后端开发招聘</a></li>
          <li><a href="#">华为Java后端开发招聘</a></li>
        </ul>
      </div>
    </body>
  </html>
  ```

* 实现细节：

  * 去掉所有内外边距，并开启css盒子，自动内减

    ```css
    * {
            margin: 0;
            padding: 0;
            box-sizing: border-box; /*内减模式*/
          }
    ```

  * line-height: 1; 行高为一倍，也就是字体的大小

  * list-style: none; 去掉无序列表默认原点符号

  * line-height: 50px; 行内垂直居中



## 7. 外边距折叠现象

### 7.1 合并现象

* 垂直布局的块级元素，上下的margin会合并
* 最终两者距离为两个的margin的最大值
* 解决方法：
  * 只给其中一个元素设置margin即可

### 7.2 塌陷现象

* 互相嵌套的块级元素，子元素的margin-top会作用在父元素上
* 导致父元素一起往下移动
* 解决方法：
  * 给父元素设置border-top或者padding-top（分割父子元素的margin-top）
  * 给父元素设置overflow：hidden
  * 转换为行内块元素
  * 设置浮动



## 8. 行内元素的内外边距问题

* 想要通过margin或padding改变行内元素的垂直位置，无法生效
* 也就是padding-top/bottom和margin-top/bottom不生效
* 解决方法：
  * 加上line-height: xxpx;即可。行高使得改变行间距从而改变垂直位置
  * 或者转换显示模式，比如转为inline-block/block



> 版权iWyh2