### Python 科学计算：Pandas
#### 数据结构：Series 和 DataFrame
1. Series：定长的字典序列
  * 定长：在存储的时候，相当于两个 ndarray。
  * 两个基本属性：index 和 values，index 默认是 0,1,2,...递增的整数序列，也可以自己指定。
  * 例
  ```Python
  import pandas as pd
  x1 = pd.Series([1,2,3,4])
  x2 = pd.Series(index=["a", "b", "c", "d"],data=[1,2,3,4])
  d = {"a": 1, "b": 2, "c": 3, "d":4}
  x3 = pd.Series(d)
  ```
2. DataFrame：类似数据库表
  * 包括了行索引和列索引。
  * 例
  ```Python
  import pandas as pd
  data = {"Chinese":[66, 95, 93, 90, 80], "Math":[30, 98, 96, 77, 90], "English":[65, 85, 92, 88, 90]}
  df1 = pd.DataFrame(data)
  df2 = pd.DataFrame(data, index=["ZhangFei", "GuanYu", "ZhaoYun", "HuangZhong", "DianWei"],
                     columns=["Chinese", "Math", "English"])
  ```
  * index 指定行索引，columns 指定列索引。

#### 数据的导入和输出
允许从 xlsx、csv 等文件中导入数据，也可以输出到 xlsx、csv 等文件。
* 例
```Python
import pandas as pd
score = pd.DataFrame(pd.read_excel('data.xlsx'))
score.to_excel('data1.xlsx')
```

#### 数据清洗
* 数据
```
data = {'Chinese': [66, 95, 93, 90,80],'English': [65, 85, 92, 88, 90],'Math': [30, 98, 96, 77, 90]}
df2 = DataFrame(data, index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'], columns=['English', 'Math', 'Chinese'])
```
  1. drop()：删除 DataFrame 中不必要的列或行
  ```
  df = df2.drop(columns=["Chinese"])
  df = df2.drop(index=["ZhangFei"])
  ```
  2. rename()：重命列名
  ```
  df2.rename(columns={"Chinese":"YuWen", "Math":"ShuXue", "English":"YingYu"}, inplace=True)
  ```
  3. drop_duplicates()：去除重复行
  ```
  df2.drop_duplicates()
  ```
  4. 格式问题
    * astype()：更改数据格式
    ```
    df2["YuWen"].astype('str')
    df2["YuWen"].astype(np.int64)
    ```
    * strip()：删除数据间的空格
    ```
    # 删除左右两边空格
    df2['Chinese']=df2['Chinese'].map(str.strip)
    # 删除左边空格
    df2['Chinese']=df2['Chinese'].map(str.lstrip)
    # 删除右边空格
    df2['Chinese']=df2['Chinese'].map(str.rstrip)
    ```
    * 大小写转换
    ```
    # 全部大写
    df2.columns = df2.columns.str.upper()
    # 全部小写
    df2.columns = df2.columns.str.lower()
    # 首字母大写
    df2.columns = df2.columns.str.title()
    ```
    * 查找空值
      * ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/one_NaN.png)
      * df.isnull()：![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/isnull.png)
      * df.isnull().any()：![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/isnull_any.png)
      * df.fillna()：填补缺失值
  5. apply() 进行数据清洗
    * 对 Name 列数值进行大写转化
    ```
    df2["Name"] = df2["Name"].apply(str.upper)
    ```
    * 自定义函数，语文成绩翻倍
    ```
    def double_df_chinese(x):
        return 2*x
    df2["Chinese"] = (df2["Chinese"]).apply(double_df_chinese)
    ```

#### 数据统计
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/Pandas常用统计函数.jpg)

#### 数据表合并
* 数据
```
df1 = DataFrame({'name':['ZhangFei', 'GuanYu', 'a', 'b', 'c'], 'data1':range(5)})
df2 = DataFrame({'name':['ZhangFei', 'GuanYu', 'A', 'B', 'C'], 'data2':range(5)})
```
  1. 基于指定的列进行连接
  ```
  df3 = pd.merge(df1, df2, on="name")
  ```
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/按指定列合并.png)
  2. inner 内连接：merge 的默认情况，键的交集，本例中即为 name 列
  ```
  df3 = pd.merge(df1, df2, how="inner")
  ```
  3. left 左连接：以第一个 DataFrame 为主进行的连接，第二个作为补充
  ```
  df3 = pd.merge(df1, df2, how="left")
  ```
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/左连接.png)
  4. right 右连接：以第二个 DataFrame 为主进行的连接，第一个作为补充
  ```
  df3 = pd.merge(df1, df2, how="right")
  ```
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/右连接.png)
  5. outer 外连接：求并集
  ```
  df3 = pd.merge(df1, df2, how="outer")
  ```
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/外连接.png)
