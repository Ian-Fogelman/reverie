---
layout: post
title:  An introduction to Data Quality Services
date:   2019-06-24
description: How to deploy and configure a DQS instance.# Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [DQS, SQL, Data Cleasing]

datatable: true
author: # Add name author (optional)
---

The use case for DQS is simple, a quick way to audit and correct data through an abstraction layer. This abstraction layer can be
managed by someone other than a DBA or developer and help improve your database's data quality and over all employee productivity.

<br>
<br>

First step is to install the DQS client, you can search for data quality server installer on your machine running your SQL instance. 
<br>
Once you follow the prompts and install you should to able to access to DQS client.

<br>
<br>
![00](/assets/img/DQS00.PNG)

<br>
<br>

Lets take a simple set of data as follows :

<br>
<br>

<div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>ID</th>
          <th>Fname</th>
          <th>Lname</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>Ian</td>
          <td>Fogelman</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Bill</td>
          <td>Gates</td>
        </tr>
        <tr>
          <td>3</td>
          <td>Jim</td>
          <td>Brown</td>
        </tr>
		        <tr>
          <td>4</td>
          <td>D</td>
          <td>Smith</td>
        </tr>
		<tr>
          <td>5</td>
          <td>J</td>
          <td>Green</td>
        </tr>
      </tbody>
    </table>
  </div>


<br>
<br>

Suppose that we want to enforce a business rule that each first name must be 3 characters.
<br>
We will solve this using a DQS approach.


<br>
<br>
First step is to create a knowledge base
<br>
<br>
![1](/assets/img/DQS1.png)
<br>
<br>
Create New Domain
<br>
<br>
Input a name and a description for your domain
<br>
<br>
![2](/assets/img/DQS2.png)
<br>
<br>
Next click domain rules and enter Length3 for name and add a rule of length equal to 3
<br>
<br>
![X](/assets/img/DQSX.PNG)
<br>
<br>
Click publish
<br>
<br>
![3](/assets/img/DQS3.png)
<br>
<br>
Now go to data quality projects, New Data Quality Project.
<br>
Name the project and select the knowledge base created, for this project we will be cleansing.
<br>
<br>
![4](/assets/img/DQS4.png)
<br>
<br>
On the next screen match each field name with a domain, in this case we only have first name, click next.
<br>
<br>
![5](/assets/img/DQS5.png)
<br>
<br>
Click Start
<br>
<br>
![6](/assets/img/DQS6.png)
<br>
<br>
After the task is complete you can see how many rows of data passed and how many were invalid.
<br>
<br>
![7](/assets/img/DQS7.png)
<br>
<br>
On the following screen we can see which records failed, because the length of the first name field was not 3.
<br>
Which we originally stated in the criteria of the domain. You can approve and reject.
<br>
<br>
![8](/assets/img/DQS8.png)
<br>
<br>
On the last screen we can Export our results to a table in the database by defining the table name and clicking export.
<br>
<br>
![9](/assets/img/DQS9.png)
<br>
<br>
Once clicking finish , we can view the data in the Output table in the database we specificied to see the full results
<br>
<br>
![0](/assets/img/DQS0.png)
<br>
<br>

DQS can be used in combination with other technology such as SSIS/PowerBi/SSRS and the process of reporting data issues or automatically fixing them becomes much more interesting!

