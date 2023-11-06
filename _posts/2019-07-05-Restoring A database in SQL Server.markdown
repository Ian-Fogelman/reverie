---
layout: post
title:  Restoring A database in SQL Server
date:   2019-07-05
description: A quick look at how to restore a database from a mdf file. # Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [SQL Server, Database backup, Database restore]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<br>
<br>
Today we discuss a powerful topic, restoring a database into your sql server instance.
This can be done for a variety of reasons, if there is a critical error commited that deletes sensitive data and you do not have a differential backup you could have to restore the entire database.
That scenario has only happened to me a couple of times in a production enviorment, I take care to approve all database updates and create table backups to avoid this situation.
<br>
<br>
For this demo we will be pulling a sample database called <a target="_blank" href="https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&ved=2ahUKEwjksJX49Z7jAhVnRN8KHTmRD6gQFjACegQIBhAB&url=https%3A%2F%2Fgithub.com%2FMicrosoft%2Fsql-server-samples%2Freleases%2Fdownload%2Fadventureworks2012%2Fadventure-works-2012-dw-data-file.mdf&usg=AOvVaw31N3kiGC6iwsbcWwq6gcYP"> Adventure Works DW 2012 </a>.
The download will prompt when you click the link.

<br>
<br>
Now go to your SQL server Management Studio interface, right click Databases.
<br>
<br>
Select Attach Database.
<br>
<br>
![](/assets/img/Restore1.PNG)
<br>
<br>
You will be prompted with the Attach Database dialogue box.
<br>
<br>
![](/assets/img/Restore2.PNG)
<br>
<br>
Click Add... 
<br>
<br>
Now Navigate to where you extracted the Adventure Works MDF file.
<br>
<br>
![](/assets/img/Restore3.PNG)
<br>
<br>
You can remove the LDF from the file selection.
<br>
<br>
![](/assets/img/Restore4.PNG)
<br>
<br>
The default location for mdf files are similar to :
C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\DATA\
<br>
<br>
You can change the location to another drive if you wish.
<br>
<br>
Verify that the current File Path is the actual path to the MDF file.
<br>
<br>
Now Click Ok.
<br>
<br>
Right click on databases and refresh, your new database should be in the database list now!
<br>
<br>
![](/assets/img/Restore5.PNG)
<br>
<br>
![](/assets/img/Restore6.PNG)
<br>
<br>
The process is slightly different from a .bak file which I will cover in another blog post soon.
