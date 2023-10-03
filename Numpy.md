## Numpy

### numpy对象

NumPy 最重要的一个特点是其 N 维数组对象 ndarray，它是一系列同类型数据的集合，以 0 下标为开始进行集合中元素的索引。

ndarray 内部由以下内容组成：

- 一个指向数据（内存或内存映射文件中的一块数据）的指针。
- 数据类型或 dtype，描述在数组中的固定大小值的格子。
- 一个表示数组形状（shape）的元组，表示各维度大小的元组。
- 一个跨度元组（stride），其中的整数指的是为了前进到当前维度下一个元素需要"跨过"的字节数。

创建一个 ndarray 只需调用 NumPy 的 array 函数即可：

```python
import numpy as np
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)

# 多于一个维度   
a = np.array([[1,  2],  [3,  4]])  
print (a)
##[[1  2] 
##[3  4]]

# 最小维度  
a = np.array([1, 2, 3, 4, 5], ndmin =  2)  
print (a)
##[[1 2 3 4 5]]

# dtype 参数  
a = np.array([1,  2,  3], dtype = complex)  
print (a)
##[1.+0.j 2.+0.j 3.+0.j]
```

### numpy数据类型

numpy 支持的数据类型比 Python 内置的类型要多很多，基本上可以和 C 语言的数据类型对应上，其中部分类型对应为 Python 内置的类型。

```python
import numpy as np
# 使用标量类型
dt = np.dtype(np.int32)
print(dt)
## int32
```

### numpy数组操作

```python
import numpy as np 
#创建数组
my_array = np.array([1, 2, 3, 4, 5]) 
print my_array.shape
## (5, )
my_new_array = np.zeros((5)) 
print my_new_array
## [0., 0., 0., 0., 0.]
#创建多维数组
my_2d_array = np.zeros((2, 3)) 
print my_2d_array
##[[0. 0. 0.]
##[0. 0. 0.]]
#提取
```

