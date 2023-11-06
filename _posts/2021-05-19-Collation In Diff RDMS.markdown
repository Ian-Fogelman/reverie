---
layout: post
title:  Database Collations in Different RDMS
date:   2021-05-19
description: 2021-05-19-Database Collations in Different RDMS
img: sq.png # Add image post (optional)
tags: [RDMS,SQL,Collation,SQL-Concepts]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Database Collations in Different RDMS">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<h3>Understanding Database Collation</h3>

<br>

In a best case scenario, you may never notice your databases collation at all. But it can have a detrimental affect on your databases and user experience if not configured properly. Having collation knowledge ahead of time can help you make higher quality decisions when you are standing up additional database resources. Collation is a decision that you should make early in a new server configuration. Ideally inheriting the default server level collation to all underlying objects would be ideal. Only explicit deviations from that server level collation would need to be considered for individual use cases.

<br>

<h3>Breaking Down Collation</h3>

<br>

Collation can also cause different experiences with regards to querying your data out of your database. Probably the most influential piece of a collation format is the case sensitivity indicator. Lets examine the following collation setting : Latin1_General_CI_AI.

<br>

Lets break down the above collation string :

<br>

Latin1_General  = Character encoding or character set used

CI = Case Insensitive, will dictate if query results will return based on column exact matching. I.E. if your case sensitive "Dog" <> "DoG"

AI = Accent Insensitive, will dictate if accents affect the query results I.E. "ü" equals to "u".

WI = Width Insensitive, although not specified the lack of the WS in the format string dictates width insensitivity.

KS = Kanatype-sensitive, although not specified the lack of the KS in the format string dictates kanatype insensitivity.



<br>

A little more description for Width and Kanatatype.

<br>

Width Sensitive - Distinguishes between the two types of Japanese kana characters:  Hiragana and Katakana. If this option is not selected, SQL Server considers Hiragana and  Katakana characters to be equal for sorting purposes

<br>

Kanatype Sensitive - Distinguishes between a single-byte character and the same character  when represented as a double-byte character. If this option is not selected, SQL Server considers the single-byte  and double-byte representation of the same character to be identical  for sorting purposes.

<br>

<h3> Applying Collation </h3>

<br>

Collation levels can be set at multiple levels, this is agnostic between RDMS but the the general theme is a hierarchy of priority. The hierarchy detailed is as follows : Server > Database > Table > Query, with the caveat of the possibility of schema level collation as well.

<br>

![Model Results](/assets/img/Collation.png)

<br>

