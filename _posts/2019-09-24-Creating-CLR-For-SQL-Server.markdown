---
layout: post
title:  Creating C# CLR's For SQL Server
date:   2019-09-24
description: An Example of creating a CLR to be utalized in SQL Server
img: sq.jpg # Add image post (optional)
tags: [SQL, CLR, Management Studio]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

Before we begin the lesson let me touch on breifly what this post addresses.
<br>
<br>

In today's lesson I am addressing how to run c# code in a SQL server enviorment.
<br>
<br>
This is accomplished by something called Common Language Runtimes or CLR's.
<br>
<br>
These CLR can come in flavors or Visual basic or C# code, today I will demo the C# flavor.
<br>
<br>
Because it is C# it gives a wide range of options in terms of the .Net frame work and limitless abilities to bring in packages for an immense level of flexibility.
<br>
<br>

Today I will be using a sentiment parser called Vader Sharp that will return a sentiment score of a text string.
<br>
<br>
This will be turned into a SQL server function and will be able to have SQL server data passed as a parameter and a data type of float return which is a number between -1 and 1 representing the sentiment of the text.
<br>
<br>
Lets Begin!
<br>
<br>

Before we begin run the following code to prepare your SQL enviorment for CLR integration.
<br>
<br>
{% highlight SQL %}

EXEC sp_configure 'clr enabled', 1;  RECONFIGURE WITH OVERRIDE;

ALTER DATABASE Debug SET TRUSTWORTHY ON;

{% endhighlight %}



First Create the correct type of project...
<br>
<br>
![CLR Project Type](/assets/img/CLR1.png)
<br>
<br>
Next add the following dependencies via the nuget package manager....
<br>
<br>
Tools >> Nuget Package Manager >> Manage Nuget Packages for Solution...
<br>
<br>

![CLR Project Type](/assets/img/CLR2.png)

Next paste this code into the project.

{% highlight csharp %}
using Microsoft.SqlServer.Server;
using System.Data.SqlClient;
using VaderSharp;

public class Sentiment
{
    [SqlFunction(DataAccess = DataAccessKind.Read)]
    public static double ParseSentiment(string input)
    {
        SentimentIntensityAnalyzer analyzer = new SentimentIntensityAnalyzer();
        var results = analyzer.PolarityScores(input);
        return results.Compound;

    }
}

{% endhighlight %}

On the visual studio ribbon click build.

Make a note of the build location on your file system, we will need this for the T-SQL code to create the assembly that references the .dll that was just created.

Now that we have the dll built, we can call this function that will run the C# code from SQL server, to do this we must first create the assembly, then create a database object to reference that external assembly. This can be any type of database object ( stored procedure, table value function, scalar function) in this example I am using a scalar function.

{% highlight SQL %}

CREATE ASSEMBLY SentimentParser from 'C:\Users\XXX\source\repos\CLRFunc\CLRFunc\bin\Debug\CLRFunc.dll' WITH PERMISSION_SET = SAFE  
GO 

CREATE FUNCTION ParseSentiment(@input NVARCHAR(MAX)) 
RETURNS FLOAT   
AS EXTERNAL NAME SentimentParser.Sentiment.ParseSentiment; 
--           ASSEMBLY.CLASS___.METHOD
GO  


{% endhighlight %}

<br>
<br>

![Function Results](/assets/img/CLR3.png)

<br>
<br>
As you can see we can now return data using .Net and C# code just like we would a regular SQL Server function!
<br>
<br>

Some common hangups when dealing with CLR's are the data type matchings between the CLR and Sql Server.
<a href="https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping" target="_blank"> Here is a conversion chart </a> to assist with that.


