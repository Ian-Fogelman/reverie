---
layout: post
title:  Using SQL Window and Offset Window Functions
date:   2020-01-14
description: Using SQL Window and Offset Window Functions
img: sq.png # Add image post (optional)
tags: [SQL,Window Functions]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Using SQL Window and Offset Window Functions">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
Hello There!
<br>
<br>
Today I was faced with a task of diseminating sequence of events in a report, so I thought I would share how I approached the task.
<br>
<br>

The Rexy can be found here : <a href="https://rextester.com/AVTOWD47990" target="_blank">https://rextester.com/AVTOWD47990</a>

<br>
<br>
Using window functions such as ROW_NUMBER(), RANK(), DENSE_RANK() and NTILE() we can gain insight into how data are ranked amoung a result set. Each of these functions have specific use cases, rank and dense rank treat ties amoungst data slightly differently while ROW_NUMBER() is useful for assigning a ID field to a result set if for some reason one is not already present.

<br>
<br>
Have a look at the data below which has 4 fields, OrderId,CustId,Val and OrderDate. Typically you will be comparing values based on a sequence of a date field. In this example we will apply first the window functions and secondly offset window functions.

<br>
<br>

  <div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>OrderId</th>
          <th>CustId</th>
          <th>Val</th>
          <th>OrderDate</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>10782</td>
          <td>12</td>
          <td>18</td>
          <td>2017-01-01</td>
        </tr>
		    <tr>
          <td>10807</td>
          <td>12</td>
          <td>23</td>
          <td>2016-01-01</td>
        </tr>
        <tr>
          <td>10586</td>
          <td>12</td>
          <td>28</td>
          <td>2018-01-01</td>
        </tr>
        <tr>
          <td>10767</td>
          <td>28</td>
          <td>30</td>
          <td>2017-02-01</td>
        </tr>
        <tr>
          <td>10898</td>
          <td>28</td>
          <td>33</td>
          <td>2016-02-01</td>
        </tr>
        <tr>
          <td>10900</td>
          <td>28</td>
          <td>36</td>
          <td>2018-02-01</td>
        </tr>
        <tr>
          <td>10883</td>
          <td>36</td>
          <td>36</td>
          <td>2017-03-01</td>
        </tr>
        <tr>
          <td>11051</td>
          <td>36</td>
          <td>40</td>
          <td>2016-03-01</td>
        </tr>
        <tr>
          <td>10815</td>
          <td>36</td>
          <td>45</td>
          <td>2018-03-01</td>
        </tr>
      </tbody>
    </table>
  </div>

<br>
<br>
Lets first apply the window function query to our data, this will show how each treats data slightly differently.

<br>
<br>

{% highlight SQL %}

SELECT 
ROW_NUMBER() OVER(ORDER BY VAL) AS ROWNUM,
RANK() OVER(ORDER BY VAL) AS RANK,
DENSE_RANK() OVER(ORDER BY VAL) AS DENSE_RANK,
NTILE(5) OVER(ORDER BY VAL) AS NTILE,
VAL

FROM #TEMP_ORDERVALUES
ORDER BY VAL
{% endhighlight %}

<br>
<br>

This will produce the result set

<br>
<br>

<div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>RowNum</th>
          <th>Rank</th>
          <th>Dense_Rank</th>
          <th>Ntile</th>
	  <th>Val</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>1</td>
          <td>1</td>
          <td>1</td>
	  <td>18</td>
        </tr>
	<tr>
          <td>2</td>
          <td>2</td>
          <td>2</td>
          <td>1</td>
	  <td>23</td>
        </tr>
	<tr>
          <td>3</td>
          <td>3</td>
          <td>3</td>
          <td>2</td>
	  <td>28</td>
        </tr>
	<tr>
          <td>4</td>
          <td>4</td>
          <td>4</td>
          <td>2</td>
	  <td>30</td>
        </tr>
	<tr>
          <td>5</td>
          <td>5</td>
          <td>5</td>
          <td>3</td>
	  <td>33</td>
        </tr>
	<tr>
          <td>6</td>
          <td>6</td>
          <td>6</td>
          <td>3</td>
	  <td>36</td>
        </tr>
	<tr>
          <td>7</td>
          <td>6</td>
          <td>6</td>
          <td>3</td>
	  <td>36</td>
        </tr>
	<tr>
          <td>8</td>
          <td>8</td>
          <td>7</td>
          <td>4</td>
	  <td>40</td>
        </tr>
	<tr>
          <td>9</td>
          <td>9</td>
          <td>8</td>
          <td>5</td>
	  <td>45</td>
        </tr>
      </tbody>
    </table>
  </div>

<br>
<br>


Lets Examine row numbers 6,7 and 8 to gain a better understanding of the window functions.

<br>
<br>


<div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>RowNum</th>
          <th>Rank</th>
          <th>Dense_Rank</th>
          <th>Ntile</th>
	  <th>Val</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>1</td>
          <td>1</td>
          <td>1</td>
	  <td>18</td>
        </tr>
	<tr>
          <td>2</td>
          <td>2</td>
          <td>2</td>
          <td>1</td>
	  <td>23</td>
        </tr>
	<tr>
          <td>3</td>
          <td>3</td>
          <td>3</td>
          <td>2</td>
	  <td>28</td>
        </tr>
	<tr>
          <td>4</td>
          <td>4</td>
          <td>4</td>
          <td>2</td>
	  <td>30</td>
        </tr>
	<tr>
          <td>5</td>
          <td>5</td>
          <td>5</td>
          <td>3</td>
	  <td>33</td>
        </tr>
	<tr style="background-color:yellow;">
          <td>6</td>
          <td>6</td>
          <td>6</td>
          <td>3</td>
	  <td>36</td>
        </tr>
	<tr style="background-color:yellow;">
          <td>7</td>
          <td>6</td>
          <td>6</td>
          <td>3</td>
	  <td>36</td>
        </tr>
	<tr style="background-color:yellow;">
          <td>8</td>
          <td>8</td>
          <td>7</td>
          <td>4</td>
	  <td>40</td>
        </tr>
	<tr>
          <td>9</td>
          <td>9</td>
          <td>8</td>
          <td>5</td>
	  <td>45</td>
        </tr>
      </tbody>
    </table>
  </div>

<br>
<br>

Firstly lets cover NTILE(), NTILE accepts an integer parameter which is the number of groups to partition by. For example I choose 5, so we can mod the number of rows by that parameter 9 % 5 = 1, we will have 4 equal groups (with 2 in this case) and 1 odd group with only one.

<br>
<br>

Next Rank, Rank will honor the ties in our data for example for rows 6 and 7 we have val of 36. Notice that rank honors the tie by assigning 6 to both rows but then skips the next rank and jumps straight to 8. This is essentially the different between RANK() and DENSE_RANK()

<br>
<br>

Lastly Dense_Rank will honor the ties in data and keep the sequence of rank instead of jumping to the next value. So instead of jumping to rank 8 for row number 8, it is assigned a dense_rank of 7, dense_rank will always have contigious values.

<br>
<br>

Last but not least is the LAG() and LEAD() functions, these are referred to as OFFSET WINDOW functions, so just imagine putting little windows inside of your result sets and that allow you to view the next logical iteration of a row. This iterations are decided by the PARTITION BY statement in the function, each call to this function will need a PARTITION BY and ORDER BY inside of the over clause. The Complete syntax looks as follows:

<br>
<br>


{% highlight SQL %}
SELECT TOP 9999 CUSTID,
ORDERID,
VAL,
LAG(VAL) OVER(PARTITION BY CUSTID ORDER BY ORDERDATE,ORDERID) AS PREVVAL,
LEAD(VAL) OVER(PARTITION BY CUSTID ORDER BY ORDERDATE,ORDERID) AS NEXTVAL
FROM #TEMP_ORDERVALUES
{% endhighlight %}

<br>
<br>
Here we are saying, lets select the next VAL field, per CustId and order by OrderDate and OrderId (Ascending is assumed if you do not specify ASC or DESC). Lag Works the same way, a common hang up is the Order By Clause inside of the Partition, usually you can play around and figure out the correct combination of ASC DESC and PARTITION BY to achieve your reporting needs.
<br>
<br>
Here is the result set from the query above, notice that when the custID is the first in window the PREVVAL field is NULL and when it is the final VAL in window the NEXTVAL is NULL. This confirms that our window is setup correctly for CUSTID.

<br>
<br>


<div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>CustID</th>
          <th>OrderID</th>
          <th>Val</th>
          <th>PrevVal</th>
	  <th>NextVal</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>12</td>
          <td>10807</td>
          <td>23</td>
          <td>NULL</td>
	  <td>18</td>
        </tr>
	<tr>
          <td>12</td>
          <td>10782</td>
          <td>18</td>
          <td>23</td>
	  <td>28</td>
        </tr>
	<tr>
          <td>12</td>
          <td>10586</td>
          <td>28</td>
          <td>18</td>
	  <td>NULL</td>
        </tr>
	<tr>
          <td>28</td>
          <td>10898</td>
          <td>33</td>
          <td>NULL</td>
	  <td>30</td>
        </tr>
	<tr>
          <td>28</td>
          <td>10767</td>
          <td>30</td>
          <td>33</td>
	  <td>36</td>
        </tr>
	<tr>
          <td>28</td>
          <td>10900</td>
          <td>36</td>
          <td>30</td>
	  <td>NULL</td>
        </tr>
	<tr>
          <td>36</td>
          <td>11051</td>
          <td>40</td>
          <td>NULL</td>
	  <td>36</td>
        </tr>
	<tr>
          <td>36</td>
          <td>10883</td>
          <td>36</td>
          <td>40</td>
	  <td>45</td>
        </tr>
	<tr>
          <td>36</td>
          <td>10815</td>
          <td>45</td>
          <td>36</td>
	  <td>NULL</td>
        </tr>
      </tbody>
    </table>
  </div>
  
  

<br>
<br>

#https://rextester.com/PHHX28464

