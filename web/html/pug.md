## pug介绍
参考资料<https://pugjs.org/api/getting-started.html>

pug是用来编译生成html文件的，由缩进进行控制，类似python的书写形式，具有优雅的格式。pug支持众多语法，包括变量、数组、循环等。
pug原名jade，因版权原因改名为pug。


## 命令行工具
### 安装
```bash
$ npm install pug
```

### 编译
```bash
$ pug -P  pug.pug 
```

#### 监听文件
```bash
$ pug -P --watch pug.pug
```

## 语法
### 变量

1. 普通属性
```pug
a(class='button', href='google.com') Google
```

2. 三目运算符
```pug
- var authenticated = true
body(class=authenticated ? 'authed' : 'anon')
```

3. 多个属性
```pug
input(
  type='checkbox'
  name='agreement'
  checked
)
```

4. style属性
```pug
a(style={color: 'red', background: 'green'})
```

5. class属性
```pug
- var classes = ['foo', 'bar', 'baz']
a(class=classes)
|
|
//- the class attribute may also be repeated to merge arrays
a.bang(class=classes class=['bing'])
```

6. class选择器
```pug
a.button      // <a class="button"></a>
.content      // <div class="content"></div>
```

7. id选择器
```pug
a#main-link     // <a id="main-link"></a>
#content        // <div id="content"></div>
```

8. &变量
```pug
div#foo(data-bar="foo")&attributes({'data-foo': 'bar'})  // <div id="foo" data-bar="foo" data-foo="bar"></div>
```

### case语句,类似于switch语句
```pug
- var friends = 0
case friends
  when 0
    - break
  when 1
    p you have very few friends
  default
    p you have #{friends} friends
```

### code代码
1. for循环
```pug
- for (var x = 0; x < 3; x++)
  li item
```

2. each循环
```pug
-
  var list = ["Uno", "Dos", "Tres",
          "Cuatro", "Cinco", "Seis"]
each item in list
  li= item
```

3. 转义代码
pug中的`<`,`>`等会被自动转译为`&lt;`,`&gt;`
```pug
p= 'This code is' + ' <escaped>!'     // <p>This code is &lt;escaped&gt;!</p>
// 非转义代码
p!= 'This code is' + ' <strong>not</strong> escaped!'     // <p>This code is <strong>not</strong> escaped!</p>
```

### 注释
1. 标准注释 `//` 使用双斜杠，会转译到html文件中。
2. `//-` pug私有注释，不会转译到html文件中。
3. `<` 使用< 开头使用html的注释格式

### 条件控制
```pug
if xxx
  code
else if xxx
  code
else
  code
```

```pug
unless user.isAnonymous     // -> if !user.isAnonymous
```

### 过滤器

### 导入
1. 导入pug文件，将文件合并导入
```pug
include includes/head.pug
```

2. 导入css、js文件
```pug
//- index.pug
doctype html
html
  head
    style
      include style.css
  body
    h1 My Site
    p Welcome to my super lame site.
    script
      include script.js
```

3. 导入过滤器
//- index.pug
doctype html
html
  head
    title An Article
  body
    include:markdown-it article.md

### 模板继承


### 插入元素
#### 插入转译字符串
```pug
- var title = "On Dogs: Man's Best Friend";
- var author = "enlore";
- var theGreat = "<span>escape!</span>";

h1= title
p Written with love by #{author}
p This will be safe: #{theGreat}  // -> <p>This will be safe: &lt;span&gt;escape!&lt;/span&gt;</p>
```

```pug
- var msg = "not my inside voice";
p This is #{msg.toUpperCase()}   // -> <p>This is NOT MY INSIDE VOICE</p>
```

#### 插入非转义字符串
```pug
- var riskyBusiness = "<em>Some of the girls are wearing my mother's clothing.</em>";
.quote
  p Joel: !{riskyBusiness}   // ->    <p>Joel: <em>Some of the girls are wearing my mother's clothing.</em></p>
```

#### 插入标签
```pug
p.
  This is a very long and boring paragraph that spans multiple lines.
  Suddenly there is a #[strong strongly worded phrase] that cannot be
  #[em ignored].
p.
  And here's an example of an interpolated tag with an attribute:
  #[q(lang="es") ¡Hola Mundo!]
```

-------> 编译结果

```html
<p>This is a very long and boring paragraph that spans multiple lines. Suddenly there is a <strong>strongly worded phrase</strong> that cannot be<em>ignored</em>.</p>
<p>And here's an example of an interpolated tag with an attribute:<q lang="es">¡Hola Mundo!</q></p>
```

#### 空白控制
```pug
p
  | If I don't write the paragraph with tag interpolation, tags like
  strong strong
  | and
  em em
  | might produce unexpected results.
p.
  If I do, whitespace is #[strong respected] and #[em everybody] is happy.
```

------> 编译结果
```html
<p>If I don't write the paragraph with tag interpolation, tags like<strong>strong</strong>and<em>em</em>might produce unexpected results.</p>
<p>If I do, whitespace is <strong>respected</strong> and <em>everybody</em> is happy.</p>
```

### 循环控制
#### each
```pug
ul
  each val, index in ['zero', 'one', 'two']
    li= index + ': ' + val
```

#### while
```pug
- var n = 0;
ul
  while n < 4
    li= n++
```

### mixins混合
```pug
//- Declaration
mixin list
  ul
    li foo
    li bar
    li baz
//- Use
+list
+list
```

带参数
```pug
mixin pet(name)
  li.pet= name
ul
  +pet('cat')
  +pet('dog')
  +pet('pig')
```

#### 代码块混合
```pug
mixin article(title)
  .article
    .article-wrapper
      h1= title
      if block
        block
      else
        p No content provided

+article('Hello world')

+article('Hello world')
  p This is my
  p Amazing article
```

```pug
mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class href=href)= name

+link('/foo', 'foo')(class="btn")
```

### 纯文本
#### 内联标签
```pug
p This is plain old <em>text</em> content.
```

#### 管道文本
```pug
p
  | The pipe always goes at the beginning of its own line,
  | not counting indentation.
```

#### 空白符控制
1. 
```pug
| You put the em
em pha
| sis on the wrong syl
em la
| ble.
```

-------> 编译结果
```html
You put the em<em>pha</em>sis on the wrong syl<em>la</em>ble.
```

2. 
```pug
a ...sentence ending with a link
| .
```

-----> 便以结果
```html
<a>...sentence ending with a link</a>.
```

3. 
推荐的解决方法
```pug
| Don't
|
button#self-destruct touch
|
| me!
```

-------> 编译结果
```html
Don't
<button id="self-destruct">touch</button> me!
```
