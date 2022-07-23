---
abbrlink: a028ac0f
title: math_python
categories:
  - Python
  - my_packages
---
```python
from sympy import *
x = Symbol('x')#变量
k = Symbol('k')
y = Function('y')
n=1		#求导层数
qiudao  = diff(y,x,n)		#求导表达式，变量，层数
latex = latex(y)		#latex公式
yinshifenjie= factor(y)	#因式分解
jiangmi = collect(y)		#降幂排列
yuefen = cancel(y)		#约分
huajian  = simplify(y)		#化简
```





