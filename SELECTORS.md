选择器
===

## 缩进
Stylus看起来很神奇(因为缩进).在这里空格尤其重要，因为我们用缩进和减少缩进来替代了 `{` 和 `}` ,就像下面的代码:
```
body
    color white
```
它会编译成这样：
```
body
    color:#fff
```
如果你愿意，你也可以使用毛和来分隔属性和值:
```
body
    color:white
```

## 规则集
Stylus也和CSS一样，允许你使用逗号分隔来为多个选择器定义属性.
```
textarea,input
    border 1px solid #eee
```
使用新行也是一样:
```
textarea
input
    border 1px solid #eee
```
唯一例外的情况是那些看起来像属性的选择器。例如`foo bar baz`可能是一个属性或是一个选择器
```
foo bar baz
>input
    border 1px solid
```
因为这个原因，我们可能需要在结尾加上一个逗号：
```
foo bar baz,
form input,
> a
    border 1px solid
```

## 父集引用
`&`符号将引用父选择器。下面的例子中，两个选择器(`textarea`和`input`)会在`:hover`伪类选择器上都更新`color`值。
```
textarea
input
    color #A7A7A7
    &:hover
        color #000
```
会被编译为：
```
textarea,
input{
    color:#A7A7A7;
}

textarea:hover,
input:hover{
    color：#000
}
```

下面这里例子，将在混合书写中使用父集引用在IE浏览器下实现一个`2px`的边框：
```
box-shadow()
    -webkit-box-shadow arguments
    -moz-box-shadow arguments
    -box-shadow arguments
    html.ie8 &,
    html.ie7 &,
    html.ie6 &
        border 2px solid arguments[length(arguments)-1]

body
    #login
    box-shadow 1px 1px 3px #eee
```
会变成这个样子：
```
body #login{
    -webkit-box-shadow: 1px 1px 3px #eee;
    -moz-box-shadow: 1px 1px 3px #eee;
    box-shadow: 1px 1px 3px #eee;
}

html.ie8 body #login,
html.ie7 body #login,
html.ie6 body #login {
    border: 2px solid #eee;
}
```
如何你不需要`&`在选择器之外也像一个父引用一样，你可以使用这样来避免:
```
.foo[title*=‘\&']
//=> .foo[title*='&']
```

## 部分引用
`^[N]`出现在选择器的任意位置，`N`是一个数字，表示一个部分引用。
部分引用用法向父集引用一样，但是父引用包含着整个选择器，而部分引用仅包含着嵌套选择器的第`N`级，所以你可以单独引用潜逃中的某个级别。
`^[0]`将获取到第一级选择器，`^[1]`将获取到第二级选择器元素，以此类推:
```
.foo
    &_bar
        width:10ox

        ^[0]:forver &
            width:20px
```
这将被渲染为:
```
.foo_bar{
    width:10px;
}

.foo:hover .foo_bar{
    width:20px;
}
```
负值将从结尾开始结算,所以`^[-1]`将返回`&`链前的最后一个选择器:
```
.foo
    &_bar
        &_baz
            width:10px

            ^[-1]:hover &
            width:20px
```

这将被渲染为:
```
.foo_bar_baz{
    width:10px;
}
.foo_bar:hove .foo_bar_baz{
    width：20;
}
```

在回合书写中，当你不知道嵌套级别的时候，负值会是特别有用的用法去帮你找到它。

*请记住部分引用在你给出获取的嵌套级别之前他将包含整个选择器渲染链，而不是选择器的一部分*

## 部分引用中的范围

`^[N..M]`出现在选择器的任何位置，`N`和`M`是一个数字，表示一个部分引用。

如果你有这种情况，当你需要去获取选择其中的部分或者获取这个范围的部分程序，你可能需要在部分引用中使用范围。

如果范围从正值开始，这结果将不包含先前级别的选择器，你获得的结果中那些级别的选择器会被插入到根部并忽略组合(*翻译的不一定正确*)。
```
.foo
    & .bar
        width：10px
    
        ^[0]:hover ^[1..-1]
            width:20px
```
它将会渲染为:
```
.foo .bar{
    width:10px;
}
.foo:hover .bar{
    width:20px;
}
```

在这范围中一个数据会作为开始位置，另一个是结束位置，注意的是那些数字的顺序其实是无所谓的，他总会被从小到大进行渲染，如`^[1..-1]`和`^[-1..1]`是等同的。

当两个数字相等时，结果将会仅获取选择气的一级，所以在上个例子中你可以用`^[-1..-1]`代替`^[1..-1]`，它们都会返回最后一级选择器，如果在混合书写中使用它你会觉得更加可靠。

## 初始引用

`~/`符号在选择器的开始会被只想选择器嵌套的开始，可以认为他是`^[0]`的快捷方式。唯一的缺点就是你仅可以在选择器的开始使用它。

```
.block
    &_element
        ~/:hover &
            color:red
```
会被渲染为:
```
.block:hover .block_element{
    color:#foo
}
```

## 相对引用










