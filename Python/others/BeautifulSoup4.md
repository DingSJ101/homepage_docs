---
date: 2022-09-23 21:51:27.984574
lastmod: 2022-09-29 19:55:13.738434
---
# BeautifulSoup4

reference:https://www.crummy.com/software/

## 数据结构

### tag

基本操作对象为`tag`，与XML或HTML原生文档中的tag相同。

使用`bs.find()`或`bs.find_all()`方法、以及`bs.title`、`bs.div`等方法获得的都是该对象，对象的属性如下：

1. tag.name

   tag标签的名字

2. tag.attrs

   tag标签的所有属性，返回字典，如属性为多值属性，则该属性的键值为列表。属性包括'class','href','src'等

3. tag.<tag_name>

   查找tag内名字为tag_name的子tag，返回第一个结果

   - soup.head
   - soup.title
   - soup.body.b
   - tag.a

4. tag.string / tag.text

   返回tag或NavigableString中的文本，类型为NavigableString。如果tag内含有tag则返回None。

5. tag.strings

   返回tag中所有字符串（包括子孙节点）

6. tag.contents

   获取tag的所有子节点，输出列表，列表中元素为tag或者NavigableString字符串

   tag.children 返回生成器

7. tag.parent 

   返回父节点

   tag.parents 递归返回所有父节点，生成器

8. tag.descendants

   获取tag的所有子孙节点，返回生成器

9. tag.next_sibling / tag.previous_sibling

   返回tag的兄弟节点，实际文档中的tag的 `.next_sibling` 和 `.previous_sibling` 属性通常是字符串或空白。

方法如下：

1. .has_attr('attr')

   返回布尔值，该tag是否有attr属性
   
2. .get('attr')

   获取tag的attr属性值

## 常用函数

> find()和find_\*()函数参数中的过滤器定义如下
> | 类型                | 效果                                                         |
| ------------------- | ------------------------------------------------------------ |
| 字符串              | 匹配tag的name属性                                            |
| re.compile(pattern) | 使用re.compile.search()方法匹配tag的name属性                 |
| 列表                | 使用列表中任意的元素，匹配tag的name属性                      |
| TRUE                | 返回除字符串节点外的所有tag                                  |
| 函数                | 作为name或string参数时，函数只接受tag作为唯一参数，如果返回True则选中该tag；作为keyword参数的键值时，接收唯一参数str表示tag对象的某个属性，如果返回True则选中该tag。 |

### soup.find()

函数原型：`find(name, attrs, recursive, string, **kwargs)`

```python 
html =  open('./aa.html', 'rb').read()
bs = BeautifulSoup(html,"lxml") 
print(bs.find_all("a")) # 获取所有的a标签
print(bs.find(id="u1")) # 获取id="u1"
 
for item in bs.find_all("a"):
  print(item.get("href")) # 获取所有的a标签，并遍历打印a标签中的href的值
 
for item in bs.find_all("a"):
  print(item.get_text())
```

###  soup.find_all()

函数原型：`find_all(name, attrs, recursive, string, limit, **kwargs)`

| 参数      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| name      | 过滤器，包括字符串、正则表达式、列表、布尔值、函数等         |
| attrs     | 字典，键为tag的属性，键值为对应的过滤器                      |
| string    | 匹配tag对象的string属性                                      |
| limit     | 返回前limit个结果                                            |
| recursive | 是否匹配子孙节点，default = True                             |
| keyword   | 匹配tag的keyword属性是否符合对应的键值（键值为过滤器），如`soup.find_all(href =  re.compile('jpg')，id=True)`，则会去匹配tag的href属性值是否满足过滤器re.compile.search('jpg')；或者`soup.find('div',style="margin-bottom:20px")`可以匹配`<div style="margin-bottom:20px">`<br /> 由于class为关键字，所以该属性使用class_作为参数，如果class为多值属性时，可以独立搜索每个class类名，如需同时匹配多个属性，则采用`soup.select("<name>.<attr1>.<attr2>")`，reference package:SoupSieve。 |

### soup.find_parents()

函数原型：`find_parents(name, attrs, string, limit, **kwargs)`

从tag的.parents中匹配符合的tag。

> 嵌套的两个<a>标签parents会跳过外层的的<a>。应该是解析的时候封闭了外层的标签。

### soup.find_parent()

函数原型：`find_parent(name, attrs, string, **kwargs)`

### 其他find_*()

`find_next_siblings(name, attrs, string, limit, **kwargs)`
`find_next_sibling(name, attrs, string, **kwargs)`
`find_previous_siblings(name, attrs, string, limit, **kwargs)`
`find_previous_sibling(name, attrs, string, **kwargs)`
`find_all_next(name, attrs, string, limit, **kwargs)`
`find_next(name, attrs, string, **kwargs)`
`find_all_previous(name, attrs, string, limit, **kwargs)`
`find_previous(name, attrs, string, **kwargs)`

### soup.get_text()

函数原型：`soup.text(separator="",strip=False)`

将所有子孙节点的字符部分通过separator拼接，strip参数表示拼接前是否去除前后的空白字符


