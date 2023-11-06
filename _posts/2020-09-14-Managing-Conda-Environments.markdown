---
layout: post
title:  Managing Conda Environments for Jupyter Notebooks
date:   2020-09-14
description: How to build and load conda environments in Jupyter notebooks
img: AN.png # Add image post (optional)
tags: [Anaconda,conda,Environments]
markdown: kramdown
datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Managing Conda Environments for Jupyter Notebooks">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Today we take a look at anaconda, not only is anaconda a flavor of Python distribution but it also a powerful environment management tool.

<br>
<br>

I am well aware that you can create and manage Python environment and install packages into virtual Python environments.
Recently however the need for me to run my Jupyter notebook instance in a different version of Python arose.
This can happen do to a particular package not being updated or compaitable with the most recent versions of Python.
In my case this conflict was caused from the difference in Python 3.7 -> 3.8.

<br>
<br>

Now I knew that I could just download the previous version of Anaconda so that Python would be 3.7, but this is not the correct answer!
There is a lot of misleading information or incomplete steps into loading the Anaconda environment so that your jupter notebook instance can run it.
Today I detail how this can be accomplished step by step.

<br>
<br>

In this example I am starting in Python 3.7 as my base or root environment, I will build a conda environment of 3.6 and emulate that Python version in jupyter notebook.

<br>
<br>

First these commands can help show current anaconda environments and kernels for Jupyter notebooks.

<br>
<br>

{% highlight Cmd %} 
conda env list
{% endhighlight %}
<br>
{% highlight Cmd %} 
jupyter kernelspec list
{% endhighlight %}

<br>
<br>

Step 1 - Create the Anaconda Environment

<br>
<br>

![](/assets/img/MCE1.png)
<br>
<br>
{% highlight Cmd %} 
conda create -name Python_36 python==3.6
{% endhighlight %}

<br>
<br>

This creates a Anaconda virtual environment in the following directory: C:\Users\XXX\Anaconda3\envs
<br>
<br>
![](/assets/img/MCE2.png)
<br>
<br>

Step 2 - Activate new environment and install helper modules
<br>
<br>
activate Python_36
<br>
<br>
{% highlight Cmd %} 
conda install nb_conda
{% endhighlight %}
{% highlight Cmd %} 
conda install ipykernel
{% endhighlight %}
<br>
<br>

Step 3 - Add virtual environment to Jupyter:
<br>
<br>
Inside of a CMD prompt or terminal window :
<br>
<br>
{% highlight Cmd %} 
python -m ipykernel install --user --name=Python_36
{% endhighlight %}
<br>
<br>

Now restart your jupyter notebook instance by closing the current CMD that is running it and issue "jupyter notebooks" command.

<br>
<br>
You should notice two additional kernels available in your dropdown.
One called Python_36 and Python[conda env:Python_36]

<br>
<br>

![](/assets/img/MCE3.png)

<br>
<br>

Step 4 - Inside Jupyter notebooks lets create a new notebook under then Python_36 kernel.
<br>
<br>

Run the following command to get the version of Python running for that particular notebook.
<br>
<br>

![](/assets/img/MCE4.png)
<br>
<br>

{% highlight Python %} 
import platform
print(platform.sys.version)
{% endhighlight %}
<br>
<br>
You should see a 3.6.XX version of Python returned.
If you see the same version as your base environment try the Python[conda env:Python_36].
<br>
<br>
Now create another Notebook under Python3 and run the same command.
You should see the base Python version, in my case 3.7.6
<br>
<br>
![](/assets/img/MCE5.png)
<br>
<br>
Optionally we may want to clean up unused kernels in both Jupyter notebooks and Anaconda.

<br>
<br>

Step 5 - (Optional) Kernel and Env cleanup
<br>
<br>

If you ever want to remove a kernel from the jupter notebooks, use the following command:
{% highlight Cmd %} 
jupyter kernelspec uninstall xxxx
{% endhighlight %}
<br>
<br>

To remove an envirnment from Anaconda :
{% highlight Cmd %} 
conda env remove -n env_name
{% endhighlight %}
<br>
<br>

In this post we covered how to create multiple conda Python environments and flip between the two.
We extended that functionaility to Jupyter notebooks so that we can get other package congruenty if a package does not support a specific version of Python.
Lastly we reviewed how to remove a Python environment if we no longer need it in both the Jupyter notebook kernels and Anaconda.

On a closing note you can also view all the current environments and packages created from the command line in Anaconda navigator.

![](/assets/img/MCE6.png)



