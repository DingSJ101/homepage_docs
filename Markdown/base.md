---
categories:
  - Markdown
tags:
  - latex
abbrlink: ec04897e
title: '[base]'
date: 2022-06-19 19:30:29
---

# 一、标题

```markdown
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题
```

# 二、字体

```
*斜体*或_斜体_
**粗体**
***加粗斜体***
~~删除线~~
++下划线++
==背景高亮==
```

*斜体*或_斜体_
**粗体**
***加粗斜体***
~~删除线~~
++ 下划线 ++
== 背景高亮 ==

# 三、引用

```markdown
>这是引用的内容
>>这是引用的内容
>>>>>>>>>>这是引用的内容
```

> 这是引用的内容
>
> > 这是引用的内容
> >
> > > > > > > > > > 这是引用的内容

# 四、分割线

```latex
//三个或者三个以上的 - 或者 * 
---
*****
//显示效果是一样的
```
---
----
***
*****

# 五、图片

```latex
![图片alt](图片地址 ''图片title'')
//图片alt就是显示在图片下面的文字，相当于对图片内容的解释。
//图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加
```

# 六、超链接

```
[超链接名](超链接地址 "超链接title")
//title可加可不加
```

```
<a href="超链接地址" target="_blank">超链接名</a>
//示例
//<a href="https://www.jianshu.com/u/1f5ac0cf6a8b" target="_blank">简书</a>
```

# 七、列表

## 无序列表

```
- 列表内容
+ 列表内容
* 列表内容

//无序列表用 - + * 任何一种都可以
//- + * 跟内容之间都要有一个空格
```

- 列表内容
- 列表内容
- 列表内容

## 有序列表

```
1. 列表内容
2. 列表内容
3. 列表内容

//序号跟内容之间要有空格
```

1. 列表内容
2. 列表内容
3. 列表内容

## 列表嵌套

```
上一级和下一级之间敲三个空格即可
```

- 一级无序列表内容
  - 二级无序列表内容
  - 二级无序列表内容
  - 二级无序列表内容
- 一级无序列表内容
  1. 二级有序列表内容
  2. 二级有序列表内容
  3. 二级有序列表内容

1. 一级有序列表内容
   - 二级无序列表内容
   - 二级无序列表内容
   - 二级无序列表内容
2. 一级有序列表内容
   1. 二级有序列表内容
   2. 二级有序列表内容
   3. 二级有序列表内容

# 八、表格

```
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容

//第二行分割表头和内容。
//- 有一个就行，为了对齐，多加了几个
//文字默认居左
	//-两边加：表示文字居中
	//-右边加：表示文字居右
//注：原生的语法两边都要用 | 包起来。此处省略
```

```
姓名|技能|排行
--|:--:|--:
刘备|哭|大哥
关羽|打|二哥
张飞|骂|三弟
```

效果如下：

| 姓名 | 技能 | 排行 |
| ---- | :--: | ---: |
| 刘备 |  哭  | 大哥 |
| 关羽 |  打  | 二哥 |
| 张飞 |  骂  | 三弟 |

# 九、代码


```
 //单行代码：代码之间分别用一个反引号包起来
 `代码内容`
 
 //代码块：代码之间分别用三个反引号包起来，且两边的反引号单独占一行
​```
  代码...
  代码...
  代码...
​```
```

# 十、流程图

```
​```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
​```
```

```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op

```

# 十一、注释

```html
<div style='display: none'>
代码注释
</div>
<!--HTML单行注释-->

<!--
HTML，
多行注释。
-->
```

# 十二、复选框

```markdown
- [ ] txt1
- [x] txt2
```

- [ ] txt1
- [x] txt2

# 十三、引用

```markdown
text1 [^1] text2
[^1]:引用文字
```

text1 [^1] text2

[^1]:链接url

# 十四、锚点

```
## <span id = "index" > 目录 </span>
[返回](#index)
```

## <span id = "index" > 目录 </span>

[返回](#index)

# 十五、公式字体

$$
\begin {array}{}
文本效果 & 代码 \\
\Huge text & \backslash Huge\space text\\
\huge text & \backslash huge\space text\\
\LARGE text & \backslash LARGE\space text\\
\Large text & \backslash Large\space text\\
\large text & \backslash large\space text\\
\normalsize text & \backslash normalsize\space text\\
text & text(default) \\
\small text & \backslash small\space text\\
\footnotesize text & \backslash footnotesize\space text\\
\scritpsize text & \backslash scritpsize\space text\\
\tiny text & \backslash tiny\space text\\

\end {array}
$$

# 多行公式

基础语法同矩阵，矩阵类型可替换为以下不同样式

## multiline 

$$
\begin{multline}
x = a + b \\
y = c + d + e \\
z = f + g
\end{multline}
$$

## split

$$
\begin{split}
x = a + b \\
y = c + d + e \\
z = f + g
\end{split}
$$

## gather

$$
\begin{gather}
x = a + b \\
y = c + d + e \\
z = f + g
\end{gather}
$$

## align

$$
\begin{align}
x = a + b \\
y = c + d + e \\
z = f + g
\end{align}
$$

## cases

$$
\begin{cases}
x = a + b \\
y = c + d + e \\
z = f + g
\end{cases}
$$

## equation

$$
\begin{equation}
x = a + b \\
y = c + d + e \\
z = f + g
\end{equation}
$$



## 公式的编号和引用

使用`\label{anchor_name}\tag{number}`可以显示编号(number)，并定义为anchor_name的锚点，之后可以通过`eqref{anchor_name}`引用公式的编号。对于不需要编号的行，可以添加`\nonumber`或`\notag`标记。

```latex
$
x = f (x) \label{myeq} \tag{1.a}
$
引用 $\eqref{1.a}$
```

$
x = f (x) \label{myeq} \tag{1.a}
$

引用$\eqref{1.a}$





