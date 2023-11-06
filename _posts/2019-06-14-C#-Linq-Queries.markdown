---
layout: post
title:  C# Linq Queries
date:   2019-06-14 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img:  cs.png #lamp.jpg #greengrad.png # Add image post (optional)
tags: [c#, Linq]

datatable: true
author: # Ad 431394E5-6F9C-443C-B24F-5D6518D0FA05 https://rextester.com/KSVGS52268
---

A super valueable skill that is worth knowing and practicing on a weekly basis is Language Integrated Query or LinQ statements. 
Linq statements can be defined in c# and extract data from various object types. The flexibility of being able to apply 
a uniform language to extract data to different object types is what makes linq so valuable.

<br>
<br>

The example for this post can be found <a href="https://rextester.com/KSVGS52268" target="_blank"> here </a>.

<br>
<br>


We have two arrays of strings, the first has strings the word version of 1 - 10, the second has a-z as characters.

{% highlight csharp %}
String[] QueryString = {"One","Two","Three","Four","Five","Six","Seven","Eight","Nine","Ten"};
String[] IndexArray = {"A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"};
{% endhighlight %}  

<br>
<br>

A typical linq query may look like this

<br>
<br>

{% highlight csharp %}
var ThisQuery = 
	from StringValue in QueryString
{% endhighlight %} 

<br>
<br>

Additional clauses similar to SQL clauses can be applied to linq queries, although many keys words are the same as in SQL there are several drastic differences in Linq.

<br>
<br>

You can apply custom methods to result sets, use logic to narrow in on the selectivity your after, define ordering and much more in linq. As a simple example I will join the two string arrays together on the matching first letter of each word in the <strong>QueryString</strong> array and print the results.

<br>
<br>

{% highlight csharp %}

String[] QueryString = {"One","Two","Three","Four","Five","Six","Seven","Eight","Nine","Ten"};

String[] IndexArray = {"A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"};

var ThisQuery = 
	from StringValue in QueryString
	join IndexValue in IndexArray
	on StringValue.Substring(0, 1) equals IndexValue
	where Convert.ToChar(IndexValue) > 'F' //Added Where Filter here...
	orderby IndexValue //Order By Clause must be written as orderby (lowercase)
	select new {StringValue, IndexValue};
	
foreach (var ThisValue in ThisQuery)
		Console.WriteLine(ThisValue.IndexValue + " - " + ThisValue.StringValue + "\r\n");
    
{% endhighlight %} 
