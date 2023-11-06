---
layout: post
title:  Getting-Started-With-SnowFlake
date:   2020-09-03
description: 2020-09-03-Getting-Started-With-SnowFlake
img: SF.png # Add image post (optional)
tags: [SnowFlake,SQL,AWS]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Getting Started With SnowFlake">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
Today we will be discussing a new and interesting cloud database technology called SnowFlake.
<br>
<br>

SnowFlake is a cloud based data warehouse solution which can be hosted in AWS, Azure or Google Cloud platform.
Initial trials of SnowFlake get a $400 credit 30 day trial.
<br>
<br>

Snowflake breaks out charges into a credit system by compute and storage.
You can sign up for a free trial of SnowFlake <a href="https://trial.snowflake.com/" target="_blank">here</a>. 

<br>
<br>

For these posts I am going to assume that you have signed up and stood up your test instance of SnowFlake.

<br>
<br>

We are going to cover how SnowFlake handles compute and loading some data from S3 into SnowFlake.

<br>
<br>
<h1>Compute</h1>
<br>
<br>
Compute in SnowFlake is handled by warehouses, warehouses can be sized according to need. For example you may have a warehouse for reporting, and another warehouse for data science query processing. You are not limited by the number of warehouses, these warehouses will consume credits when active and will auto suspend themselves when not in use. All these options can be configured in the Warehouses tab :

![](/assets/img/SF1.PNG)

<br>
<br>
<h1>Loading Data into SnowFlake</h1>
<br>
<br>
When you choose a source destination to load data from in S3, make sure it matches the region you choose when creating your SnowFlake instance.
Also make sure that you allow a list object, read bucket permissions and possibly get access in your CORS rules for that bucket.
<br>
<br>
![](/assets/img/SF2.PNG)
<br>
<br>


{% highlight SQL %} 
CREATE DATABASE Debug;

USE DATABASE Debug;

--CREATE TABLE
CREATE TABLE Customer (
  Customer_ID string,
  Customer_Name string ,
  Customer_Email string ,
  Customer_City string 
  );
  
--CREATE STAGE
create or replace stage CustomerStage url='s3://fogeldev-snowflake/';

list @CustomerStage;

--COPY DATA FROM S3
copy into Customer
  from s3://fogeldev-snowflake/Customers.csv
  file_format = (type = csv field_delimiter = ',' skip_header = 1);
  
SELECT * FROM Customer;

{% endhighlight %}


<br>
<br>

In the above code we created a database, set the database context, created a table and created a stage from which to load a csv file hosted in a s3: bucket. And lastly using the stage copied that csv data into our SnowFlake table.

<br>
<br>

You can also load data without stages and flatten json formated data in SnowFlake quite easily, which I will cover in future topics.

