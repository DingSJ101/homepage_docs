# base

SQLAlchemy是 Python 著名的 ORM 工具包。通过 ORM，开发者可以用面向对象的方式来操作数据库，不再需要编写 SQL 语句。本篇不解释为什么要使用 ORM，主要讲解 SQLAlchemy 的用法。SQLAlchemy 支持多种数据库，除 sqlite 外，其它数据库需要安装第三方驱动。

## install

```bash
pip install  sqlalchemy ,pymysql
```

# use

## create engine

```python
from sqlalchemy import create_engine
# 使用pymysql驱动连接到mysql
engine = create_engine('mysql+pymysql://user:pwd@localhost/testdb')
# 使用pymssql驱动连接到sql server
engine = create_engine('mssql+pymssql://user:pwd@localhost:1433/testdb')
```

## create model

```python
from sqlalchemy.ext.declarative import declarative_base
Base = declarative_base()
# 通过 declarative_base() 函数创建 Base 类，Base 类本质上是 一个 registry 对象
class Employee(Base):
    __tablename__ = 'employees'
    EMP_ID = Column(SmallInteger, primary_key=True)
    FIRST_NAME = Column(String(255))
    AGE = Column(SmallInteger)
```

### create by sqlacodegen

```bash
pip install sqlacodegen 
sqlacodegen  mysql+pymysql://user:pwd@localhost/testdb # 根据数据库生成所有数据表的模型
sqlacodegen  mysql+pymysql://user:pwd@localhost/testdb --tables users, addresses# 指定部分表
```

### auto-reate

```python
from sqlalchemy import  create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.sql.schema import Table

engine = create_engine("sqlite:///testdb.db")
Base = declarative_base()
metadata = Base.metadata
metadata.bind = engine

class Employee(Base):
    __table__ = Table("employees", metadata, autoload=True) # 使用autoload参数，需要先绑定engine和Base
```

## CRUD

SQLAlchemy 操作数据库，需要引入另外一个对象 Session。Session 建立与数据库的会话 (conversation)，可以将其想象成对象的容器，包含的对象叫 identity map 的结构，identity map 的作用就是保证对象的唯一性。另外，Session 对 Python 对象进行状态管理。

首先，需要构建一个 Session 对象，比较常用的方式是使用 sessionmaker() 函数来创建一个 global 的 Session Factory，进行调用后就生成 Session 对象：

```python
from sqlalchemy.orm import sessionmaker
engine = create_engine(SQL_DB_URL, echo=False)
Session = sessionmaker(bind=engine)
session = Session()
```



### Create

```python
session.add(Employee()) # 添加模型实例
session.add_all([Users(name="lqz"),Users(name="egon"),Hosts(name="c1.com")])
session.commit()
```

### Read

```python
res = session.query(Employee).all() #  模型作为 query() 方法的参数，返回列表，元素为模型对象
res = session.query(Employee.EMP_ID, Employee.FIRST_NAME, Employee.LAST_NAME).all() #  字段作为 query() 方法的参数，返回列表，元素为元组，元组内为字段
## select
res = session.query(Employee).filter_by(EMP_ID='1001').first() # filter_by(字段)
res = session.query(Employee).filter(Employee.EMP_ID == '1001').first() # filter(条件) 

#　条件
ret = session.query(Users).filter_by(name='lqz').all()
#表达式，and/or条件连接
ret = session.query(Users).filter(Users.id > 1, Users.name == 'eric').all()
ret = session.query(Users).filter((Users.id.between(1, 3), Users.name == 'eric').all()
#注意下划线
ret = session.query(Users).filter(Users.id.in_([1,3,4])).all()
#~非，除。。外
ret = session.query(Users).filter(~Users.id.in_([1,3,4])).all()
#二次筛选
ret = session.query(Users).filter(Users.id.in_(session.query(Users.id).filter_by(name='eric'))).all()
from sqlalchemy import and_, or_
#or_包裹的都是or条件，and_包裹的都是and条件
ret = session.query(Users).filter(and_(Users.id > 3, Users.name == 'eric')).all()
ret = session.query(Users).filter(or_(Users.id < 2, Users.name == 'eric')).all()

# 通配符
ret = session.query(Users).filter(Users.name.like('e%')).all()

# 限制，用于分页，区间
ret = session.query(Users)[1:2]

# 排序，根据name降序排列（从大到小）
ret = session.query(Users).order_by(Users.name.desc()).all()
#第一个条件重复后，再按第二个条件升序排
ret = session.query(Users).order_by(Users.name.desc(), Users.id.asc()).all()

# 分组
from sqlalchemy.sql import func

ret = session.query(Users).group_by(Users.extra).all()
#分组之后取最大id，id之和，最小id
ret = session.query(
    func.max(Users.id),
    func.sum(Users.id),
    func.min(Users.id)
	).group_by(Users.name).all()
#haviing筛选
ret = session.query(
    func.max(Users.id),
    func.sum(Users.id),
    func.min(Users.id)
	).group_by(Users.name).having(func.min(Users.id) >2).all()

# 连表（默认用forinkey关联）
ret = session.query(Users, Favor).filter(Users.id == Favor.nid).all()
#join表，默认是inner join
ret = session.query(Person).join(Favor).all()
#isouter=True 外连，表示Person left join Favor，没有右连接，反过来即可
ret = session.query(Person).join(Favor, isouter=True).all()
#打印原生sql
aa=session.query(Person).join(Favor, isouter=True)
print(aa)
# 自己指定on条件（连表条件）,第二个参数，支持on多个条件，用and_,同上
ret = session.query(Person).join(Favor,Person.id==Favor.id, isouter=True).all()
# 组合（了解）UNION 操作符用于合并两个或多个 SELECT 语句的结果集
#union和union all的区别？
q1 = session.query(Users.name).filter(Users.id > 2)
q2 = session.query(Favor.caption).filter(Favor.nid < 2)
ret = q1.union(q2).all()

q1 = session.query(Users.name).filter(Users.id > 2)
q2 = session.query(Favor.caption).filter(Favor.nid < 2)
ret = q1.union_all(q2).all()

```

### Update

```python
emp = session.query(Employee).filter_by(EMP_ID='9002').first() # 获取记录对象
emp.AGE = '21' # 修改属性
session.query(Employee).filter_by(EMP_ID='9002').update({"AGE" : "21"}) # update(dict)
session.commit()
```

### Delete

```python
emp = session.query(Employee).filter_by(EMP_ID='9002').first() # 获取记录对象
session.delete(emp) # 删除对象
session.commit()
```



## format

为了能更加友好的输出，可以在 Employee 类中编写 `__repr__` 方法，以 dict 类型输出数据表的内容。为了实现通用性，我使用 `__dict__` 属性获取 Employee 的字段，将方法放在专门的扩展类中：

```python
import json
# 为了能更加友好的输出，在 Employee 类中编写 __repr__ 方法，以 dict 类型输出数据表的内容。
# 为了实现通用性，使用 __dict__ 属性获取 Employee 的字段，将方法放在专门的扩展类中：
class ModelExt(object):
    """
    Model extension, implementing `__repr__` method which returns all the class attributes
    """
    def __repr__(self):
        fields = self.__dict__
        if "_sa_instance_state" in fields:
            del fields["_sa_instance_state"]
        return json.dumps(fields) 
    
# 利用 tablib 将数据转换为格式化的输出，方便查看，代码如下：
def to_formatted_table(tab_data):
    """
    tab_data is supposed to be of type list(dict)
    """
    ds = tablib.Dataset()
    return(ds.load(str(tab_data)))

# 利用 Python 的多重继承机制，将 Model 类增加一个父类 ModelExt:
from sqlalchemy.ext.declarative import declarative_base
class Employee(declarative_base(),ModelExt):
    __tablename__ = 'employees'
    EMP_ID = Column(SmallInteger, primary_key=True)
    FIRST_NAME = Column(String(255))
    AGE = Column(SmallInteger)
    
# print
employees = session.query(Employee).all()
print(to_formatted_table(employees))
```





