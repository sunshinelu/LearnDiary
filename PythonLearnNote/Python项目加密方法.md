# Python 项目加密方法

> 参考链接
>
> Mac上把python源文件编译成so文件
> <https://blog.csdn.net/weixin_34392906/article/details/86001563>
>
> 使用cython将py文件编译成so文件
> <https://blog.csdn.net/marleylee/article/details/77718241>
>
> 我算是白学Python了，现在才知道原来Python是可以编译的
> <https://baijiahao.baidu.com/s?id=1618495304088415793&wfr=spider&for=pc>
>
> windows 下Python3.x生成pyd文件(python加密)
> <https://blog.csdn.net/Gavinmiaoc/article/details/84340736>
>
> python封装.so文件注意事项
> <https://blog.csdn.net/ustcjinggg/article/details/100514463>


将python代码编译成.so文件
<https://www.cnblogs.com/deeplearning2015/p/10026379.html>

python文件编译成so介绍 - 1.使用cython将py文件编译成so文件
<https://blog.csdn.net/linshenyuan1213/article/details/72677246>

python源代码加密
<https://blog.csdn.net/weixin_40744265/article/details/90032109>




## Mac上对源码进行加密

1. 创建 `setup.py` 文件。

```
# -*- coding: utf-8 -*-


import Cython.Build
import distutils.core
data_preprocess = Cython.Build.cythonize("/Users/sunlu/Downloads/pyd_test/data_preprocess.py")[0] #提取Extension对象
mysql_conn = Cython.Build.cythonize("/Users/sunlu/Downloads/pyd_test/mysql_conn.py")[0]
distutils.core.setup(
name = 'pyd的编译测试', #包名称
version = "1.0", #包版本号
ext_modules= [data_preprocess,mysql_conn], #被扩展的模块
author = "孙露", #作者
author_email='sunlu@sdas.org' #作者邮箱
)

```


2. 创建`mysql_conn.py` 文件。
 
```
# -*- coding: utf-8 -*-

import pandas as pd

# mysql
from sqlalchemy import create_engine


class Mysql(object):
    """
    mysql database read and write
    url['username']:username
    url['password']:password
    url['host']:host
    url['database']:database
    """

    def __init__(self, username, password, host, database):

        mysql_db = 'mysql+pymysql://' + str(username) + ':' + str(password) + '@' \
             + str(host) + '/' + str(database) + '?charset=utf8'
        self.sql_conn = create_engine(mysql_db, echo=False)

    def read_sql(self, temp_table):

        sql = "select * from %s" % temp_table
        sql_df = pd.read_sql(sql, self.sql_conn)
        print('read sql success: ' + sql)
        return "%s is null" % temp_table if sql_df.empty else sql_df

    def write_sql(self, new_table, df):
        sql_df = pd.DataFrame(df)
        sql_result = sql_df.to_sql(new_table, self.sql_conn, if_exists='replace', index=False)
        print("write to new_table success:" + new_table)
        return sql_result

    def drop_table(self, temp_table):
        print('drop table beginning ......')
        sql = "DROP TABLE %s" % temp_table
        self.sql_conn.execute(sql)
        return True



``` 
    
3. 创建 `data_preprocess.py` 文件。
    
```
# -*- coding: utf-8 -*-

import re
from math import isnan
import pandas as pd
from mysql_conn import *


# 数据预处理
def data_preprocess(input_sql, output_sql, temp_table, new_table, process, params):
    """
    :param temp_table: 数据表
    :param new_table: 结果表
    :param process:
    :param params:
    :return: df
    """
    preprocess_dict = {
        'data_null':
            {'save': nan_data_null,
             'drop': nan_dele_data,
             'mean_data': nan_mean_data,
             'mode_data': nan_mode_data,
             'constant': nan_fill_constant,
             'donothing': nan_donothing
             }
    }

    global data_prepro
    sql_df = input_sql.read_sql(temp_table)

    if process[0] in preprocess_dict.keys():
        data_prepro = preprocess_dict[process[0]][process[1]](sql_df, params)

    if isinstance(data_prepro, str):
        Code = -1  # 数据预处理成功，但结果为空
        return Code, data_prepro
    else:
        if output_sql.write_sql(new_table, data_prepro) is None:
            Code = 0
            data_prepro = 'write to %s success' % new_table
            return Code, data_prepro


# 保存空值
def nan_data_null(df, cols):
    """
    isnull output
    :return:
    """
    isnull = df[df[cols].isnull().values == True].drop_duplicates()

    return 'Null data is None' if isnull.empty else isnull


# 删除空值
def nan_dele_data(df, cols):
    # 过滤缺失数据
    drop_data = df.dropna(subset=cols)
    return 'All data is None' if drop_data.empty else drop_data


# 对空值填充均值
def nan_mean_data(df, cols):
    # 插补均值
    val = df[cols].mean()
    df[cols] = df[cols].fillna(val)
    return 'All data is None' if df.empty else df


# 对空值填充中位数
def nan_median_data(df, cols):
    # 插补[中位数]
    val = df[cols].median()
    df[cols] = df[cols].fillna(val)
    return 'All data is None' if df.empty else df


# 对空值填充众数
def nan_mode_data(df, cols):
    # 插补[众数]
    val = df[cols].mode().iloc[0]
    df[cols] = df[cols].fillna(val)
    return 'All data is None' if df.empty else df


# 对空值填充固定值
def nan_fill_constant(df, cols):
    # 插固定值
    df[cols[:-1]] = df[cols[:-1]].fillna(cols[-1])
    return 'All data is None' if df.empty else df


# 对空值不做处理
def nan_donothing(df, cols):
    # 插补[不处理]
    donoth = df
    return 'data do nothing' if donoth.empty else donoth
  

```

4. 输入命令
```
python setup.py build
```

或

```
python setup.py build_ext --inplace
```

`cd build/lib`
文件夹中有两个so文件。

5. 调用so文件文件方法如下：

1). 方法1:

将这两个so文件移入到另外一文件目录下，在该目录下输入`python`。

可以直接输入`from data_preprocess import *`。

2). 方法2:

如果想再任意路径下使用so文件中的方法，那么需要在`/Users/sunlu/.pyenv/versions/3.6.2/lib/python3.6/site-packages/`路径下新建一个文件夹，将so文件放入文件夹中。

然后在任意路径下输入`python`均可找到该方法。

例如在`/Users/sunlu/.pyenv/versions/3.6.2/lib/python3.6/site-packages/`路径下新建`testDM`文件夹，将so文件放入该文件夹中。

在任意路径下输入`python`启动python，然后输入

```
from testDM.mysql_conn import *
from testDM.data_preprocess import *
```
> 注意：`data_preprocess`方法 依赖`mysql_conn`方法，因此要先输入`mysql_conn`方法。如果python脚本和so文件在同一路径下则没有先后顺序。

获取`/Users/sunlu/.pyenv/versions/3.6.2/lib/python3.6/site-packages/`路径的方法如下：

在任意路径下输入`python`启动python，然后输入
```
import sys
sys.path
```

3). 方法3: √ (个人比较喜欢这种)

python的sys.path模块路径添加so文件所在文件夹路径，也可以直接使用so文件中的方法。
```
import sys
# 添加so文件所在文件夹路径
sys.path.append("/Users/sunlu/Downloads/pyd_test4/")
# 引入包测试
from data_preprocess import *

```

> 注意：
> python封装.so文件注意事项
> <https://blog.csdn.net/ustcjinggg/article/details/100514463>
```
1.封装文件名需要和代码文件名保持相同

封装时你的Extension("a",["a.py"])第一个"a"是封装后.so文件的名字，由于我不想透露这个名字，我在写的时候改成了c,比如Extension("c",["a.py"])，这是生成的so文件会是c.so，但是如果b.py调用了a.py，那么就会报错，它不认识c.so文件。

2.如果代码文件有很多子目录怎么办

如果代码文件目录是：

--root:

./a.py

/core/b.py

/core/c.py

其中a调用b c,b c 又互相调用这个问题困扰我很久最后通过一个方法解决了，但我还没有相处原因。

先写一个setup1.py文件并执行：

from distutils.core import setup
from Cython.Build import cythonize
from distutils.extension import Extension

extensions = [
    Extension("a",["a.py"]，include_dirs=["./core"]),
    Extension("b",["core/b.py"]，include_dirs=["../core"]),

    Extension("c",["core/c.py"], include_dirs=["../core"]),
]
setup(ext_modules=cythonize(extensions))

先执行完这个文件(python3 setup.py build_ext)再执行下面的

from distutils.core import setup
from Cython.Build import cythonize
from distutils.extension import Extension

extensions = [
    Extension("a",["a.py"]，include_dirs=["./core"]),
    Extension("core/b",["core/b.py"]，include_dirs=["../core"]),

    Extension("core/c",["core/c.py"], include_dirs=["../core"]),
]
setup(ext_modules=cythonize(extensions))

只执行其中一个都会报错，我现在还不明白原因，但是依次执行生成的lib*文件夹就没有问题。

后面的操作相同的。

至于增加了inlude_dirs = ["./core"]我尚不清楚是否有用，但我实际执行的代码里写了

3.中间生成的temp*文件夹

我发现封装过程中build文件夹下的temp*文件夹是没用的，cd进去以后也不能用，最终的封装结果也不需要这个文件夹。


```