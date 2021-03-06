# 在flask中启动分布式版pyspark任务


>  参考链接
>
>
>
> Running Flask on Spark2
> <https://www.thegoldfish.org/2018/04/running-flask-on-spark2/>


exampleweb.py
    
    #! /usr/bin/env python
    # -*- coding: utf-8 -*-
    # vim:fenc=utf-8
    #
    
    """
    Flask on Spark example.
    
    Run with:
    
        ./bin/spark-submit --master spark://$(hostname -s):7077 exampleweb.py
    
    """
    
    import sys
    sys.path.append( './python' )
    from random import random
    from operator import add
    
    from flask import Flask, request
    from pyspark.sql import SparkSession
    
    app = Flask(__name__)
    
    spark = SparkSession\
            .builder\
            .appName("Flark - Flask on Spark")\
            .getOrCreate()
    
    @app.route("/")
    def hello():
        return "Hello World! There is a spark example at <a href=\"/pi?partitions=1\">/pi</a>"
    
    @app.route("/pi")
    def pi():
    
        try:
            partitions = int(request.args.get('partitions', '1'))
        except Exception as e:
            return e
    
        partitions = 4
        n = 1000000 * partitions
    
    
        def f(_):
            x = random() * 2 - 1
            y = random() * 2 - 1
            return 1 if x ** 2 + y ** 2 <= 1 else 0
    
        count = spark.sparkContext.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
        return "Pi is roughly %f" % (4.0 * count / n)
    
    
    if __name__ == "__main__":
        app.run()


Now we can start our Flask application by submitting it to spark.

    ./bin/spark-submit --master spark://$(hostname -s):7077 exampleweb.py
    
And then access it at `http://localhost:5000/` and `http://localhost:5000/pi?partitions=1`.