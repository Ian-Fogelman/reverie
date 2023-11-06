---
layout: post
title:  Working With custom objects and Soup
date:   2019-08-07
description: Beautifulsoup and custom object
img: py.png # Add image post (optional)
tags: [Python,Requests,Beautifulsoup,OOP]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

Today we take a look at creating custom objects in Python.
<br />
https://anaconda.org/IanFogelman/working-with-python-classes-and-soup/notebook
<br />
<br />

A high level discussion on why to use an OOP (object orientated programming) approach when working with data can be summarized as follows. In programming abstracting as much as possible should be your over all goal, programming for implementation is a recipe for dis·as·ter. When you program for any type of solution it can be tempting to only solve for the requirements of your project. What you should also consider is, is this code reusable? If I am programming a web application to send birthday card emails, can I just as easy use the application to send christmas card emails? Halloween card emails? 
<br />
<br />

The point is you will often be under some type of time pressure to complete task X, what you have to remember is do not code for that particular implementation of the project. Take the extra time and critically analyze your program, extend it so that it make be extended. Abstract your logic into classes and objects that can be reused at a later date, you will be glad you did in the future.
<br />
<br />

Here is an example of the table we will scrape:

<br />
<br />

![](/assets/img/TableScrapeTable.PNG)

<br />
<br />

Now here is an example in Python of how to scrape data and use those scrapes to construct a consistently modeled object called TableRow.
<br />
<br />


<br />
<br />
In this demo we will scrape each td in each row of the table, create a temporary object with that data then append the temporary object to a list. Then we can iterate over that list of objects and access our data in a consistent and organized manner.

<br />
<br />

<hr>
{% highlight Python %}

from bs4 import BeautifulSoup
import urllib.request
import nltk
import matplotlib.pyplot as plt
import pyodbc
import re
import requests
import pandas as pd
from datetime import datetime



TableRowList = []
class TableRow:
    def __init__(self,OrderDate,Region,Rep,Item,Units,UnitCost,Total):
        self.OrderDate = OrderDate
        self.Region = Region
        self.Rep = Rep
        self.Item = Item
        self.Units = Units
        self.UnitCost = UnitCost
        self.Total = Total
        self.ScrapeTime = str(datetime.now().strftime('%Y-%m-%d %H:%M %p'))




r =requests.get('https://www.contextures.com/xlSampleData01.html')
soup = BeautifulSoup(r.text,'lxml')
table = soup.find('table')
rows = table.find_all('tr')
for row in rows:
    data = row.find_all('td')
    OD = data[0].text.strip()
    Region = data[1].text.strip()
    Rep = data[2].text.strip()
    Item = data[3].text.strip()
    Units = data[4].text.strip()
    UnitCost = data[5].text.strip()
    Total = data[6].text.strip()
    t = TableRow(OD,Region,Rep,Item,Units,UnitCost,Total)
    TableRowList.append(t)



i = 1
for x in TableRowList:
    if(i <= 5):
        print(x)
    i += 1


{% endhighlight %}
