---
layout: post
title:  SQL Simple Having 
categories: [SQL, Having, Query]
datatable: true
---

Howdy partner, today I will dicuss a way to filter your aggregated data, this is done by the use of a not so often used query keyword <strong> Having </strong>.
<br >

The whole solution can be viewed at this <a href="https://rextester.com/MIWL74883" target="_blank"> RexTester</a>.

<br>

I say not so used because it is only maybe about 5% of the time that you will use this keyword, 
there are many other ways to achieve desired results such as CTE,Subquies and window functions.

<br>

Lets assume we have a dataset such as this:

<br>

  <div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>Jim</td>
        </tr>
		<tr>
          <td>1</td>
          <td>Jim</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Bob</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Bob</td>
        </tr>
		<tr>
          <td>2</td>
          <td>Bob</td>
        </tr>
      </tbody>
    </table>
  </div>
  
<br>

We simply want to know what name has 3 or more entries in the table.
We can apply a having clause to achieve this Having clause will go outside of the group by as an addition selectivity measure  in our query.

<br>

{% highlight sql %}
  SELECT [NAME], COUNT(*) AS RecordCount 
  FROM #TEMP_A 
  GROUP BY [NAME] 
  HAVING COUNT(*) >= 3
{% endhighlight %}  