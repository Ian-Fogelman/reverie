---
layout: post
title:  Validating UTF8 Errors
date:   2020-02-05
description: Validating UTF8 Errors
img: sq.png # Add image post (optional)
tags: [Python]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Validating UTF8 Errors">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

Recently a problem arouse in a production database of non UTF-8 encodable characters appearing in a name field. This can theoretically can happen in any user entered front end.
It may or may not be an immediate problem, eventually it can cause errors in data exports or other processes, so today I will share how I solved this issue with some dynamic SQL.
<br>
<br>
<a href="https://rextester.com/PHHX28464" target="_blank"> Rexy </a>
<br>
<br>
This will be done by 4 temp tables, Temp_A is the table that will hold the references to tables and columns, Temp_B and Temp_C will hold example data with non-UTF8 characters.
Examples of Non UTF8 encodable characters include : ŠŸž€ ΑΒΓΔΩαβγδω АБВГДабвгд, this can occur with foreign names with cultural accent markings. 
<br>
<br>

The last temp table temp_Result will hold the violating data, as well as the table and column of the table that violated the encoding.

<br>
<br>

This solution utilizes dynamic SQL and iterative cursor type logic, instead of using an exclusive cursor syntax I use a while loop.
Doing this can provide more flexibility than a cursor in my opinion.

<br>
<br>

~~~sql
DECLARE @dynamicstring VARCHAR(MAX),@trg INT = 1,@columnname VARCHAR(MAX),@tablename VARCHAR(MAX),@IdColumnName VARCHAR(MAX) 
WHILE (@trg <= (SELECT MAX(id) FROM [#TEMP_A]))
BEGIN 

	SET @columnname = (SELECT TOP 1 columnname FROM [#TEMP_A] WHERE ID = @TRG)
	SET @tablename = (SELECT TOP 1 [TABLENAME] FROM [#TEMP_A] WHERE ID = @TRG)

	SET @dynamicstring = '
	WITH AllNumbers AS
	(   SELECT 1 AS Number
		UNION ALL
		SELECT Number+1
			FROM AllNumbers
			WHERE Number < 50
	)
	INSERT INTO #TEMP_RESULT
	SELECT 
		BatchId, '''+ @tablename + ''' as TableName, ''' + @columnname + ''' BadValueColumn, CONVERT(varchar(50),' + @columnname + ') AS BadValue
		
		FROM ' + @tablename +' AS y
			INNER JOIN AllNumbers n ON n.Number <= LEN(y.' + @columnname + ')
		WHERE ASCII(SUBSTRING(y.' + @columnname + ', n.Number, 1))<32 OR ASCII(SUBSTRING(y.' + @columnname + ', n.Number, 1))>127'
   PRINT(@dynamicstring)
   EXEC(@dynamicstring)
   SET @TRG = @TRG + 1
END

~~~


<br>
<br>

Each time the loop runs it grabs the table and column name in the reference table temp_A, this gives flexibility in design.
The idea being to add fields and tables to the process all you need to do is add the parameters to this table.




