# 数据处理
1. remove part of string in a column
`AttributeError: 'float' object has no attribute 'lstrip'`
solution:
use: **replace**
`temp_dataframe['PPI'].replace('PPI/','',regex=True,inplace=True)`
or **string.replace**
`temp_dataframe['PPI'].str.replace('PPI/','')`
[link](https://stackoverflow.com/questions/37919479/removing-characters-from-a-string-in-pandas)
2. 获取列名: 遍历columns[link](https://www.geeksforgeeks.org/how-to-get-column-names-in-pandas-dataframe/)
3. 将Nan值转为其他值: `df["A"] = df["A"].fillna(0)` [Link](https://www.kite.com/python/answers/how-to-replace-nan-values-with-zeros-in-a-column-of-a-pandas-dataframe-in-python#:~:text=Use%20pandas.,name%20from%20the%20DataFrame%20df%20.)
4. 将类别数据转化：[Convert categorical data in pandas dataframe](https://stackoverflow.com/questions/32011359/convert-categorical-data-in-pandas-dataframe)
5.  pandas.Series.str.contains
6. [find Nan](https://datatofish.com/check-nan-pandas-dataframe/)
7. [drop rows where is Nan](https://www.kite.com/python/answers/how-to-drop-empty-rows-from-a-pandas-dataframe-in-python)
8. [Pandas堆叠两个DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html)
9. `DataFrameGroupBy.nunique(_dropna=True_)` 
10. `DataFrame.reset_index()` 重新设置索引，drop = True 时删除原索引
11. `DataFrame.set_index()` 将某个列设置为索引
12. `pandas.DataFrame.rename()` 改变label 
13. **`zip()`** 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。可以使用 list() 转换来输出。如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。
14. `DataFrame.merge()`左右拼接
