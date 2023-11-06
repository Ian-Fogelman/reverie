---
layout: post
title:  Using Python scripts natively in SQL Server 2017
date:   2019-10-07
description: Overview of running Python scripts on a SQL Server 2017 instance!
img: sq.jpg # Add image post (optional)
tags: [Python,SQL,SSMS,Sql Server]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Using Python scripts natively in SQL Server 2017">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
In a recent blog post I covered how to get started with C# common language runtimes. I mentioned that the flavors available are C# and Visual basic, this technology began shipping with SQL Server 2005. 
<br>
<br>
However a more recent development in the SQL server world began shipping with SQL Server 2017 and this is availability for Python and R scripts to run natively in your SQL instance.
<br>
<br>
This is extremly interesting because now you can develop stored procedures that utalize all the machine learning models that you have developed natively. And your OLTP data can be ran through the models natively on your server.
<br>
<br>
To get things started I will just show a few high level steps of how to get your SQL instance primed for this.
<br>
<br>
Firstly your going to need a SQL server 2017 instance, I learned that SQL express editions do not carry the functionaility for Python and R scripting. So you can download a 180 day eval edition <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2017-rtm" target="_blank" >here</a>.
<br>
<br>
The main thing to be concerned with here under feature selection is to click the check for Python under database engine services.
<br>
<br>

![Features](/assets/img/PythonS001.PNG)

<br>
<br>
Additionally when your instance has finished installing enable external scripts.
<br>
<br>
{% highlight SQL %}

sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  

{% endhighlight %}
<br>
<br>
Once that is complete, a restart for the instance will need to be performed.
<br>
<br>
![Restart](/assets/img/PythonS002.PNG)
<br>
<br>

Here is an example to check the location of the install for specifically the SQL Server 2017 Python install.
<br>
<br>
A quick note here that if you run anaconda or Python on the client machine already, the dependencies that you have will not be carried over to this new SQL Server 2017 Python. It is completly different and you will need to manage the libraries seperately!
<br>
<br>
{% highlight SQL %}
EXEC sp_execute_external_script

  @language =N'Python',

  @script=N'import sys; print("\n".join(sys.path))'
{% endhighlight %}

<br>
<br>
Now you have the install location you can open a CMD prompt and cd the directory and then issue a pip.exe command to install a new module. Here is a example:
<br>
<br>
{% highlight CMD %}
cd C:\Program Files\Microsoft SQL Server\MSSQL14.FOGELDEV2017\PYTHON_SERVICES\Scripts
pip.exe install text-tools
{% endhighlight %}
<br>
<br>

And thats it congratulations, you are all setup to run Python on your SQL instance!
<br>
<br>
It is possible to output data straight to the query grid like you would a normal procedure in SQL Server. This opens up the flood gates in terms of data warehousing, model performance and so much more interesting functionality that is available in Python.
<br>
<br>
Passing data to a Python CLR Proc, here I wrote a function to count the frequency of characters in the text passed to the proc. The results are then translated to a pandas dataframe and returned in the format specified of WITH RESULT SETS:
<br>
<br>
{% highlight SQL %}
CREATE PROCEDURE CountChars (
      @param1 VARCHAR(MAX)
    )
AS
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import pandas as pd
freq_count = [(c, text.count(c)) for c in text if c.isalpha()]
OutputDataSet = sorted(list(set(freq_count)), key=lambda x: (-x[1], x[0]))
df = pd.DataFrame(OutputDataSet)

OutputDataSet = df
'
, @input_data_1 = N'   ;'
    , @params = N' @text VARCHAR(MAX)'
    , @text = @param1    
WITH RESULT SETS(([Letter] CHAR(20) NOT NULL,[Count] CHAR(20) NOT NULL));
{% endhighlight %}
<br>
<br>
{% highlight SQL %}
exec CountChars 'super fun sql' 
{% endhighlight %}
<br>
<br>
![XImage](/assets/img/PythonS003.PNG)

