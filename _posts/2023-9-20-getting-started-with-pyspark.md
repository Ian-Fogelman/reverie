---
layout: post
title:  Getting Started With PySpark
categories: [Python, SQL, Spark, PySpark]
---
Howdy partner, today we look at how to easily get started with PySpark.

PySpark is the Python library for Apache Spark, an open-source, distributed computing system designed for big data processing and analytics.


PySpark allows Python developers to leverage the power of Spark for distributed data processing while working within the Python programming environment. It has gained popularity for its flexibility, performance, and support for various data processing tasks, ranging from batch processing to machine learning and streaming data analytics.

The easiest way to install PySpark is with Docker.
If the image is not present on your system, Docker will pull it automatically.

This Docker image [jupyter/pyspark-notebook](https://hub.docker.com/r/jupyter/pyspark-notebook) is maintained by the Jupyter Project.

Docker command from terminal:

```
docker run -p 8888:8888 jupyter/pyspark-notebook
```

As a windows users, I like to create a bat script in my startup folder so my Spark instance starts everytime I boot up my computer.
Any shortcut or bat executable you put in the directory `C:\Users\XXXX\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` fires on windows startup.
If you make a bat file with the docker command, your PySpark environment will attempt to spin up on every restart:

PySpark.bat
```
docker run -p 8888:8888 jupyter/pyspark-notebook
pause
```

Once the Docker image loads, create a new notebook and paste the following code to validate your now running PySPark:

MyFirstPySpark.ipynb
```
import urllib.request
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Basics").getOrCreate()

urllib.request.urlretrieve("https://gist.githubusercontent.com/Ian-Fogelman/e924c13d2ad838cad5d2c96398bf3073/raw/6d713de4a29223f35027748ce62fa7f8786755f7/iris.csv", "iris.csv")
df = spark.read.csv('iris.csv', header='true')

df.show()
df.printSchema()
```

Create a table and run raw SQL commands from a dataframe :
```
df.createOrReplaceTempView("iris")
spark.sql("SELECT * from iris where species = 'setosa' limit 5").show()
```