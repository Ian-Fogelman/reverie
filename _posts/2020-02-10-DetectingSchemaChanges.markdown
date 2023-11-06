---
layout: post
title:  Automatically detecting schema changes
date:   2020-02-10
description: How to easily detect which database tables have changed.
img: sq.png # Add image post (optional)
tags: [SQL,ChangeDetection]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Automatically detecting schema changes">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Have you ever been blind sided by a database schema change in your production enviorment? It is no fun!
You may work in a enviorment when changes are few and far between or hundreds per day, either way it would be great to automatically detect changes.
Today I am going to explain how to do just that! Lets get started, here is the <a href="https://rextester.com/BNWS86838" target="_blank">Full Rexy</a>.
<br>
<br>

Lets start with this basic query to the sys objects and sys columns DMVs. This query will return the tablename, name of column, column id ,column type and the length.
This will be the foundation of our query to detect changes, we still need to do some work to get the exact result we want.

<br>
<br>

{% highlight SQL %}

SELECT CAST([SUB].[NAME] AS VARCHAR(MAX)) AS [text()]
FROM
    (SELECT ROW_NUMBER() OVER (PARTITION BY 1 ORDER BY [COLUMN_ID]) AS [ID],
            [tablename] + ' | ' + [name] + ' | '
            + CAST([column_id] AS VARCHAR(MAX)) + ' | '
            + CAST([max_length] AS VARCHAR(MAX)) + ' | ' + [ColType] AS [Name]
     FROM
         (SELECT [OBJ].[name] AS [TableName],
                 [COL].[name],
                 [COL].[column_id],
                 TYPE_NAME([system_type_id]) AS [ColType],
                 [COL].[max_length]
          FROM
              [SYS].[columns] AS [COL]
              JOIN [SYS].[OBJECTS] AS [OBJ]
              ON [COL].[object_id] = [OBJ].[object_id]) AS [X] ) AS [SUB];

{% endhighlight %}

<br>
<br>

Next we need to add a subquery to this base query to blend all of those rows into a single row.
To accomplish this we will utalize the STUFF and For XML path trick seperating each with a comma.

<br>
<br>

{% highlight sql %}
SELECT ','+ CAST(SUB.NAME AS VARCHAR(MAX)) AS [text()]
                        FROM (
						SELECT ROW_NUMBER() OVER(PARTITION BY 1 ORDER BY COLUMN_ID) AS ID,
tablename + ' | ' + name + ' | ' + CAST(column_id AS VARCHAR(MAX)) + ' | ' + CAST(max_length AS VARCHAR(MAX)) + ' | ' + ColType AS [Name] 
FROM(
SELECT OBJ.name AS TableName,COL.[name],COL.[column_id],TYPE_NAME(system_type_id) AS ColType,COL.[max_length]
FROM SYS.columns AS COL
	JOIN SYS.OBJECTS AS OBJ
	ON COL.object_id = OBJ.object_id

) AS X ) SUB
FOR XML PATH('')

{% endhighlight %}

<br> 
<br>

Now we have a single row comma seperated of every column for each table in the database we are connected to.
We need to parameterize and get rid of the leading comma, this is where stuff comes in. It works similar to substring,
and concatenation in a single function. Lets also go ahead and roll the entire function into a scalar function. The final resulting function looks as follows:

<br> 
<br>

{% highlight sql %}
CREATE FUNCTION FN_RETURN_CHECK_SUM(@OBJECT VARCHAR(MAX))
RETURNS NVARCHAR(32)

AS
BEGIN

/*
SELECT DBO.FN_RETURN_CHECK_SUM('[dbo].[mytable]')
*/


DECLARE @RET NVARCHAR(32)
SET @RET = (SELECT 
	   DISTINCT 
            CONVERT(NVARCHAR(32),HASHBYTES('MD5',STUFF((    SELECT ','+ CAST(SUB.NAME AS VARCHAR(MAX)) AS [text()]
                        FROM (
						SELECT ROW_NUMBER() OVER(PARTITION BY 1 ORDER BY COLUMN_ID) AS ID,
tablename + ' | ' + name + ' | ' + CAST(column_id AS VARCHAR(MAX)) + ' | ' + CAST(max_length AS VARCHAR(MAX)) + ' | ' + ColType AS [Name] 
FROM(
SELECT OBJ.name AS TableName,COL.[name],COL.[column_id],TYPE_NAME(system_type_id) AS ColType,COL.[max_length]
FROM SYS.columns AS COL
	JOIN SYS.OBJECTS AS OBJ
	ON COL.object_id = OBJ.object_id
WHERE COL.OBJECT_ID = 
(SELECT OBJECT_ID(@OBJECT))
) AS X ) SUB
FOR XML PATH('')), 1, 1,'')),2) AS CheckSumValue )
RETURN @RET
END;
{% endhighlight %}

<br>
<br>

Lastly its time to test the function, lets create a table, plug in the table name, modify the table and compare checksum values.

<br>
<br>

{% highlight sql %}

CREATE FUNCTION FN_RETURN_CHECK_SUM(@OBJECT VARCHAR(MAX))
RETURNS NVARCHAR(32)

AS
BEGIN

/*
SELECT DBO.FN_RETURN_CHECK_SUM('[dbo].[mytable]')
*/


DECLARE @RET NVARCHAR(32)
SET @RET = (SELECT 
	   DISTINCT 
            CONVERT(NVARCHAR(32),HASHBYTES('MD5',STUFF((    SELECT ','+ CAST(SUB.NAME AS VARCHAR(MAX)) AS [text()]
                        FROM (
						SELECT ROW_NUMBER() OVER(PARTITION BY 1 ORDER BY COLUMN_ID) AS ID,
tablename + ' | ' + name + ' | ' + CAST(column_id AS VARCHAR(MAX)) + ' | ' + CAST(max_length AS VARCHAR(MAX)) + ' | ' + ColType AS [Name] 
FROM(
SELECT OBJ.name AS TableName,COL.[name],COL.[column_id],TYPE_NAME(system_type_id) AS ColType,COL.[max_length]
FROM SYS.columns AS COL
	JOIN SYS.OBJECTS AS OBJ
	ON COL.object_id = OBJ.object_id
WHERE COL.OBJECT_ID = 
(SELECT OBJECT_ID(@OBJECT))
) AS X ) SUB
FOR XML PATH('')), 1, 1,'')),2) AS CheckSumValue )
RETURN @RET
END;
GO

CREATE TABLE TEMP_A
(
ID INT,
NAME VARCHAR(50)
)
--BEFORE ALTER CHECK SUM OF 55FD251F00F8E1EB7A2B7DCBF75A3D30
SELECT DBO.FN_RETURN_CHECK_SUM('[dbo].[TEMP_A]')

ALTER TABLE TEMP_A
ADD Email varchar(255);
--AFTER ALTER CHECK SUM OF 4E1BA8DA9302CE7DD21949454D39F22A
SELECT DBO.FN_RETURN_CHECK_SUM('[dbo].[TEMP_A]')

{% endhighlight %}

As you can see for the first run we get a check sum for the first run is 55FD251F00F8E1EB7A2B7DCBF75A3D30
<br> 
<br>
The second run is 4E1BA8DA9302CE7DD21949454D39F22A.
<br> 
<br>
This technique will work for any column changes, if the order, datatype or length changes the checksum will be different!
You could expand this function to alert via teams or an email notification to keep you in the know when these changes occur.

