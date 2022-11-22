**数组常用函数**

1.where()
按条件返回数组的索引值
2.take(a,index)
从数组a中按照索引index取值
3.linspace(a,b,N)
返回一个在(a,b)范围内均匀分布的数组，元素个数为N个
4.a.fill()
将数组的所有元素以指定的值填充
5.diff(a)
返回数组a相邻元素的差值构成的数组
6.sign(a)
返回数组a的每个元素的正负符号
7.piecewise(a,[condlist],[funclist])
数组a根据布尔型条件condlist返回对应元素结果
8.a.argmax(),a.argmin()
返回a最大、最小元素的索引

**改变数组维度**

a.ravel(),a.flatten():
将数组a展平成一维数组
a.shape=(m,n),a.reshape(m,n):
将数组a转换成m*n维数组
3.a.transpose,a.T
转置数组a

**数组组合**

1.hstack((a,b)),concatenate((a,b),axis=1)
将数组a,b沿水平方向组合
2.vstack((a,b)),concatenate((a,b),axis=0)
将数组a,b沿竖直方向组合
3.row_stack((a,b))
将数组a,b按行方向组合
4.column_stack((a,b))
将数组a,b按列方向组合

**数组分割**

1.split(a,n,axis=0),vsplit(a,n)
将数组a沿垂直方向分割成n个数组
2.split(a,n,axis=1),hsplit(a,n)
将数组a沿水平方向分割成n个数组

**数组修剪和压缩**

1.a.clip(m,n)
设置数组a的范围为(m,n),数组中大于n的元素设定为n,小于m的元素设定为m
2.a.compress()
返回根据给定条件筛选后的数组

**数组属性**

1.a.dtype
数组a的数据类型
2.a.shape
数组a的维度
3.a.ndim
数组a的维数
4.a.size
数组a所含元素的总个数
5.a.itemsize
数组a的元素在内存中所占的字节数
6.a.nbytes
整个数组a所占的内存空间
7.a.astype(int)
转换a数组的类型为int型

**数组计算**

1.average(a,weights=v)
对数组a以权重v进行加权平均
2.mean(a),max(a),min(a),middle(a),var(a),std(a)
数组a的均值、最大值、最小值、中位数、方差、标准差
3.a.prod()
数组a的所有元素的乘积
4.a.cumprod()
数组a的元素的累积乘积
5.cov(a,b),corrcoef(a,b)
数组a和b的协方差、相关系数
6.a.diagonal()
查看矩阵a对角线上的元素
7.a.trace()
计算矩阵a的迹，即对角线元素之和