### 用 NumPy 快速处理数据
#### 为什么需要一个第三方的数据结构？
1. 标准的 Python 中用 list 保存数组的数值，由于列表的元素可以是任意对象，所以 list 中保存的是对象的指针。如保存 [0,1,2] 需要 3 个指针和 3 个对象，浪费了内存和计算时间。

2. list 的元素是分散存储，NumPy 存储在一个均匀连续的内存块中，计算时遍历元素不需要对内存地址进行查找，节省计算资源。

3. 内存访问模式中，缓存会直接把字节块从 RAM 加载到 CPU 寄存器中，因数据连续存储在内存中，NumPy 直接利用现代 CPU 的矢量化指令计算，加载寄存器中多个浮点数。

4. NumPy 中的矩阵计算可以采用多线程的方式。

#### 规则
避免采用隐式拷贝，而是采用就地操作的方式。
例：用 x \*= 2 而不是 y = x * 2

#### ndarray 对象
N-dimensional array object，解决了多维数组问题。
* NumPy 数组中，维数称为秩(rank)，一维数组的秩为 1，二维数组的秩为 2。
* Numpy 中，每一个线性的数组称为一个轴(axes)，秩就是描述轴的数量。
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/axes_img.png)

* **创建数组**
  * 代码
  ```Python
  import numpy as np
  a = np.array([1, 2, 3])
  b = np.array([[1, 2, 3],
               [4, 5 ,6],
               [7, 8, 9]])
  b[1,1] = 10
  print(a.shape)
  print(b.shape)
  print(a.dtype)
  print(b)
  ```
  * 结果
  ```
  >> (3,)
  (3, 3)
  int32
  [[ 1  2  3]
   [ 4 10  6]
   [ 7  8  9]]
  ```
* **结构数组**
  * 类似于 C 语言中的 struct 定义结构类型，结构中的字段占据连续的内存空间，每个结构体占用的内存大小相同。
  * 例：用 dtype 定义结构类型。统计一个班级学生的姓名、年龄、语数英成绩。
  ```Python
  import numpy as np
  persontype = np.dtype({
      "names": ["name", "age", "chinese", "math", "english"],
      "formats": ["S32", "i", "f", "f", "f"]})
  peoples = np.array([("ZhangFei", 32, 75, 100, 90), ("GuangYu", 24, 85, 96, 88.5),
                      ("ZhaoYun", 28, 85, 92, 96.5), ("HuangZhong", 29, 65, 85, 100)],
                    dtype=persontype)
  ages = peoples[:]["age"]
  chineses = peoples[:]["chinese"]
  maths = peoples[:]["math"]
  englishs = peoples[:]["english"]
  print(np.mean(ages))
  print(np.mean(chineses))
  print(np.mean(maths))
  print(np.mean(englishs))
  ```
* **ufunc运算**
universal function，对数组中每个元素进行函数操作。
  * 连续数组的创建，[1,3,5,7,9]
    ```
    x1 = np.arange(1, 11, 2)
    x2 = np.linspace(1, 9, 5)
    ```
    * arange()：类似 range()，指定初始值、终值、步长。
    * linspace()：linear space，线性等分向量。指定初始值、终值、元素个数创建等差数列的一维数组。
  * 算数运算
    * add()：加。
    * subtract()：减。
    * multiply()：乘。
    * divide()：除。
    * power()：n次方。
    * remainder()/mod()：求余。
  * 统计函数
    * amax()：最大值
    * amin()：最小值
      * 例
      ```Python
      import numpy as np
      a = np.array([[1,2,3],
                     [4,5,6],
                     [7,8,9]])
      print(np.amin(a))
      print(np.amin(a, 0))
      print(np.amin(a, 1))
      print(np.amax(a))
      print(np.amax(a, 0))
      print(np.amax(a, 1))
      ```
      * 结果
      ```
      >> 1
      [1 2 3]
      [1 4 7]
      9
      [7 8 9]
      [3 6 9]
      ```
      * 计算数组中的元素沿指定轴的最大/小值。对于一个二维数组 a，amax(a)、amin(a) 指全部元素的最大/小值。而 amin(a, 0)是沿着 axis = 0 轴的最小值，把元素看成 [1,4,7]、[2,5,8]、[3,6,9]，所以最小值为 [1,2,3]。同理，axis = 1，把元素看成 [1,2,3]、[4,5,6]、[7,8,9]，最小值为 [1,2,3]。

    * ptp()：最大值与最小值之差
      * 例
      ```Python
      import numpy as np      
      a = np.array([[1,2,3], [4,5,6], [7,8,9]])
      print(np.ptp(a))
      print(np.ptp(a, 0))
      np.ptp(a, 1)
      ```
      * 结果
      ```
      >> 8
      [6 6 6]
      [2, 2, 2]
      ```
      * 同样可以指定沿轴计算。
    * percentile()：统计数组的百分位数
      * 例
      ```Python
      import numpy as np
      a = np.array([[1,2,3], [4,5,6], [7,8,9]])
      print(np.percentile(a, 50))
      print(np.percentile(a, 50, axis=0))
      print(np.percentile(a, 50, axis=1))
      ```

      * 结果
      ```
      >> 5.0
      [4. 5. 6.]
      [2. 5. 8.]
      ```
      * 同样求得在 axis=0 和 axis=1 两个轴上的 p% 的百分位数。
    * median()：中位数
    * mean()：平均数
    * average()：加权平均值
      * 例
      ```
      a = np.array([1,2,3,4])
      wts = np.array([1,2,3,4])
      np.average(a)
      np.average(a, weights=wts)
      ```
      * 结果
      ```
      >> 2.5
      >> 3.0
      ```
      * 默认情况下每个元素权重相同，指定权重数组后，计算加权平均为：(1*1 + 2*2 + 3*3 + 4*4)/(1 + 2 + 3 + 4) = 3.0
    * std()：标准差，方差的算数平方根。
    * var()：方差：每个数值与平均值之差的平方求和的平均值，代表一组数据离平均值的离散程度。
* **NumPy 排序**
  * sort(a, axis=-1, kind='quicksort', order=None)
    * kind：默认情况下快速排序，可以指定 quicksort(快排)、mergesort(合并排序)、heapsort(堆排序)。
    * axis：默认 -1，沿着数组的最后一个轴排序；指定为 None 按扁平化的方式作为一个向量排序。
    * order：对于结构化的数组，可以指定某个字段进行排序。
    * 例
    ```
    a = np.array([[4,3,2],[2,4,1]])
    np.sort(a)
    np.sort(a, axis=None)
    np.sort(a, axis=0)
    np.sort(a, axis=1)
    ```
    * 结果
    ```
    [[2 3 4]
     [1 2 4]]
    [1 2 2 3 4 4]
    [[2 3 1]
     [4 4 2]]
    [[2 3 4]
     [1 2 4]]
    ```
