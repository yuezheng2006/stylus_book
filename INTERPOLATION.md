插值
===
Stylus支持插入值，通过使用`{}`中包裹的表达式，表达式的结果将会变为定义的一部分。例如，`-webkit-{'border'＋’－radius‘`等同于`-webkit-border-radius`.

一个很好的例子就是对于私有前缀额扩展属性。

```
vendor(prop,args)
    -webkit-{prop} args
    -moz-prop args
    {prop} args

border-radius()
    vendor{'border-radius',augments}

box-shadow()
    vendor('box-shadow',augments)

button
    border-radius 1px 2px / 3px 4px
```

生成为:

```
button {
    -webkit-border-radius: 1px 2px / 3px 4px;
    -moz-border-radius: 1px 2px / 3px 4px;
    border-radius: 1px 2px / 3px 4px;
}
```

## 选择器插值

插值也可以在选择器中运用到。例如，我可以插入值去指定一个表格中前５行的高属性值，向下面这样：

```
table
    for row in 1 2 3 4 5
        tr:nth-child({row})
            height:10px*row
```

生成为:

```
table tr:nth-child(1){
    height:10px;
}

table tr:nth-child(1){
    height:20px;
}

table tr:nth-child(1){
    height:30px;
}

table tr:nth-child(1){
    height:40px;
}

table tr:nth-child(1){
    height:50px;
}
```

你也可以把多个选择器当做一个字符串放在一个变量中，去插入到他们应该在的地方：

```
mySelector = '#foo,#bar,.bar'

{mySelectors}
    background:#000
```

变成为:

```
#foo,
#bar,
.baz{
    background:#000l
}
```
