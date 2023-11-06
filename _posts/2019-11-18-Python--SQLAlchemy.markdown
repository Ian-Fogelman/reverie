---
layout: post
title:  Python and SQLAlchemy
date:   2019-11-18
description: SQLalchemy enginge
img: sq.png # Add image post (optional)
tags: [Python,SQLAlchemy]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Python and SQLAlchemy">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

A quick example of how to create a SQLAlchemy engine connection string in python.
<br>
<br>

{% highlight Python %}

from sqlalchemy import *
import pyodbc
engine = create_engine("mssql+pyodbc://SERVER\\INSTANCE/DATABASE?driver=SQL+Server+Native+Client+11.0")

{% endhighlight %}

<br>
<br>

Once defined we can use the engine to feed dataframes into our SQL instances such as :

<br>
<br>

{% highlight Python %}
df.to_sql("TABLE", engine, if_exists='append',index = False, chunksize = 200)
{% endhighlight %}


<br>
<br>

Additionally we can run selects from the engine we defined and read that data straigth into a data frame.
<br>
<br>

{% highlight Python %}
df = pd.read_sql_query("SELECT top 10 * from XXXX", engine)
{% endhighlight %}

Here is an example of using SqlAlchemy to retieve stock prices and stock in a SQL server database.
<a href="https://anaconda.org/IanFogelman/stock-analysis/notebook" target="_blank">Example</a>
