---
layout: post
title:  Using your own modules in Python
date:   2019-06-16
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: py.png # Add image post (optional)
tags: [Python, Modules, Import,Custom]

datatable: true
author: # Add name author (optional)
---

In this blog post I will detail how you can abstract your custom defined methods to stand alone python scripts and later import them into another script.
This strategy can help save space in your jupyter notebook and can help clean up logic to make your debugging more easy.
In my experience it is always best to abstract as much as possible, it alleiviates heart aches caused by coding mistakes and can help your development timeline.


<br>
<br>

This repl will show how this is done, essentially you write a python script in this case my_math.py, and define a single function in it.
Then in the master script you use a import statement, as long as that .py script is in the same directory the functions defined within will be accessible.
You will notice also i aliased the import and appended that alias to the function call in main.py. You will need to use this alias when calling your functions in the main script.



<br>
<br>

{% highlight python %}
import my_math as mm

mm.add_nums(2,2)
{% endhighlight %}  

<br>
<br>

Full Example in this repl.
Click the files button in the top left corner to view all the files.

![Repl Explanation](/assets/img/repl1.PNG)
<br>
<br>


<iframe height="400px" width="100%" src="https://repl.it/@IanFogelman/FrillyWellwornDownload?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
