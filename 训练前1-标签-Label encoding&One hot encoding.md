# Label encoding/One hot encoding 的几种方法

## 1. np.eye/keras.utils 快速生成 one_hot 编码

**只对针对 int or boolen类型的array/list**

函数：`np.eye(N, M=None, k=0, dtype=float, order=‘C’)`
功能说明：用来返回一个2维的对角数组
参数：

* `N`：用来控制输出二维数组的行数
* `M`：用来控制输出二维数组的列数，如果M为None，则M等于N
* `k`：主对角线的index，默认是0，如果k为正数，则对角线往上移动，如果k为复数，则对角线往下移动


```python
import numpy as np
from keras.utils import to_categorical

print(np.eye(5))
print(np.eye(5,5,1))
>>>>>>>>>>>>>>>>>>>>>>>>
[[1. 0. 0. 0. 0.]
 [0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]
 [0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 1.]]
[[0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]
 [0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 1.]
 [0. 0. 0. 0. 0.]]

labels = [1,2,3,4]

one_hot = to_categorical(labels)
eye_hot = np.eye(5)[labels]

print(one_hot,eye_hot)
>>>>>>>>>>>>>>>>>>>>>>>>
[[0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]
 [0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 1.]] 
[[0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]
 [0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 1.]]

```


* 对于multi_label的情况

```python
mul_labels = [[1,1],
              [2,1],
              [2,2]]
mul_eye = []
for x in mul_labels:
    mul_eye.append(np.eye(3)[x])
mul_hot = to_categorical(mul_labels)
print(mul_eye)
print(mul_hot)
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
[array([[0., 1., 0.],
       [0., 1., 0.]]), 
 array([[0., 0., 1.],
       [0., 1., 0.]]), 
 array([[0., 0., 1.],
       [0., 0., 1.]])]

[[[0. 1. 0.]
  [0. 1. 0.]]

 [[0. 0. 1.]
  [0. 1. 0.]]

 [[0. 0. 1.]
  [0. 0. 1.]]]


print(mul_eye[0].dtype)
print(mul_hot[0].dtype)
print(mul_hot.dtype)
>>>>>>>>>>>>>>>>
float64
float32
float32
```



**总结**

`np.eye()` 方法：

* 默认dtype为float64
* 可以自由调整矩阵形状
* 对于multi_labels还需要for_loop并收集output

`keras.utils.to_categorical()` 方法：

* 默认dtype为float32
* 全自动，包装完整，一键multi_labels



**两种方法都可以使用dtype硬转化格式**

```python
for x in mul_labels:
    mul_eye.append(np.eye(3,dtype='int8')[x])

mul_hot = to_categorical(mul_labels,dtype='int8')
```





## 2. sklearn.preprocessing 方法

**是啥？**

Label encoding : 做类别->数字的maping，不增加栏位
One hot encoding : 独热向量，增加栏位

![img](D:/Dropbox/学习-CVonly.assets/1_ZFCX83XaMNzOAXRxAcvMJw.png)

**sklearn.preprocessing.LabelEncoder() class**

使用0到n_classes-1之间的值编码/解码目标标签。

用于非数值型标签的编码转换成数值标签（只要它们是 **可哈希并且可比较的**）:

```python
from sklearn.preprocessing import LabelEncoder

labels = ['a','b','a','c','d']
le = LabelEncoder()
le.fit(labels)
print(le.classes_)
a = le.transform(labels)
print(a)
inv_labels = le.inverse_transform(a)
print(inv_labels)
>>>>>>>>>>>>>>>>>>>>>>>>>
['a' 'b' 'c' 'd']
[0 1 0 2 3]
['a' 'b' 'a' 'c' 'd']
```

方法：

| [`fit`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.fit)(self, y) | Fit标签编码器                         |
| ------------------------------------------------------------ | ------------------------------------- |
| [`fit_transform`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.fit_transform)(self, y) | Fit标签编码器并返回经过编码的标签     |
| [`get_params`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.get_params)(self[, deep]) | Get parameters for this estimator.    |
| [`inverse_transform`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.inverse_transform)(self, y) | 将标签解码为原始编码                  |
| [`set_params`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.set_params)(self, \*\*params) | Set the parameters of this estimator. |
| [`transform`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.transform)(self, y) | 将标签转化                            |

**LabelEncoder 将参数存储在classes_属性中**

```python
labels = ['a','b','a','c','d']

le = LabelEncoder()
value = le.fit_transform(labels)
key = le.classes_

dic = dict(zip(key,value))
print(dic)
>>>>>>>>>>>>>>>>>>>>>>>>>>
{'a': 0, 'b': 1, 'c': 0, 'd': 2}
```

**其他**

**1. OrdinalEncoder() **

> *class* `sklearn.preprocessing.``OrdinalEncoder`(***, *categories='auto'*, *dtype=<class 'numpy.float64'>*)

* 标签数>=2
* 标签规整
* **接受不同输入类型**

```python
labels = [['a','a',2],
          ['b','a',1],
          ['c','b',1]]

encoder = OrdinalEncoder()
encoder.fit(labels)
print(encoder.categories_)
result = encoder.transform(labels)
inv_result = encoder.inverse_transform(result)
print(result,'\n',
      result.dtype,'\n',
      inv_result,'\n',
      inv_result.dtype)
'''
[array(['a', 'b', 'c'], dtype=object), array(['a', 'b'], dtype=object), array([1, 2], dtype=object)]
[[0. 0. 1.]
 [1. 0. 0.]
 [2. 1. 0.]] 
 float64 
 [['a' 'a' 2]
 ['b' 'a' 1]
 ['c' 'b' 1]] 
 object
'''

labels = [['a','a','b'],
          ['b','a','a'],
          ['c','b']]
'''
ValueError: Expected 2D array, got 1D array instead:
array=[list(['a', 'a', 'b']) list(['b', 'a', 'a']) list(['c', 'b'])].
Reshape your data either using array.reshape(-1, 1) if your data has a single feature or array.reshape(1, -1) if it contains a single sample.

```



**2. OneHotEncoder()** : one_hot版的OrdinalEncoder()

> *class* `sklearn.preprocessing.``OneHotEncoder`(***, *categories='auto'*, *drop=None*, *sparse=True*, *dtype=<class 'numpy.float64'>*, *handle_unknown='error'*)

* 标签数>=2
* 标签规整
* 需要toarray() one_hot化
* **接受不同数据类型**

```python
labels = [['a','a','b'],
          ['b','a','a'],
          ['c','b','a']]

encoder = OneHotEncoder(handle_unknown='ignore',dtype= 'int8')
encoder.fit(labels)
print(encoder.categories_)
og_result = encoder.transform(labels)
result = encoder.transform(labels).toarray()
inv_result = encoder.inverse_transform(result)
print(og_result,'\n',
      result,'\n',
      result.dtype,'\n',
      inv_result,'\n',
      inv_result.dtype)
'''
[array(['a', 'b', 'c'], dtype=object), array(['a', 'b'], dtype=object), array(['a', 'b'], dtype=object)]
  (0, 0)        1
  (0, 3)        1
  (0, 6)        1
  (1, 1)        1
  (1, 3)        1
  (1, 5)        1
  (2, 2)        1
  (2, 4)        1
  (2, 5)        1 
 [[1 0 0 1 0 0 1]
 [0 1 0 1 0 1 0]
 [0 0 1 0 1 1 0]] 
 int8 
 [['a' 'a' 'b']
 ['b' 'a' 'a']
 ['c' 'b' 'a']] 
 object
'''
```



**3. MultiLabelBinarizer**

* Onehot化
* 标签数随意
* 不需要标签规整
* **不接受不同数据类型**

> *class* `sklearn.preprocessing.``MultiLabelBinarizer`(***, *classes=None*, *sparse_output=False*)

```python
labels = [['a','dog','red'],
          ['b','cat','yellow'],
          ['a','cat']]

encoder = MultiLabelBinarizer()
mlb = encoder.fit_transform(labels)
print(encoder.classes_)
print(mlb)
'''
['a' 'b' 'cat' 'dog' 'red' 'yellow']
[[1 0 0 1 1 0]
 [0 1 1 0 0 1]
 [1 0 1 0 0 0]]
 '''
```

一个常见的错误

只有一个[]的

```python
labels = ['dog','cat','fish']

'''
['a' 'c' 'd' 'f' 'g' 'h' 'i' 'o' 's' 't']
[[0 0 1 0 1 0 0 1 0 0]
 [1 1 0 0 0 0 0 0 0 1]
 [0 0 0 1 0 1 1 0 1 0]]
'''
```

无重复，无顺序：

```python
labels = [['a','b'],
          ['b','a']]
'''
['a' 'b']
[[1 1]
 [1 1]]
 '''

labels = [['a','b'],
          ['b','c','d'],
          ['a','e']]
encoder = MultiLabelBinarizer()
mlb = encoder.fit_transform(labels)
print(encoder.classes_)

new_label = [['a','b','e','a','b']]
print(encoder.transform(new_label))
'''
['a' 'b' 'c' 'd' 'e']
[[1 1 0 0 1]]
'''
```



4. [`LabelBinarizer`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelBinarizer.html#sklearn.preprocessing.LabelBinarizer) 是一个用来从多类别列表创建标签矩阵的工具类:

### 使用提议

1. 对于cifar-10，使用LabelEncoder编码，使用LabelBinarizer one_hot编码
2. 对于multi_lamp，使用OrdinalEncoder编码，使用OneHotEncoder one_hot编码
3. 对于从多个小label池中生成 **无重复项** 大label池，从大label池pick **不定数/不重复/无顺序** 的 **one_hot** label，使用MultiLabelBinarizer编码
   eg. fashion 问题
4. 输出都是nd.array





## 3. Pandas.get_dummies

> `pandas.``get_dummies`**(***data***,** *prefix=None***,** *prefix_sep='_'***,** *dummy_na=False***,** *columns=None***,** *sparse=False***,** *drop_first=False***,** ***dtype=None*****)** **→ 'DataFrame'**

类似于sklearn.preprocessing中的OneHotEncoder方法

* 基本上会对label one_hot
* 如果label为int或float则保留
* 输出dataframe👉`to.numpy()`方法转化为numpy对象
* 依旧需要对齐，可用`np.nan` 填充
* 不记忆label encoder map

```python
df = pd.DataFrame({'A': ['a', 'b', 'a'], 
                   'B': ['b', 'a', 'c'],
                   'C': ['1', '2', '3']})
print(df)
df2 = pd.get_dummies(df)
df3 = df2.to_numpy()
print(df2)
print(df3)
'''
   A  B  C
0  a  b  1
1  b  a  2
2  a  c  3
   A_a  A_b  B_a  B_b  B_c  C_1  C_2  C_3
0    1    0    0    1    0    1    0    0
1    0    1    1    0    0    0    1    0
2    1    0    0    0    1    0    0    1
[[1 0 0 1 0 1 0 0]
 [0 1 1 0 0 0 1 0]
 [1 0 0 0 1 0 0 1]]
'''
```

## 4. to_categorical

```python
keras.utils.to_categorical(y, num_classes=None, dtype='float32')
```

将类向量（整数）转换为二进制类矩阵。

* labels必须是int
* 一键one_hot，**并且multi_label中每个label单独one_hot**

```python
labels = [[1,2,2],
          [1,1,1],
          [2,1,0]]

a = to_categorical(labels,dtype='int8')
print(a)
'''
[[[0 1 0]
  [0 0 1]
  [0 0 1]]

 [[0 1 0]
  [0 1 0]
  [0 1 0]]

 [[0 0 1]
  [0 1 0]
  [1 0 0]]]
'''
```



