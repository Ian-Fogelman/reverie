---
layout: post
title:  SQL Wild cards
date:   2019-06-15 
description: A quick look at SQL wildcards. # Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [SQL, Like, Wildcard]

datatable: true
author: # Add name author (optional)
---

Today I will touch on the wild card functionality in SQL. 
Many times you will be searching for records in a database table by some type of name.
Instead of looking specifically for an exact match on a name you can specify a wildcard search in your where statement by using a like clause.

<br>
<br>

Lets take the following table, <strong>classinfo</strong> into consideration:

<br>
<br>

  <div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Address</th>
          <th>Class</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>Jane</td>
          <td>123 Beech street</td>
          <td>Math</td>
        </tr>
        <tr>
          <td>2</td>
          <td>John</td>
          <td>1234 Smith street</td>
          <td>Math</td>
        </tr>
        <tr>
          <td>3</td>
          <td>Jasmin</td>
          <td>4432 Etcher Rd</td>
          <td>Science</td>
        </tr>
        <tr>
          <td>4</td>
          <td>Chase</td>
          <td>9332 Green St</td>
          <td>Music</td>
        </tr>
      </tbody>
    </table>
  </div>
  
Lets suppose we want to get every record that is enrolled in a course whose first letter begins with <strong>"M"</strong>.
{% highlight sql %}
SELECT * FROM CLASSINFO WHERE CLASS LIKE 'M%'
{% endhighlight %}  
<br>
<br>

If we want to match on an entire word we use a double percent such as

<br>
<br>
{% highlight sql %}
SELECT * FROM CLASSINFO WHERE CLASS LIKE '%MUSIC%'
{% endhighlight %}  
<br>
<br>

We can also match on multiple characters the following, here we will return records that start with M or S.

<br>
<br>

{% highlight sql %}
SELECT * FROM CLASSINFO WHERE CLASS LIKE '[MS]%'
{% endhighlight %}  
<br>
<br>

Finally we can also skip characters if we are only interested in the third character for instance in the address field.
We can use the <strong> _ </strong> character. The following will return records whos 3rd character in address is 3.

<br>
<br>
{% highlight sql %}
SELECT * FROM CLASSINFO WHERE ADDRESS  LIKE '__3%'
{% endhighlight %}  
