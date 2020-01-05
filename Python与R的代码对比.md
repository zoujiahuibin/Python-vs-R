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



## 集合运算

python中使用set（集合）的相关方法也可以判断集合关系：

1. s1.isdisjoint(s2)    #若s1, s2之间没有相同元素返回True， 否则返回false

2. s1.issubset(s2)      #判断s1是不是s2的子集

3. s1.issuperset(s2)   #判断s1是不是s2的超集

4. s1.union(s2,...)        #返回s1, s2,....的并集

5. s1.intersection(s2,...)  #返回交集

6. s1.difference(s2,...)      #返回差集

7. s1.symmetric_difference(s2)    #返回对称差集
