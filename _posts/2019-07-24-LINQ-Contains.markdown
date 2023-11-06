---
layout: post
title:  LINQ Contains
date:   2019-07-24
description: A quick look at how to restore a database from a mdf file. # Add post description (optional)
img: cs.png # Add image post (optional)
tags: [C#,LINQ]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<br>
<br>
An example of how to apply a IN() statement to a LINQ query.
Traditionally in SQL you can use the IN() operator to narrow in on a list of Ids or string values.
<br>
<br>

{% highlight c# %}

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace Rextester
{
        public class Movie
    {
        public string Title { get; set; }
        public float Rating { get; set; }

        int _year;
        public int Year
        {   get
            {
                Console.WriteLine("Returning {_year} for {Title}");                
                return _year;
            }
            set
            {
                _year = value;
            }
        }
    };
    
    public class Program
    {
        public static void Main(string[] args)
        {
            var movies = new List<Movie>
            {
                new Movie { Title = "The Dark Knight",   Rating = 8.9f, Year = 2008 },
                new Movie { Title = "The King's Speech", Rating = 8.0f, Year = 2010 },
                new Movie { Title = "Casablanca",        Rating = 8.5f, Year = 1942 },
                new Movie { Title = "Star Wars V",       Rating = 8.7f, Year = 1980 }                        
            };

            var query = from movie in movies
                        where movie.Year > 2000
                        orderby movie.Rating descending
                        select movie;

            //Extenstion method
            var query2 = movies.Where(m => m.Year > 2000);

            foreach (var movie in query2)
            {
                Console.WriteLine(movie.Title);
            }

            var enumerator = query.GetEnumerator();
            while (enumerator.MoveNext())
            {
                Console.WriteLine(enumerator.Current.Title);
            }
        }
    }
}
{% endhighlight %}   

https://rextester.com/QOW66617
