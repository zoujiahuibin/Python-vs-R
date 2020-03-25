# Python与R的代码对比

# 向量矩阵的定义

python中vector, array, matrix对象的定义方式：

```python
import numpy as np
#定义vector
a=np.array([1,2,3])
a
Out[151]: array([1, 2, 3])
#定义array
A=np.array([[1,2,3],[5,6,9]])
A
Out[152]: 
array([[1, 2, 3],
       [5, 6, 9]])
#定义matrix
A_mat=np.mat("1,2,3;5,6,9")
A_mat
Out[153]: 
matrix([[1, 2, 3],
        [5, 6, 9]])
#或者
A_mat=np.mat(A)
```

R语言中vector，matrix的定义：

```R
#定义vector
a=c(1,2,3)
> a
[1] 1 2 3
#定义matrix
A=matrix(c(1,2,3,5,6,9),nrow=2,byrow=T)
> A
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    5    6    9
```



## 向量的索引问题

Python中只有tuple·、list、array(numpy),其中对tuple和list的索引==只能用整数和切片==，唯有array的索引选取方式多样可以和R中的vector媲美。

**特别要注意**: 

- python中的索引从0开始，R从1开始，python中的切片a:b表示[a,b）而R中表示[a,b].

- python中数组中的==元素是每一个行向量==，即：array[0]选取的是第1个行向量，array\[0\]\[0\]选取的是第1行第1列的元素；而R中X[1]（X是矩阵）选取的是第1个元素（列向拉伸排序）。




1. 利用切片作为索引值

   ```python
   #python
   A=np.arange(1,26).reshape(5,5)
   A
   Out[171]: 
   array([[ 1,  2,  3,  4,  5],
          [ 6,  7,  8,  9, 10],
          [11, 12, 13, 14, 15],
          [16, 17, 18, 19, 20],
          [21, 22, 23, 24, 25]])
   
   A[0:6:2,0:6:2]
   Out[172]: 
   array([[ 1,  3,  5],
          [11, 13, 15],
          [21, 23, 25]])
   ```

python中还可以定义切片对象：

```python
import numpy as np
myslice=slice(0,10,2)
A=np.arange(20)
A[myslice]
Out[159]: array([0, 2, 4, 6, 8])
```




```R
#R
> A=matrix(1:25,nrow=5,byrow=T)
> A
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    6    7    8    9   10
[3,]   11   12   13   14   15
[4,]   16   17   18   19   20
[5,]   21   22   23   24   25
> A[seq(1,5,2),seq(1,5,2)]
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]   11   13   15
[3,]   21   23   25
```

2. 利用bool值作为索引值

```python
#python
A=np.arange(1,26).reshape(5,5)
A
index=[True,True,True,False,False]
A[index]
Out[182]: 
array([[ 1,  2,  3,  4,  5],
       [ 6,  7,  8,  9, 10],
       [11, 12, 13, 14, 15]])

A[index,index]
Out[183]: array([ 1,  7, 13])
    
A[index][:,index]
Out[184]: 
array([[ 1,  2,  3],
       [ 6,  7,  8],
       [11, 12, 13]])
```



```R
#R
A=matrix(1:25,nrow=5,byrow=T)
index=c(T,T,T,F,F)
> A[index]
 [1]  1  6 11  2  7 12  3  8 13  4  9 14  5 10 15

> A[index,]
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    6    7    8    9   10
[3,]   11   12   13   14   15

> A[index,index]
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    6    7    8
[3,]   11   12   13
```

从切片的索引引用中，可以看出python中array的基本元素是每一个行向量；而R的matrix中的基本元素就是每个数值。

3. 利用向量作为索引值

```python
#python
A=np.arange(1,26).reshape(5,5)
A
index=[1,3,2]
A[index]
array([[ 6,  7,  8,  9, 10],
       [16, 17, 18, 19, 20],
       [11, 12, 13, 14, 15]])

A[index,index]
Out[187]: array([ 7, 19, 13])
    
A[index][:,index]
Out[188]: 
array([[ 7,  9,  8],
       [17, 19, 18],
       [12, 14, 13]])

#注意对list是没有这样的操作的，例如：
mylist=list(range(10))
mylist
Out[195]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    
mylist[index]
TypeError: list indices must be integers or slices, not list
```



```R
#R
A=matrix(1:25,nrow=5,byrow=T)
index=c(1,3,2)
> A[index,]
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]   11   12   13   14   15
[3,]    6    7    8    9   10

> A[index,index]
     [,1] [,2] [,3]
[1,]    1    3    2
[2,]   11   13   12
[3,]    6    8    7
```

## 向量的组合

python在进行向量和矩阵组合时需要用，np.hstack((vec1,vec2,...)), np.vstack((vec1,vec2,...))

```python
#python
import numpy as np
vec=np.arange(5)
vec2=np.hstack((2,vec))
>>>vec
array([0, 1, 2, 3, 4])
>>>vec2
array([2, 0, 1, 2, 3, 4])

```



```R
#R
vec=seq(0,4)
vec2=c(2,vec)
> vec
[1] 0 1 2 3 4
> vec2
[1] 2 0 1 2 3 4
```



## 集合运算

python中使用set（集合）的相关方法也可以判断集合关系：

1. s1.isdisjoint(s2)    #若s1, s2之间没有相同元素返回True， 否则返回false
2. s1.issubset(s2)      #判断s1是不是s2的子集
3. s1.issuperset(s2)   #判断s1是不是s2的超集
4. s1.union(s2,...)        #返回s1, s2,....的并集
5. s1.intersection(s2,...)  #返回交集
6. s1.difference(s2,...)      #返回差集
7. s1.symmetric_difference(s2)    #返回对称差集

# 删除变量

```python
#python
# 删除全部用户自定义变量
import re
for x in dir():
    if not re.match('^__',x) and x!="re":
        exec(" ".join(("del",x)))
```



```R
#R,删除全部用户自定义变量
rm(ls())
```



# 同时做多根不同类别的曲线

## python中利用pandas+matplotlib

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
X=np.mat(range(1,21)).reshape(4,5).T
#python中矩阵的排列按行
df=pd.DataFrame(X,index=range(1,6),columns=("a","b","c","d"))
df.plot(style ='--', alpha=0.5, subplots = False,layout=(2, 3),figsize=(8,6))
plt.show()
```

<img src="E:\sourcetree\python-vs-R\image-20200207130450435.png" alt="image-20200207130450435" style="zoom:50%;" />

## R中利用graphics包

```R
#利用graphics
library(graphics)
x=1:5
y=matrix(1:20,ncol = 4,nrow=5)
#y是数据框也可以
matplot(x, y, type = "b", lty = 1:4, lwd = 1, pch = c('a','b','c','d'),
        col = 1:6,xlab = 'xxx', ylab = 'yy')
#每一列就是一根曲线，同类函数还有
matpoints(x, y+1, type = "p", lty = 1:5, lwd = 1, pch = NULL,
          col = 1:6, xlab = 'xxx', ylab = 'yy')
matlines (x, y-1, type = "l", lty = 1:5, lwd = 1, pch = NULL,
          col = 1:6, xlab = 'xxx', ylab = 'yy')
```

结果如下：

<img src="E:\sourcetree\python-vs-R\image-20200207114240707.png" alt="image-20200207114240707" style="zoom:50%;" />

## R中利用ggplot包

```R
#利用ggpolot
library(ggplot2)
library(reshape2)
x=1:5
y=data.frame(matrix(1:20,ncol = 4,nrow=5))
names(y)=c("a","b","c","d")
y=cbind(y,x)

mydata=melt(y,id.vars ='x' ,variable.name = "var",value.name = "value")

ggplot(data=mydata,aes(x=x,y=value))+geom_line(aes(col=var))+
  geom_point(aes(pch=var))+theme_test()
```

<img src="E:\sourcetree\python-vs-R\image-20200207114858418.png" style="zoom:50%;" />

# 数据框

## 获取列的名字

```python
#python
import pandas as pd

df.columns
```

```R
#R
names(df)
```



## 给数据框添加新列，删除列

```python
import pandas as pd
b=pd.DataFrame({'x':[1,2,5,4]})
b
Out[62]: 
   x
0  1
1  2
2  5
3  4
# 添加新列
b['y']=10
b
Out[64]: 
   x   y
0  1  10
1  2  10
2  5  10
3  4  10

b['x']=[8,5,6,2]
b
Out[79]: 
    y  x
0  10  8
1  10  5
2  10  6
3  10  2

#进行部分赋值
b.iloc[0:2,0]=[1,9]
b
Out[37]: 
    x  y
0   1  1
1   9  2
2  10  5
3  10  4


#删除列
del b['x']
b
Out[71]: 
    y
0  10
1  10
2  10
3  10
```

# 对于行或列的处理

```python
#python
import pandas as pd
df=pd.DataFrame({'x':[1,2,3],'y':[1,5,9]})
#默认对变量进行处理，等价于df.sum(axis=0)（即对纵向方向进行加和）
df.sum()
# x     6
# y    15
# dtype: int64

df.sum(axis=1)#对横向方向进行加和
# 0     2
# 1     7
# 2    12
# dtype: int64



import numpy as np
ar=np.array([[1,2,3],[4,5,6]])
#进行全求和
ar.sum()
# 21

#对纵向进行操作
ar.sum(axis=0)
# ar.sum(axis=0)
# array([5, 7, 9])

#对横向进行操作
ar.sum(axis=1)
# array([6, 15])
```

```r
#R

df=data.frame(x=1:3,y=c(1,5,9))
#对数据全求和
sum(df)
# 21

rowSums(df)
# [1]  2  7 12

colSums(df)
#x  y 
# 6 15 

#利用purrr::map_dfr输出结果还是一个dataframe
purrr::map_dfr(df,sum)
# A tibble: 1 x 2
# x     y
# <int> <dbl>
#   1     6    15
```