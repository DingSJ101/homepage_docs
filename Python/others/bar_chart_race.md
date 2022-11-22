# install

```bash
pip install bar_chart_race
```

# use

```python
import bar_chart_race as bcr
#下载数据
df = bcr.load_dataset('covid19_tutorial')
#生成GIF图像
bcr.bar_chart_race(df, 'covid19_horiz.gif') #orientation='v'，垂直柱状图 # sort='asc' # n_bars=6
#生成MP4
bcr.bar_chart_race(df, 'covid19_horiz.MP4')
#设置数值的最大值，固定数值轴
fixed_maxbcr.bar_chart_race(df, 'covid19_horiz.gif', fixed_max=True)
#读取数据
df = pd.read_csv('data.csv')
#格式处理，需要把日期date转换成索引，不能作为单独一列
df = df.set_index(keys='date')
# 作者也提供了两个处理数据的函数
bcr.prepare_wide_data
bcr.prepare_long_data
```

