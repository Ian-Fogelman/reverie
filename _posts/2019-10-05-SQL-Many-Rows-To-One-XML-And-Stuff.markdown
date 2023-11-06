---
layout: post
title:  SQL Many Rows To One XML And Stuff
date:   2019-10-05
description: How to combine rows in SQL example
img: py.png # Add image post (optional)
tags: [SQL,XML,STUFF]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<br>
<br>
This question arose on the SO forum some months ago.
<br>
<br>
![Before](/assets/img/Stuff1.PNG)
<br>
<br>
The poster had a number of rows per UserID and wanted a result set of one row per userId with all the roles that would be associated to them.
<br>
<br>
![After](/assets/img/Stuff2.PNG)
<br>
<br>

The question appears easy, but it does take a bit of critical thinking because any simple join on userId will result in many rows.

My solution was to use the STUFF and For XML trick to work for you.

{% highlight SQL %}

SELECT 
   U.USER_NAME,
   STUFF((SELECT ',' + UR.ROLE 
          FROM [USER_ROLES] UR
          WHERE UR.USER_ID = U.USER_ID
          FOR XML PATH('')), 1, 1, '') [ROLES]
FROM [USER] U
GROUP BY U.USER_NAME, U.USER_ID
ORDER BY 1

{% endhighlight %}

<br>
<br>

<a href="https://rextester.com/YLIJQ30828" target="_blank">https://rextester.com/YLIJQ30828</a>

<br>
<br>
Example of compiling a comma seperated list from a <u>single</u> table.
<br>
<br>

{% highlight SQL %}

SELECT STUFF((
SELECT ',' + EMPLOYEEID FROM [#TEMP_A]
FOR XML PATH('')), 1, 1, '') as Employees

{% endhighlight %}



<a href="https://rextester.com/XBUW63425" target="_blank"> https://rextester.com/XBUW63425</a>

