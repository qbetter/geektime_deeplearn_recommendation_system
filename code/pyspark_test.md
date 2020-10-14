pyspark初次使用笔记心得

一，安装spark/pyspark/redis/执行官网测试代码
1，mac 下安装spark：https://blog.csdn.net/roguesir/article/details/78335034
""
因为只在本地执行数据，没有使用Hadoop的hdfs所以没有安装Hadoop，只安装了spark。
""
遇到的困难就是找不到spark的包，从这个地址可以下载：https://archive.apache.org/dist/spark/
在/usr/local/spark-2.4.7-bin-hadoop2.7/sbin下，执行
./stop-all.sh

可以关闭spark的运行。
2，mac下安装pyspark：https://blog.csdn.net/hil2000/article/details/90747665
Hadoop下载地址：
https://archive.apache.org/dist/hadoop/common/
3，执行spark教程中的测试代码：
进入到spark所在目录：/usr/local/spark-2.4.7-bin-hadoop2.7
然后执行
bin/spark-shell

进入到带有特殊字体写的“spark”的scala中了，然后就可以执行官网样例(https://spark.apache.org/docs/2.4.3/quick-start.html)中scala的代码了。

4，安装redis
redis下载、安装教程地址：http://www.redis.cn/download.html

二，代码部分
代码配置参考链接：https://blog.csdn.net/dxyna/article/details/79772343
配置成功执行的代码如下，在MacBook Pro本机上可以执行：
```cpp
import os
import sys
spark_name = os.environ.get('SPARK_HOME',None)
if not spark_name:
    raise ValueErrorError('spark环境没有配置好')
sys.path.insert(0,os.path.join(spark_name,'python'))
sys.path.insert(0,os.path.join(spark_name,'python/lib/py4j-0.10.7-src.zip'))
exec(open(os.path.join(spark_name,'./python/pyspark/shell.py')).read())

lines = sc.textFile("head_count_draw.py")
print(lines.collect())
print(lines.count())
print(type(lines))
```

执行spark mllib的代码：
```cpp
import os
import sys
spark_name = os.environ.get('SPARK_HOME',None)
if not spark_name:
    raise ValueErrorError('spark环境没有配置好')
sys.path.insert(0,os.path.join(spark_name,'python'))
sys.path.insert(0,os.path.join(spark_name,'python/lib/py4j-0.10.7-src.zip'))
exec(open(os.path.join(spark_name,'./python/pyspark/shell.py')).read())

from pyspark.ml.linalg import Vectors
from pyspark.ml.stat import Correlation

data = [(Vectors.sparse(4, [(0, 1.0), (3, -2.0)]),),
        (Vectors.dense([4.0, 5.0, 0.0, 3.0]),),
        (Vectors.dense([6.0, 7.0, 0.0, 8.0]),),
        (Vectors.sparse(4, [(0, 9.0), (3, 1.0)]),)]
df = spark.createDataFrame(data, ["features"])

r1 = Correlation.corr(df, "features").head()
print("Pearson correlation matrix:\n" + str(r1[0]))

r2 = Correlation.corr(df, "features", "spearman").head()
print("Spearman correlation matrix:\n" + str(r2[0]))
```

