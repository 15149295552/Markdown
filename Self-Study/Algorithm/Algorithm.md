# 1 时间复杂度

## 1.1 时间复杂度概念

* 时间复杂度：用来评估算法运算效率的一个式子
  * 时间复杂度记为$$O({\log_2 n})$$或$$O(\log n)$$
  * 当算法过程出现循环折半的时候，复杂度式子中会出现$$\log n$$
  
* 时间复杂度是用来估计算法运行时间的一个式子（单位）

* 一般来说，时间复杂度高的算法比复杂度低的算法慢

* 常见的时间复杂度（按效率排序）
  * $$
    O(1)<O(\log n)<O(n)<O(n \log n)<O\left(n^2\right)<O\left(n^2 \log n\right)<O\left(n^3\right)
    $$
  
* 复杂问题的时间复杂度
  * $$
    O(n!)\quad O(2^n)\quad O(n^n)
    $$

## 1.2 判断时间复杂度

* 快速判断时间算法复杂度（适用于绝大多数简单情况）
  * 确定问题规模n
  * 循环减半过程 –> $\log n$
  * k层关于n的循环 –> $n^k$
* 复杂情况：根据算法执行过程判断

## 1.3 空间复杂度

* 空间复杂度：用来评估算法内存占用大小的式子
* 空间复杂度的表示方式与时间复杂度完全一样
  * 算法使用了几个变量：$O(1)$
  * 算法使用了长度为n的一维列表：$O(n)$                                                                                    
  * 算法使用了m行n列的二维列表：$O(mn)$
* 空间换时间

# 2 递归

## 2.1 特点

* 调用自身
* 结束条件

​         ![image-20221101191913754](Algorithm.assets/image-20221101191913754.png)          <img src="Algorithm.assets/image-20221101191951948.png" alt="image-20221101191951948" style="zoom:67%;" /> 



​         ![image-20221101192051502](Algorithm.assets/image-20221101192051502.png)           <img src="Algorithm.assets/image-20221101192116906.png" alt="image-20221101192116906" style="zoom:67%;" />

* func3结果为 3 2 1
* func4结果为 1 2 3

## 2.2 递归实例：汉诺塔问题

```python
def hanoi(n, a, b, c):
    if n > 0:
        hanoi(n - 1, a, c, b)
        print("moving from %s to %s" % (a, c))
        hanoi(n - 1, b, a, c)


hanoi(3, 'A', 'B', 'C')
# 结果：
# moving from A to C
# moving from A to B
# moving from C to B
# moving from A to C
# moving from B to A
# moving from B to C
# moving from A to C
```

* 汉诺塔移动次数的递推式：$h(x)=2h(x-1)+1$
* $h(64)=18446744073709551615$
* 假设婆罗门每秒钟搬一个盘子，则总共需要5800亿年

# 3 列表查找

## 3.1 查找概念

* 查找：在一些数据元素中，通过一定的方法找出与给定关键字相同的数据元素的过程
* 列表查找（线性表查找）：从列表中查找指定元素
  * 输入：列表、待查找元素
  * 输出：元素下标（未找到元素时一般返回None或-1）
* 内置列表查找函数：index()

## 3.2 顺序查找

* 顺序查找：也叫线性查找，从列表第一个元素开始，顺序进行搜索，直到找到元素或搜索到列表最后一个元素为止。
  ```python
  def linear_search(li, val):
      for ind, v in enumerate(li):
          if v == val:
              return ind
      return None
  ```

* 时间复杂度：$O(n)$

## 3.3 二分查找

* 二分查找：又叫折半查找，从有序列表的初始候选区li[0:n]开始，通过对待查找的值与候选区中间值的比较，可以使候选区减少一半。
  ```python
  def binary_search(li, val):
      left = 0
      right = len(li) - 1
      while left <= right:  # 候选区有值
          mid = (left + right) // 2
          if li[mid] == val:
              return mid
          elif li[mid] > val:  # 待查找的值在mid左侧
              right = mid - 1
          else:  # li[mid] < val 待查找的值mid右侧
              left = mid + 1
      return None
  
  
  li = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  print(binary_search(li, 3))
  ```

  
