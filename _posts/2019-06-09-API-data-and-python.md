---
layout: post
title:  API data with Python
categories: [Python, API]
---

Howdy partner, today I will discuss how to retreive rest API data using python.

In particular we will make use of the following libraries:

requests,json and pandas all of the code in this post can be downloaded on my cloud anaconda page <a href="https://anaconda.org/IanFogelman/api-data-and-python/notebook" target="_blank">here</a>.
    
First lets import our libraries, if you do not have these libraries, use the !pip install command to install them.

<br>

{% highlight python %}
import requests
import json
import pandas as pd
{% endhighlight %}  

<br>


For this demo we will use a free API that is hosted at http://api.icndb.com.
This particular API returns jokes about the famous western and kungfu guru Chuck Norris.
At its core we can achieve a simple get request as follows :

<br>


{% highlight python %}
url = """http://api.icndb.com/jokes/random?"""
result = requests.get(url)
print(result.text)
#{ "type": "success", "value": { "id": 467, "joke": "Chuck Norris can delete the Recycling Bin.", "categories": ["nerdy"] } }
{% endhighlight %}  

<br>

This is pretty good in terms of basic functionality and concepts, but what if we only care about the joke itself?
We can see it is nested inside the value portion of the return dictionary. For this we can enlist the help of the JSON library.

<br>

{% highlight python %}
url = """http://api.icndb.com/jokes/random?"""
r = requests.get(url)
print(result.text)
j=json.loads(r.text)
print(j['value']['joke'])
#Chuck Norris breaks RSA 128-bit encrypted codes in milliseconds.
{% endhighlight %}  


Here we get a more appealing result just the actual joke text returned from the API.

<br>
<br>

Lastly lets see if we can get this results into a pandas dataframe.
I will write a for loop to call a GET request 5 times, parse the joke portion of the returned json data and append that to our dataframe.

<br>
<br>

{% highlight python %}

pd.set_option('display.max_colwidth', -1)
df = pd.DataFrame() #creates a new dataframe that's empty
url = """http://api.icndb.com/jokes/random?"""

for x in range(5):
    r = requests.get(url)
    j=json.loads(r.text)
    myjson = j['value']['joke']
    Tempdf = pd.DataFrame([myjson])
    df = df.append(Tempdf, ignore_index=True)
df.columns = ['Joke']
df
{% endhighlight %}  