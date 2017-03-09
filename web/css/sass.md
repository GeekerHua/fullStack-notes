## Sass介绍
> 参考资料 <http://www.w3cplus.com/sassguide/index.html>

## 命令行工具使用
### 安装
```bash
$ npm install sass -g
```

### 编译
> 通常编译使用命令`$ sass -t expanded --watch --scss style.scss:style.css 
`即可。

1. 单文件编译
```bash
$ sass style.scss style.css
```

2. 单文件监听
```bash
$ sass --watch style.scss:style.css
```

3. 文件夹监听
```bash
$ sass --watch sassFileDirectory:cssFileDirectory
```

4. css文件转成sass/scss文件（在线转换工具css2sass）
```bash
$ sass-convert style.css style.sass   
$ sass-convert style.css style.scss
```



## 语法
### 导入
分别在sass中导入sass文件和css文件
```sass
// 导入css文件最终不会被编译
@import 'reset.css';
// 导入sass文件会被编译合并，可以省略后缀和开头的'_'。
@import 'a';
```

转译结果
```css
@inport 'reset.css';
// 下边是a.scss的转译内容
p{
    background: #0982c1;
}
```

### 注释
1. 标准注释`/* */`是css的注释，会被转译到css中。
2. // 双斜杠单行注释,不会被转译出来。

### 变量
变量一$开头，变量值和变量名用分号`:`隔开，值后边添加`!default表示默认值`。
#### 普通变量
```sass
$fontSize: 12px;
```

#### 默认变量
```sass
$baseLineHeight:        1.5 !default;
```

#### 特殊变量
如果变量作为属性或在某些特殊情况下等则必须要以#{$variables}形式使用。
```sass
//应用于class和属性
.border-#{$borderDirection}{
  border-#{$borderDirection}:1px solid #ccc;
}
//应用于复杂的属性值
body{
    font:#{$baseFontSize}/#{$baseLineHeight};
}
```

#### 多值变量
多值变量分为list类型和map类型，简单来说list类型有点像js中的数组，而map类型有点像js中的对象。

1. list
> list数据可通过空格，逗号或小括号分隔多个值，
    - 可用nth($var,$index)取值。关于list数据操作还有很多其他函数如
    - length($list)，
    - join($list1,$list2,[$separator])，
    - append($list,$value,[$separator])等，具体可参考sass Functions（搜索List Functions即可）

    1.1 定义
    ```sass
    //一维数据
    $px: 5px 10px 20px 30px;

    //二维数据，相当于js中的二维数组    
    $px: 5px 10px, 20px 30px;
    $px: (5px 10px) (20px 30px);
    ```

2. map
> map数据以key和value成对出现，其中value又可以是list。格式为：        
    - $map: (key1: value1, key2: value2, key3: value3);。
    - 可通过map-get($map,$key)取值。关于map数据还有很多其他函数如
    - map-merge($map1,$map2)，
    - map-keys($map)，
    - map-values($map)等，具体可参考sass Functions（搜索Map Functions即可）
    
    2.1 定义
    ```sass
    $heading: (h1: 2em, h2: 1.5em, h3: 1.2em);
    ```

#### 全局变量
目前还用不上

### 嵌套

#### 选择器嵌套
```sass
//sass style
//-------------------------------
#top_nav{
  line-height: 40px;
  text-transform: capitalize;
  background-color:#333;
  li{
    float:left;
  }
  a{
    display: block;
    padding: 0 10px;
    color: #fff;

    &:hover{
      color:#ddd;
    }
  }
}

```

#### 属性嵌套
所谓属性嵌套指的是有些属性拥有同一个开始单词，如border-width，border-color都是以border开头。拿个官网的实例看下：
```sass
//sass style
//-------------------------------
.fakeshadow {
  border: {
    style: solid;
    left: {
      width: 4px;
      color: #888;
    }
    right: {
      width: 2px;
      color: #ccc;
    }
  }
}
```

#### @at-root
sass3.3.0中新增的功能，用来跳出选择器嵌套的。默认所有的嵌套，继承所有上级选择器，但有了这个就可以跳出所有上级选择器。
```sass
//单个选择器跳出
.parent-2 {
  color:#f00;
  @at-root .child {
    width:200px;
  }
}
```
> @at-root (without: ...)和@at-root (with: ...)
默认@at-root只会跳出选择器嵌套，而不能跳出@media或@support，如果要跳出这两种，则需使用@at-root (without: media)，@at-root (without: support)

##### @at-root与&配合使用
```sass
//sass style
//-------------------------------
.child{
    @at-root .parent &{
        color:#f00;
    }
}

//css style
//-------------------------------
.parent .child {
  color: #f00;
}
```

### mixin混合
sass中使用`@mixin`声明混合，可以传递参数，参数名以`$`符号开始，多个参数以逗号分开，也可以给参数设置默认值。声明的`@mixin`通过`@include`来调用。

```sass
//sass style
//-------------------------------   
@mixin horizontal-line($border:1px dashed #ccc, $padding:10px){
    border-bottom:$border;
    padding-top:$padding;
    padding-bottom:$padding;  
}
.imgtext-h li{
    @include horizontal-line(1px solid #ccc);
}
.imgtext-h--product li{
    @include horizontal-line($padding:15px);
}
```

#### 多组值参数mixin
```sass
//sass style
//-------------------------------   
//box-shadow可以有多组值，所以在变量参数后面添加...
@mixin box-shadow($shadow...) {
  -webkit-box-shadow:$shadow;
  box-shadow:$shadow;
}
.box{
  border:1px solid #ccc;
  @include box-shadow(0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3),0 4px 4px rgba(0,0,0,.3));
}
```

#### @content
@content在sass3.2.0中引入，可以用来解决css3的@media等带来的问题。它可以使@mixin接受一整块样式，接受的样式从@content开始。

```sass
 //sass style
//-------------------------------                     
@mixin max-screen($res){
  @media only screen and ( max-width: $res )
  {
    @content;
  }
}

@include max-screen(480px) {
  body { color: red }
}

//css style
//-------------------------------
@media only screen and (max-width: 480px) {
  body { color: red }
}                     
```

### 继承
sass中，选择器继承可以让选择器继承另一个选择器的所有样式，并联合声明。使用选择器的继承，要使用关键词@extend，后面紧跟需要继承的选择器。

```sass
//sass style
//-------------------------------
h1{
  border: 4px solid #ff9aa9;
}
.speaker{
  @extend h1;
  border-width: 2px;
}
```

#### 占位选择器%
从sass 3.2.0以后就可以定义占位选择器%。这种选择器的优势在于：如果不调用则不会有任何多余的css文件，避免了以前在一些基础的文件中预定义了很多基础的样式，然后实际应用中不管是否使用了@extend去继承相应的样式，都会解析出来所有的样式。占位选择器以%标识定义，通过@extend调用。

```sass
//sass style
//-------------------------------
%ir{
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
%clearfix{
  @if $lte7 {
    *zoom: 1;
  }
  &:before,
  &:after {
    content: "";
    display: table;
    font: 0/0 a;
  }
  &:after {
    clear: both;
  }
}
#header{
  h1{
    @extend %ir;
    width:300px;
  }
}
.ir{
  @extend %ir;
}

//css style
//-------------------------------
#header h1,
.ir{
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
#header h1{
  width:300px;
}
```

### 函数

```sass
//sass style
//-------------------------------                     
$baseFontSize:      10px !default;
$gray:              #ccc !defualt;        

// pixels to rems 
@function pxToRem($px) {
  @return $px / $baseFontSize * 1rem;
}

body{
  font-size:$baseFontSize;
  color:lighten($gray,10%);
}
.test{
  font-size:pxToRem(16px);
  color:darken($gray,10%);
}
```

### 运算
sass具有运算的特性，可以对数值型的Value(如：数字、颜色、变量等)进行加减乘除四则运算。请注意运算符前后请留一个空格，不然会出错。

### 条件判断及循环
#### @if判断

```sass
//sass style
//-------------------------------
$lte7: true;
$type: monster;
.ib{
    display:inline-block;
    @if $lte7 {
        *display:inline;
        *zoom:1;
    }
}
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

#### 三目判断
语法为：if($condition, $if_true, $if_false) 。三个参数分别表示：条件，条件为真的值，条件为假的值。
```sass
if(true, 1px, 2px) => 1px
if(false, 1px, 2px) => 2px
```

#### @for循环
> for循环有两种形式，分别为：

```sass
@for $var from <start> through <end>
@for $var from <start> to <end>
```

$i表示变量，start表示起始值，end表示结束值，这两个的区别是关键字through表示包括end这个数，而to则不包括end这个数
```sass
//sass style
//-------------------------------
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
```

#### @each循环
语法为：
```sass
@each $var in <list or map>
```

其中$var表示变量，而list和map表示list类型数据和map类型数据。sass 3.3.0新加入了多字段循环和map数据循环。

```sass
// 单个字段list数据循环
//sass style
//-------------------------------
$animal-list: puma, sea-slug, egret, salamander;
@each $animal in $animal-list {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}

// 多个字段list数据循环
//sass style
//-------------------------------
$animal-data: (puma, black, default),(sea-slug, blue, pointer),(egret, white, move);
@each $animal, $color, $cursor in $animal-data {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
    border: 2px solid $color;
    cursor: $cursor;
  }
}

// 多个字段map数据循环
//sass style
//-------------------------------
$headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
@each $header, $size in $headings {
  #{$header} {
    font-size: $size;
  }
}
```

## 调试