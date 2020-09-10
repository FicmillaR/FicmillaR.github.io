# Pandas I/O 大坑

`pandas罪大滔天，搞到百姓怨声载道`

1. pd通过 [IO接口]( https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html ) 读写数据，并储存为dataframe格式

   读取:

   ```python
   a = pd.read_excel('testdata.xlsx', 'Sheet1',index_col = None, header = None, na_values=['NA'])
   print (a[0])
   ```

   结果:

   ```python
   0       117
   1       117
   2       117
   ...       ...
   5263    101
   5264    101
   Name: 0, Length: 5265, dtype: int64
   ```

   若：

   ```python
   print (a[0:1])
   ```

   结果:

   ```python
       0     1        2       3   4   5     6   7  
   0  117  0.66875  10.6  225.1  51  50  1114   0
   ```

   对于dataframe的行列选择和切片操作建议使用 `loc` 与 `iloc`

   ```python
   print (a.iloc[0:,0:2])
   ```

   结果：

   ```python
           0         1
   0     117  0.668750
   1     117  0.668056
   2     117  0.667361
     ...       ...
   5262  101  0.419444
   5263  101  0.418750
   5264  101  0.418056
   
   [5265 rows x 2 columns]
   ```

   

2. pandas.to_excel覆盖已有sheet

   解决方法:

   ```python
   with ExcelWriter('writingtest.xlsx',mode='a') as writer:
       pre.to_excel(writer,index=False,header=False)
   ```

   可以在新sheet插入but无法改写原有sheet

   