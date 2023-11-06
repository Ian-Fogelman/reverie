---
layout: post
title:  Syncing an Entire S3 bucket to local
date:   2021-06-13
description: 2021-06-13-Syncing an Entire S3 bucket to local
img: AWS.png # Add image post (optional)
tags: [AWS,S3]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Syncing an Entire S3 bucket to local">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<h3>Syncing an entire S3 bucket to your local machine</h3>

Recently I knew for a fact that I would be loosing one of my AWS accounts and I had a specific bucket which held some files that were near and dear to my heart. I offloaded the files into S3 knowing that they would be safe and sound and would not cluster up any local storage on my PC. All said these particular files were large video files which came to a total of about 150 GB. 





<br>



<h3>Listing all available buckets to current account configured in the AWS CLI.</h3>

<br>

As a refresher, you can view all the S3 buckets you have by running the following command :



<br>

{% highlight cmd%}

aws s3api list-buckets

{% endhighlight %}

<br>



<h3>Viewing the total size and number of objects in a bucket</h3>

<br>



To view the total size and number of objects in a bucket, navigate to the "Metrics" tab of your specific bucket and you will be able to view this data :

<br>



![Model Results](\assets\img\S3_Metrics.PNG)





<br>

So I wanted to figure out how can I get all of these files which have several different folders inside a single S3 bucket to my local machine in the fastest way possible? 

<br> 

The answer is with the s3 sync https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html command, this command will allow you to take the remote files in a s3 location and write them to your local system.

<br>



<h3>Demo</h3>

<br>



For this example I created a folder on my C/: called Test which will be the location I pull my files into.

<br>

Open your favorite command line tool and change the working directory to the target you would like.

Next fire the aws s3 sync command with the target s3 bucket you are wanting to pull files from. The . in this context simply specifies to pull all remote files to the current directory, in this case C:\Test.



<br>

{% highlight cmd%}

cd C:\Test

aws s3 sync s3://MyTargetBucket .

{% endhighlight %}



<br>

When you run this command you may be flooded with messages into your command line interface, but it will look similar to this:



![Model Results](\assets\img\AWS_S3_Sync_EX.PNG)

<br>

Giving you an indication of the process of the sync and exactly which files in the batch are being moved over.



<br>



And that's it, when the command is done, you will have all the files in that S3 bucket on your local machine!



