## 安装

```
1 =================================
sass基于Ruby语言开发而成，因此安装sass前需要安装Ruby

2 ====================================
//删除替换原gem源
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
//打印是否替换成功
gem sources -l
https://gems.ruby-china.com
# 确保只有 gems.ruby-china.com
3 ==============================

正常情况下，你是不会遇到 SSL 证书错误的，除非你的 Ruby 安装方式不正确。

如果遇到 SSL 证书问题，你又无法解决，请修改 ~/.gemrc 文件，增加 ssl_verify_mode: 0 配置，以便于 RubyGems 可以忽略 SSL 证书错误。

4 =========================
sass 安装
gem install sass
gem install compass


5. ============================
//单文件转换命令
sass input.scss output.css

//单文件监听命令
sass --watch input.scss:output.css

//如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
sass --watch app/sass:public/stylesheets
```

## 快速入门

### 变量

```
1-1. 变量声明;
使用 $ 符号来标识一个变量
$highlight-color: #f90;

1-2. 变量引用;
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}
//编译后
.selected {
  border: 1px solid #F90;
}

1-3.变量名分隔
$link-color: blue;
a{
color:$link_color
}

编译后
a{
color:blue;
}
```

### 嵌套 css 规则

```
组合css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
编译前
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }


2-1. 父选择器的标识符&;
article a {
  color: blue;
  &:hover { color: red }
}

article a { color: blue }
article a:hover { color: red }

2-2. 群组选择器的嵌套;

.container h1, .container h2, .container h3 { margin-bottom: .8em }

.container {
  h1, h2, h3 {margin-bottom: .8em}
}

3-2. 默认变量值;
一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值。举例说明：

$link-color: blue;
$link-color: red;
a {
color: $link-color;
}

===================== 默认变量 ==================
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
在上例中，如果用户在导入你的sass局部文件之前声明了一个$fancybox-width变量，那么你的局部文件中对$fancybox-width赋值400px的操作就无效。如果用户没有做这样的声明，则$fancybox-width将默认为400px。
```

###  静默注释;

```
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

### 5. 混合器;

```
1.
混合器使用@mixin标识符定义。看上去很像其他的CSS @标识符，比如说@media或者@font-face。这个标识符给一大段样式赋予一个名字，这样你就可以轻易地通过引用这个名字重用这段样式。下边的这段sass代码，定义了一个非常简单的混合器，目的是添加跨浏览器的圆角边框。

@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
2.
然后就可以在你的样式表中通过@include来使用这个混合器，放在你希望的任何地方。@include调用会把混合器中的所有样式提取出来放在@include被调用的地方。如果像下边这样写：

notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}

5. 混合器传参数
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

使用==============
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }

```



