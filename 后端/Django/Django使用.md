---
abbrlink: b6814d96
title: Django使用
categories:
  - 后端
  - Django
---
## 创建项目

```django
django-admin startproject pro_name
```

## 数据库

```mysql
# mysql建库及权限
CREATE USER 'aicare'@'localhost' IDENTIFIED BY '123456';
grant reference,index,select,insert,update on *.* to 'aicare'@'localhost';
CREATE DATABASE `aicare` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci ;

```

```python
#settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'db_name',# 数据库名
        'HOST': '127.0.0.1', # 数据库地址，本机 ip 地址 127.0.0.1 
        'PORT': 3306, # 端口 
        'USER': 'sqlpro',  # 数据库用户名
        'PASSWORD': 'sqlpro', # 数据库密码
    }
}
```

```bash
# 更新数据库
python manage.py makemigrations #更新
python manage.py migrate #执行
```

## 创建应用

```bash
python manage.py startapp app_name # 在同级目录创建文件夹
```

```python
# 注册应用 pro_name/pro_name/settings.py
INSTALLED_APPS = [
	'app_name',
]
```

## 路由

```python
# 主路由 pro_name/pro_name/urls.py
from django.urls import path,include
from . import app_name
urlpatterns=[
    #http://127.0.0.1:8000/path_1
    path('path_1',views.my_view,name='n1') # 调用my_view函数处理，返回网页对象，网址反向解析为n1
    #http://127.0.0.1:8000/path_2/*
    path('path_2',include('app_name.urls',namespace='n2')) #分发给应用的路由，网址反向解析
]
```

```python
# 应用路由 pro_name/app_name/urls.py 需创建
urlpatterns=[
    #http://127.0.0.1:8000/path_2/page_1
    path('page_1',view.my_view),
]
```

## 模型Models

```python
# pro_name/app_name/models.py
## 定义模型：从models.Model 继承
from django.db import models
class my_model(models.Model):
    id = models.AutoField(primary_key=True) # id 会自动创建,可以手动写入
    field_1 = models.CharField(max_length=20) #varchar
    field_2 = models.DecimalField(max_digits=5, decimal_places=2)  #float
    field_3 = models.DateField() #datetime
```

```python
# 增加数据
### 方法一：实例化对象并保存
    record = models.my_model(field_1="1",...,field_n="n")
    record.save()
### 方法二：ORM的create方法
	record = models.my_model.objects.create(field_1="1",...,field_n="n")
```

```python
# 查找 :返回QuerySet对象
	records = models.my_model.objects.all() #返回所有记录,每个记录存为一个字典
	records = models.my_model.objects.values(field_1,field_2) #返回部分列
	records = models.my_model.objects.values_list(field_1,field_2) #返回部分列，每个记录存为一个元组
	records = models.my_model.objects.values(field_1,field_2) #返回部分列
    
	records = models.my_model.objects.filter(sql_condition)#查询id时使用pk作为字段名(primary key)
    records = models.my_model.objects.exclude(sql_condition)#返回不符合条件的记录
    records = models.my_model.objects.get(sql_condition) #查询特定的一条记录，如果结果不是一条，则抛出错误
    """sql_condition
    	## 等值查询		field_1="1",field_2="2"
    	## 非等值查询,使用查询谓词
    		__exact			等值匹配	id__exact = 1
    		__contains		包含字段 name__contains='W'
    		__icontains 	包含字段(不区分大小写)
    		__startswith	以xx开始
    		__endswith		以xx结束
    		__year/month/day	判断 DateField 数据类型
    		__gt/__gte		大于/大于等于	age__gt=50
    		__lt/__lte		小于/小于等于
    		__in			数据是否在列表中	s__in=[1,2,3]
    		__range			数据是否在区间		s__range=(10,30)
    """ 
    """其他条件
    	order_by('-field_1','field_2') # 字段前加 - 表示降序
    	reverse()	# 对查询结果进行反转
    	count()	# 查询返回的记录数
    	first()	# 返回第一条记录
    	last()	# 返回最后一条记录
    	exists() # 判断查询是否有结果，返回布尔值
    	distinct()	# 去重
    """
```

```python
# 修改
## 方法一：返回所编辑的模型类对象。
	record = models.my_model.objects.filter(condition).first()
	record.field_1 = '1'
	record.save()
## 方法二 ：返回受影响的行数
	pk = models.my_model.objects.filter(condition).update(field_1='1')
```

```python
# 删除,返回元组，第一个元素为受影响的行数
## 方法一，删除模型类的对象
	record = models.my_model.objects.filter(pk=8).first().delete()
## 方法二，删除QuerySet
	records = models.my_model.objects.filter(pk=8).delete()
```

## 视图 Views



## 模板 Templates

```django
{# view 中变量#}
	{'var_name':'views_name'} # 通过字典传递给模板

{# 模板中变量#}
    {{ var_name }}
    {{ var_name.0 }} #列表
    {{ var_name.name }} #字典
    {{ var_name | filter_1 [:para_1] | filter_2 [:para_2] }} #过滤器
        {# filter: 
			length/lower/upper/first/filesizeformat/safe
             date:"Y-m-d"/truncatewords:"len"/default:"str"
		#}
{# if #}
    {% if condition %}
        ......
    {% elif condition2 %}
        ......
    {% else %}
        ......
    {% endif %}
{# 判断 ifequal/ifnotequal #}
	{% ifequal var_1 var_2 %}
        ......
	{% else %}
		......
	{% endifequal %}
{# for #}
    {% for i in list [reversed] %} / {% for i,j in dict.items %}#获取键值对
        ......
    {# 代码段中变量
        forloop.counter: 顺序获取循环序号，从 1 开始计算
        forloop.counter0: 顺序获取循环序号，从 0 开始计算
        forloop.revcounter: 倒序获取循环序号，结尾序号为 1
        forloop.revcounter0: 倒序获取循环序号，结尾序号为 0
        forloop.first（一般配合if标签使用）: 第一条数据返回 True，其他数据返回 False
        forloop.last（一般配合if标签使用）: 最后一条数据返回 True，其他数据返回 False
    #}
    {% empty %}
        list/dict为空时执行	
    {% endfor %}
{# include标签 #}
	{% include "template_path" %}
{# 静态文件 #}
	{% load static %}
		<img src="{% static "images/1.png" %}">
{# 继承 #}
	{# 父模板文件中 #}
	{% block block_name%}
		......
	{% endblock %}
	{# 子模板文件中 #}
	{% extends "parent_tamplate_path" %}
	{% block block_name%}
		.......
	{% endblock %}
```

