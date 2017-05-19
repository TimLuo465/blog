---
title: 前端性能优化之字体@font-face
tags:
  - performance
date: 2016-06-23 23:05:53
---

#### @font-face

使用**<span style="color: #808000; font-size: 14px;">@font-face</span>**，可以定义某个特定字体资源的位置，其样式特征用于网页。<!--more-->

#### 用法示例

{% codeblock lang:css %}
@font-face {
 font-family: 'Awesome Font';
 font-style: italic;
 font-weight: 400;
 src: local('Awesome Font Italic'),
 url('/fonts/awesome-i.woff2') format('woff2'), 
 url('/fonts/awesome-i.woff') format('woff'),
 url('/fonts/awesome-i.ttf') format('ttf'),
 url('/fonts/awesome-i.eot') format('eot');
}
{% endcodeblock %}

#### 使用细节

**<span style="color: #000000;">①.</span>** 使用**<span style="color: #808000; font-size: 14px;">local()</span>**指令，我们可以引用、加载和使用本地安装的字体

<span style="color: #000000;">**②.**</span> 使用**<span style="color: #808000; font-size: 14px;">url()</span>**指令，我们可以加载外部字体，并且该指令可以包含一个可选的<span style="font-size: 14px;">**<span style="color: #808000;">format()</span>**</span>提示，指示由提供的网址所引用的字体的格式

**<span style="color: #000000;">③.</span> **对大型 <span style="color: #808000; font-size: 14px;">**unicode** </span>字体进行子集内嵌以提高性能：使用<span style="font-size: 14px;">**<span style="color: #808000;"> unicode-range</span>**</span> 子集内嵌，并为较旧的浏览器提供手动子集内嵌回退

**<span style="color: #000000;">④.</span>** 减少风格字体变体的数量以改进网页和文本呈现性能

#### 字体格式

现在网络上使用的字体容器格式有四种：<span style="font-size: 14px; color: #333333;">**EOT**</span>、<span style="font-size: 14px; color: #333333;">**TTF**</span>、<span style="font-size: 14px; color: #333333;">**WOFF **</span>和 <span style="font-size: 14px; color: #333333;">**WOFF2**</span>。遗憾的是，无论选择的范围有多宽，都不会有在所有旧浏览器和新浏览器上都可以使用的单一通用格式：<span style="font-size: 14px; color: #333333;">**EOT **</span>仅 <span style="font-size: 14px;">**<span style="color: #808000;">IE</span>**</span> 支持，<span style="font-size: 14px; color: #333333;">**TTF **</span>具有 部分 **<span style="color: #808000;">IE</span> **支持，<span style="font-size: 14px; color: #333333;">**WOFF **</span>的支持最广泛，但 它在许多较旧的浏览器中不可用，<span style="color: #333333;">**<span style="font-size: 14px;">WOFF 2.0</span>**</span> 支持 对于许多浏览器来说还未实现。

*   将<span style="color: #333333;"> **<span style="font-size: 14px;">WOFF 2.0</span>** </span>变体提供给支持它的浏览器
*   将 <span style="font-size: 14px; color: #333333;">**WOFF** </span>变体提供给大多数浏览器
*   将 **<span style="font-size: 14px; color: #808080;"><span style="color: #333333;">TTF</span> </span>**变体提供给旧 <span style="font-size: 14px;">**<span style="color: #808000;">Android</span>**</span>（4.4 版以下）浏览器
*   将 <span style="font-size: 14px; color: #333333;">**EOT **</span>变体提供给旧 <span style="font-size: 14px;">**<span style="color: #808000;">IE</span>**</span>（IE9 之下）浏览器

还有一种<span style="font-size: 14px; color: #333333;">**SVG**</span>字体，因为兼容性和用途有限，可以忽略不提。

#### 压缩字体大小

一般情况下，可以在服务器端配置GZIP压缩，可以有效的减小字体文件大小。

可以考虑使用 <span style="font-size: 14px;">**<span style="color: #333333;">Zopfli</span> **</span>压缩 处理 **<span style="font-size: 14px; color: #333333;">EOT</span>**、**<span style="font-size: 14px; color: #333333;">TTF </span>**和 **<span style="font-size: 14px; color: #333333;">WOFF </span>**格式。**<span style="color: #333333; font-size: 14px;">Zopfli </span>**是一个 **<span style="font-size: 14px; color: #333333;">zlib </span>**兼容压缩工具，该工具通过 **<span style="color: #333333; font-size: 14px;">gzip </span>**提供 **<span style="color: #333333; font-size: 14px;">~5%</span>** 的文件大小缩减。

#### 使用Unicode-range 子集内嵌

使用 **<span style="color: #808000; font-size: 14px;">unicode-range</span>** 描述符，我们可以指定一个范围值的逗号分隔列表，其中每个可以采用以下三种不同的形式之一：

*   单一代码点（例如 U+416)
*   间隔范围（例如 U+400-4ff）：指示范围的开始代码点和结束代码点
*   通配符范围（例如 U+4??): ? 字符指示任何十六进制数字
{% codeblock lang:css %}
@font-face {
 font-family: 'Awesome Font';
 font-style: normal;
 font-weight: 400;
 src: local('Awesome Font'),
 url('/fonts/awesome-l.woff2') format('woff2'), 
 url('/fonts/awesome-l.woff') format('woff'),
 url('/fonts/awesome-l.ttf') format('ttf'),
 url('/fonts/awesome-l.eot') format('eot');
 unicode-range: U+000-5FF; /* Latin glyphs */
}

@font-face {
 font-family: 'Awesome Font';
 font-style: normal;
 font-weight: 400;
 src: local('Awesome Font'),
 url('/fonts/awesome-jp.woff2') format('woff2'), 
 url('/fonts/awesome-jp.woff') format('woff'),
 url('/fonts/awesome-jp.ttf') format('ttf'),
 url('/fonts/awesome-jp.eot') format('eot');
 unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
}
{% endcodeblock %}

通过使用 **<span style="color: #808000; font-size: 14px;">unicode range</span>** 子集以及为字体的每种样式变体使用单独的文件，我们可以定义一个复合字体系列，该系列下载起来更快、更有效 - 访问者将仅下载变体及变体需要的子集，而不会强制他们下载他们可能从未在网页上看到或使用过的子集。

在浏览器不支持<span style="font-size: 14px;">**<span style="color: #808000;">unicode range</span>**</span>的情况下，浏览器会下载所有字体。

#### 优化加载和呈现

字体的延迟加载可能会延迟文本呈现，主要原因是由于**<span style="font-size: 14px; color: #808000;">浏览器必须 构造呈现树</span>**，这依赖于 <span style="color: #333333; font-size: 14px;">**DOM** </span>和 <span style="font-size: 14px; color: #333333;">**CSSOM** </span>树，在此之后，它将知道它将需要哪些字体资源来呈现文本。因此，会将字体请求很好地延迟到其他关键资源之后，并且在取回资源之前可能会阻止浏览器呈现文本。

![font-crp](/images/font-crp.png)

1.  浏览器请求 <span style="font-size: 14px; color: #808000;">**<span style="color: #333333;">HTML</span> **</span>文档
2.  浏览器开始解析 <span style="color: #333333;">**<span style="font-size: 14px;">HTML</span>** </span>响应并构造 <span style="color: #333333;">**<span style="font-size: 14px;">DOM</span>**</span>
3.  浏览器发现 <span style="color: #333333;">**<span style="font-size: 14px;">CSS</span>**</span>、<span style="color: #333333;">**<span style="font-size: 14px;">JS</span> **</span>和其他资源并分派请求
4.  收到所有 <span style="font-size: 14px; color: #333333;">**CSS **</span>内容之后，浏览器会立即构造 <span style="color: #333333;">**<span style="font-size: 14px;">CSSOM</span>**</span>，并将其与 <span style="color: #333333;">**<span style="font-size: 14px;">DOM</span> **</span>树组合到一起来构造呈现树

        *   在呈现树指明需要哪些字体变体来呈现网页上的指定文本之后，会立即分派字体请求
5.  浏览器执行布局，并将内容绘制到屏幕上

        *   如果字体还不可用，浏览器可能不会呈现任何文本像素
    *   字体可用之后，浏览器会立即绘制文本像素

网页内容的首次绘制（在构建呈现树之后可以很快完成）和字体资源请求之间的’比赛’产生了’空白文本问题’，这种情况下**<span style="color: #808000; font-size: 14px;">浏览器可能会呈现网页布局而忽略任何文本</span>**。在不同浏览器之间实际的行为会有所不同：

*   <span style="font-size: 14px;">**<span style="color: #333333;">Safari</span> **</span>在字体下载完成之前会暂停文本呈现。
*   **<span style="color: #333333; font-size: 14px;">Chrome </span>**和 <span style="font-size: 14px;">**<span style="color: #333333;">Firefox</span> **</span>会暂停字体呈现最多 **<span style="font-size: 14px; color: #808000;">3</span>** 秒钟，<span style="font-size: 14px;">**<span style="color: #808000;">3</span>**</span> 秒钟之后它们会使用一种备用字体，并且字体下载完成之后，它们会立即使用下载的字体重新呈现一次文本。
*   如果请求字体还不可用，**<span style="color: #333333; font-size: 14px;">IE </span>**会立即使用备用字体呈现，并在字体下载完成之后马上重新呈现。

**使用字体加载 API 优化字体呈现**

{% codeblock lang:javascript %}
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', 
  unicodeRange: 'U+000-5FF', weight: '400'
});

font.load(); /* don't wait for render tree, initiate immediate fetch! */

font.ready().then(function() {
/* apply the font (which may rerender text and cause a page reflow)
   once the font has finished downloading */
document.fonts.add(font);
document.body.style.fontFamily = "Awesome Font, serif";

/* OR... by default content is hidden, and rendered once font is available */
var content = document.getElementById("content");
content.style.visibility = "visible";
});
{% endcodeblock %}

通过这种方式可以定义和操纵 CSS 字体外观，跟踪其下载进度，并覆盖其默认延迟加载行为。