HTML编码规范 - 小酷内部
=======================

本文主要参考了 [《百度EFE：HTML编码规范》][1] 与 [《Google HTML/CSS Style Guide》][2]。

代码风格
--------

### 缩进与换行

使用4个空格作为一个缩进层级，不允许使用2个空格或tab字符。

```
<ul>
    <li>first</li>
    <li>second</li>
</ul>
```

  - 每行不得操作80个字符，过长的代码不容易阅读与维护。
  - 较短的标签可考虑放一行，这样会比较紧凑。
  - 换行优先顺序：标签、属性，行内可不折行（会出现空格）。
  - 属性换行加两个缩进。

```
<!--***************************80个字符有这么长******************************-->
<div class="class-one class-two class-three class-four"
        data-url="http://example.com/">
    <h1>
        这是一个很长的标题。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
    </h1>
</div>  
```

### 命名

  - class 必须单词全字母小写，单词间以 - 分隔。
  - class 必须代表相应模块或部件的内容或功能，不得以样式信息进行命名。
  
```
<!-- good -->
<div class="sidebar"></div>

<!-- bad -->
<div class="left"></div>
```

  - 元素 id 必须保证页面唯一。
  - id 建议单词全字母小写，单词间以 - 分隔。同项目必须保持风格一致。
  - id、class 命名，在避免冲突并描述清楚的前提下尽可能短。
  
> 同一个页面中，不同的元素包含相同的 id，不符合 id 的属性含义。并且使用 document.getElementById 时可能导致难以追查的问题。

```
<!-- good -->
<div id="nav"></div>
<!-- bad -->
<div id="navigation"></div>

<!-- good -->
<p class="comment"></p>
<!-- bad -->
<p class="com"></p>

<!-- good -->
<span class="author"></span>
<!-- bad -->
<span class="red"></span>
```

禁止为了 hook 脚本，创建无样式信息的 class。

> 不允许 class 只用于让 JavaScript 选择某些元素，class 应该具有明确的语义和样式。否则容易导致 CSS class 泛滥。

使用 id、属性选择作为 hook 是更好的方式。

> IE 浏览器会混淆元素的 id 和 name 属性， document.getElementById 可能获得不期望的元素。所以在对元素的 id 与 name 属性的命名需要非常小心。一个比较好的实践是，为 id 和 name 使用不同的命名法。

```
<input name="foo">
<div id="foo"></div>
<script>
// IE6 将显示 INPUT
alert(document.getElementById('foo').tagName);
</script>
```

### 标签

标签名必须使用小写字母。

```
<!-- good -->
<p>Hello StyleGuide!</p>

<!-- bad -->
<P>Hello StyleGuide!</P>
```

对于无需自闭合的标签，不允许自闭合。常见无需自闭合标签有 input、br、img、hr 等。

```
<!-- good -->
<input type="text" name="title">

<!-- bad -->
<input type="text" name="title" />
```

对 HTML5 中规定允许省略的闭合标签，不允许省略闭合标签。对代码体积要求非常严苛的场景，可以例外。比如：第三方页面使用的投放系统。

```
<!-- good -->
<ul>
    <li>first</li>
    <li>second</li>
</ul>

<!-- bad -->
<ul>
    <li>first
    <li>second
</ul>
```

标签使用必须符合标签嵌套规则。

> 比如 div 不得置于 p 中，tbody 必须置于 table 中。详细的标签嵌套规则参见[HTML DTD][3]中的 Elements 定义部分。

HTML 标签的使用应该遵循标签的语义。下面是常见标签语义：

  - p - 段落
  - h1,h2,h3,h4,h5,h6 - 层级标题
  - strong,em - 强调
  - ins - 插入
  - del - 删除
  - abbr - 缩写
  - code - 代码标识
  - cite - 引述来源作品的标题
  - q - 引用
  - blockquote - 一段或长篇引用
  - ul - 无序列表
  - ol - 有序列表
  - dl,dt,dd - 定义列表

```
<!-- good -->
<p>Esprima serves as an important <strong>building block</strong> for some JavaScript language tools.</p>

<!-- bad -->
<div>Esprima serves as an important <span class="strong">building block</span> for some JavaScript language tools.</div>
```

在 CSS 可以实现相同需求的情况下不得使用表格进行布局。

> 在兼容性允许的情况下应尽量保持语义正确性。对网格对齐和拉伸性有严格要求的场景允许例外，如多列复杂表单。

标签的使用应尽量简洁，减少不必要的标签。

```
<!-- good -->
<img class="avatar" src="image.png">

<!-- bad -->
<span class="avatar">
    <img src="image.png">
</span>
```

### 属性

属性名必须使用小写字母。

```
<!-- good -->
<table cellspacing="0">...</table>

<!-- bad -->
<table cellSpacing="0">...</table>
```

属性值必须用双引号包围。不允许使用单引号，不允许不使用引号。

```
<!-- good -->
<script src="esl.js"></script>

<!-- bad -->
<script src='esl.js'></script>
<script src=esl.js></script>
```

布尔类型的属性，建议不添加属性值。

```
<input type="text" disabled>
<input type="checkbox" value="1" checked>
```

自定义属性建议以 xxx- 为前缀，推荐使用 data-。使用前缀有助于区分自定义属性和标准定义的属性。

```
<ol data-ui-type="Select"></ol>
```

通用
----

### DOCTYPE

使用 HTML5 的 doctype 来启用标准模式，建议使用大写的 DOCTYPE。

```
<!DOCTYPE html>
```

启用 IE Edge 模式。

```
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

在 html 标签上设置正确的 lang 属性。

> 有助于提高页面的可访问性，如：让语音合成工具确定其所应该采用的发音，令翻译工具确定其翻译语言等。

```
<html lang="zh-CN">
```

### 编码

页面必须使用精简形式，明确指定字符编码。指定字符编码的 meta 必须是 head 的第一个直接子元素。

```
<html>
    <head>
        <meta charset="UTF-8">
        ......
    </head>
    <body>
        ......
    </body>
</html>
```

HTML 文件使用无 BOM 的 UTF-8 编码。

> UTF-8 编码具有更广泛的适应性。BOM 在使用程序或工具处理文件时可能造成不必要的干扰。

### CSS 和 JavaScript 引入

引入 CSS 时必须指明 rel="stylesheet"。

```
<link rel="stylesheet" href="page.css">
```

引入 CSS 和 JavaScript 时无须指明 type 属性。text/css 和 text/javascript 是 type 的默认值。

展现定义放置于外部 CSS 中，行为定义放置于外部 JavaScript 中。结构-样式-行为的代码分离，对于提高代码的可阅读性和维护性都有好处。

在 head 中引入页面需要的所有 CSS 资源。在页面渲染的过程中，新的CSS可能导致元素的样式重新计算和绘制，页面闪烁。

JavaScript 应当放在页面末尾，或采用异步加载。

> 将 script 放在页面中间将阻断页面的渲染。出于性能方面的考虑，如非必要，请遵守此条建议。

```
<body>
    <!-- a lot of elements -->
    <script src="init-behavior.js"></script>
</body>
```

移动环境或只针对现代浏览器设计的 Web 应用，如果引用外部资源的 URL 协议部分与页面相同，建议省略协议前缀。

> 使用 protocol-relative URL 引入 CSS，在 IE7/8 下，会发两次请求。是否使用 protocol-relative URL 应充分考虑页面针对的环境。

```
<script src="//s1.bdstatic.com/cache/static/jquery-1.10.2.min_f2fb5194.js"></script>
```

head
----

### title

页面必须包含 title 标签声明标题。title 必须作为 head 的直接子元素，并紧随 charset 声明之后。

> title 中如果包含 ASCII 之外的字符，浏览器需要知道字符编码类型才能进行解码，否则可能导致乱码。

```
<head>
    <meta charset="UTF-8">
    <title>页面标题</title>
</head>
```

### favicon

保证 favicon 可访问。

> 在未指定 favicon 时，大多数浏览器会请求 Web Server 根目录下的 favicon.ico 。为了保证 favicon 可访问，避免 404，必须遵循以下两种方法之一：

   - 在 Web Server 根目录放置 favicon.ico 文件。
   - 使用 link 指定 favicon。

```
<link rel="shortcut icon" href="path/to/favicon.ico">
```

### viewport

若页面欲对移动设备友好，需指定页面的 viewport。

> viewport meta tag 可以设置可视区域的宽度和初始缩放大小，避免在移动设备上出现页面展示不正常。
> 
> 比如，在页面宽度小于 980px 时，若需 iOS 设备友好，应当设置 viewport 的 width 值来适应你的页面宽度。同时因为不同移动设备分辨率不同，在设置时，应当使用 device-width 和 device-height 变量。
> 
> 另外，为了使 viewport 正常工作，在页面内容样式布局设计上也要做相应调整，如避免绝对定位等。关于 viewport 的更多介绍，可以参见 [Safari Web Content Guide][4]的介绍。

图片
----

禁止 img 的 src 取值为空。延迟加载的图片也要增加默认的 src。

> src 取值为空，会导致部分浏览器重新加载一次当前页面，参考：[https://developer.yahoo.com/performance/rules.html#emptysrc][5]

避免为 img 添加不必要的 title 属性。

> 多余的 title 影响看图体验，并且增加了页面尺寸。

为重要图片添加 alt 属性，可以提高图片加载失败时的用户体验。

添加 width 和 height 属性，以避免页面抖动。

有下载需求的图片采用 img 标签实现，无下载需求的图片采用 CSS 背景图实现。

  1. 产品 logo、用户头像、用户产生的图片等有潜在下载需求的图片，以 img 形式实现，能方便用户下载。
  2. 无下载需求的图片，比如：icon、背景、代码使用的图片等，尽可能采用 CSS 背景图实现。

表单
----

### 控件标题

有文本标题的控件必须使用 label 标签将其与其标题相关联。有两种方式：

  1. 将控件置于 label 内。
  2. label 的 for 属性指向控件的 id。

推荐使用第一种，减少不必要的 id。如果 DOM 结构不允许直接嵌套，则应使用第二种。

```
<label><input type="checkbox" name="confirm" value="on"> 我已确认上述条款</label>
<label for="username">用户名：</label> <input type="text" name="username" id="username">
```

### 按钮

使用 button 元素时必须指明 type 属性值。

> button 元素的默认 type 为 submit，如果被置于 form 元素中，点击后将导致表单提交。为显示区分其作用方便理解，必须给出 type 属性。

```
<button type="submit">提交</button>
<button type="button">取消</button>
```

尽量不要使用按钮类元素的 name 属性。

> 由于浏览器兼容性问题，使用按钮的 name 属性会带来许多难以发现的问题。具体情况可参考[此文][6]。

### 可访问性 (A11Y)

负责主要功能的按钮在 DOM 中的顺序应靠前。

> 负责主要功能的按钮应相对靠前，以提高可访问性。如果在 CSS 中指定了 float: right 则可能导致视觉上主按钮在前，而 DOM 中主按钮靠后的情况。

```
<!-- good -->
<style>
.buttons .button-group {
    float: right;
}
</style>

<div class="buttons">
    <div class="button-group">
        <button type="submit">提交</button>
        <button type="button">取消</button>
    </div>
</div>

<!-- bad -->
<style>
.buttons button {
    float: right;
}
</style>

<div class="buttons">
    <button type="button">取消</button>
    <button type="submit">提交</button>
</div>
```

当使用 JavaScript 进行表单提交时，如果条件允许，应使原生提交功能正常工作。

> 当浏览器 JS 运行错误或关闭 JS 时，提交功能将无法工作。如果正确指定了 form 元素的 action 属性和表单控件的 name 属性时，提交仍可继续进行。

```
<form action="/login" method="post">
    <p><input name="username" type="text" placeholder="用户名"></p>
    <p><input name="password" type="password" placeholder="密码"></p>
</form>
```

在针对移动设备开发的页面时，根据内容类型指定输入框的 type 属性。根据内容类型指定输入框类型，能获得能友好的输入体验。

```
<input type="date">
```

多媒体
------

当在现代浏览器中使用 audio 以及 video 标签来播放音频、视频时，应当注意格式。

音频应尽可能覆盖到如下格式：

  - MP3
  - WAV
  - Ogg

视频应尽可能覆盖到如下格式：

  - MP4
  - WebM
  - Ogg

在支持 HTML5 的浏览器中优先使用 audio 和 video 标签来定义音视频元素。

使用退化到插件的方式来对多浏览器进行支持。

```
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <object width="100" height="50" data="audio.mp3">
        <embed width="100" height="50" src="audio.swf">
    </object>
</audio>

<video width="100" height="50" controls>
    <source src="video.mp4" type="video/mp4">
    <source src="video.ogg" type="video/ogg">
    <object width="100" height="50" data="video.mp4">
        <embed width="100" height="50" src="video.swf">
    </object>
</video>
```

只在必要的时候开启音视频的自动播放。

在 object 标签内部提供指示浏览器不支持该标签的说明。

```
<object width="100" height="50" data="something.swf">DO NOT SUPPORT THIS TAG</object>
```

模版JinJa2中的HTML
------------------

模板代码的缩进优先保证 HTML 代码的缩进规则。

```
<!-- good -->
{% if display %}
<div>
    <ul>
    {% for item in items %}
        <li>{{ item.name }}<li>
    {% endfor %}
    </ul>
</div>
{% endif %}

<!-- bad -->
{% if display %}
    <div>
        <ul>
    {% for item in items %}
        <li>{{ item.name }}<li>
    {% endfor %}
        </ul>
    </div>
{% endif %}
```

模板代码应以保证 HTML 单个标签语法的正确性为基本原则。

```
<!-- good -->
<li class="{% if item.id == active %}focus{% endif %}">{{ item.name }}</li>

<!-- bad -->
<li{% if item.id == active %} class="focus"{% endif %}>{{ item.name }}</li>
```

在循环处理模板数据构造表格时，若要求每行输出固定的个数，建议先将数据分组，之后再循环输出。

```
<!-- good -->
<table>
    {% for row in rows %}
    <tr>
        {% for item in row %}
        <td>{{ item.name }}</td>
        {% endfor %}
    </tr>
    {% endfor %}
</table>

<!-- bad -->
<table>
    <tr>
    {% for item in datas %}
        <td>{{ item.name }}}</td>
        {% if loop.index % 5 == 0 %}
    </tr>
    <tr>
        {% endif %}
    {% endfor %}
    </tr>
</table>
```

  [1]: https://github.com/ecomfe/spec/blob/master/html-style-guide.md
  [2]: https://google.github.io/styleguide/htmlcssguide.xml
  [3]: http://www.cs.tut.fi/%7Ejkorpela/html5.dtd
  [4]: https://developer.apple.com/library/mac/documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html#//apple_ref/doc/uid/TP40006509-SW26
  [5]: https://developer.yahoo.com/performance/rules.html#emptysrc
  [6]: http://w3help.org/zh-cn/causes/CM2001
