---
layout: post
title:  Dynamic Python
date:   2020-04-08
description: 2020-04-08-Dynamic Python
img: py.png # Add image post (optional)
tags: [Dynamic Python]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Dynamic Python">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
<br>
Many times in programming you need to encorporate dynamic programming. Meaning that you want to apply a variable or many variables into a programming statement and run it. This is a pretty common need and can be achieved in MS SQL as follows: 
<br>
<br>
<br>

{% highlight SQL %}

DECLARE @DYNSQL VARCHAR(MAX)
DECLARE @TABLENAME VARCHAR(MAX)

IF OBJECT_ID('TEMPDB..#TEMP_A') IS NOT NULL DROP TABLE #TEMP_A
CREATE TABLE #TEMP_A
(
TABLENAME VARCHAR(MAX)
)


INSERT INTO #TEMP_A
SELECT 'SYS.all_objects'
UNION
SELECT 'SYS.all_views'
UNION
SELECT 'SYS.all_columns'



WHILE EXISTS(SELECT * FROM #TEMP_A)
BEGIN
	SET @TABLENAME = (SELECT TOP 1  TABLENAME  FROM #TEMP_A)
	SET @DYNSQL = 'SELECT * FROM ' +  @TABLENAME 
	EXEC(@DYNSQL)
	DELETE FROM #TEMP_A WHERE TABLENAME = @TABLENAME
END

DROP TABLE #TEMP_A

{% endhighlight %}

<br>
<br>
<br>

Here we can see that we have a static part of a query 'SELECT * FROM ' and the dynamic part is the table name.
<br>
<br>
The beauty of this approach is we can achieve very complex query criteria and reduce our line of code counts by utalizing CURSORs and while loops, I prefer the ladder.
<br>
<br>

This same functionality is achievable in Python, here is a code sample :
<br>
<br>

{% highlight Python %}

prog = 'print("The sum of 5 and 10 is", (5+10))'
exec(prog)
#The sum of 5 and 10 is 15

expression = '5 + 3 * a'
a = 5
eval(expression)

{% endhighlight %}

<br>
<br>
