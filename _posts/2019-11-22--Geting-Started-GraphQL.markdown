---
layout: post
title:  Getting Started With GraphQL
date:   2019-11-22
description: GraphQL getting started
img: sq.png # Add image post (optional)
tags: [GraphQL]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Getting Started With GraphQL">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

A simple example of a what graphQL is.
<br>
<br>

The best way to think of what GraphQL is, just think of it as short hand expressions to query Restful APIs.
<br>
<br>
This is achieved by applying an abstract layer, aka GraphQL server layer between your API endpoints and clients.

<br>
<br>
![RestGraphQL](/assets/img/RestGraphQL.PNG)
<br>
<br>
Now not all API's inheritly support graph QL, but there is an extensive list of example api's here : <a href="https://github.com/APIs-guru/graphql-apis" target="_blank">https://github.com/APIs-guru/graphql-apis</a> .

<br>
<br>

Yelp GraphQL documentation : <a href="https://www.yelp.com/developers/graphql/guides/intro" target="_blank">https://www.yelp.com/developers/graphql/guides/intro</a>

<br>
<br>
You can also code up your own, but for this example I will be using the yelp GraphQL service.
In the example below I put the restful API that a subquery would come from. Notice that instead of querying an entire API end point and joining that data all back together you can write the shorthand graphQL query (essentially JSON format) to extract your data. The idea is efficiency behind your efforts, doing more with less code.

<br>
<br>

{% highlight graphQL %}

#https://api.yelp.com/v3/businesses/search
#https://www.yelp.com/developers/documentation/v3/business_search
{
    business(id: "garaje-san-francisco") {
        name
        id
        alias
        rating
        url
        phone
        distance
        is_closed

        location {
          address1
          address2
          address3
          city
          state
          postal_code
          country
          formatted_address
        }
    
#https://api.yelp.com/v3/transactions/{transaction_type}/search
    transactions
    {
      restaurant_reservations {
        url
      }
    }
    
#https://api.yelp.com/v3/businesses/{id}/reviews
    reviews{
      rating
      text
      user{
        profile_url
      }
    }
    
    }  
}

{% endhighlight %}



This will produce a JSON results of :
{% highlight json %}
{
  "data": {
    "business": {
      "name": "Garaje",
      "id": "tnhfDv5Il8EaGSXZGiuQGg",
      "alias": "garaje-san-francisco",
      "rating": 4.5,
      "url": "https://www.yelp.com/biz/garaje-san-francisco?adjust_creative=J6an7peKQCWF8-n_h2R2Vg&utm_campaign=yelp_api_v3&utm_medium=api_v3_graphql&utm_source=J6an7peKQCWF8-n_h2R2Vg",
      "phone": "+14156440838",
      "distance": null,
      "is_closed": false,
      "location": {
        "address1": "475 3rd St",
        "address2": "",
        "address3": "",
        "city": "San Francisco",
        "state": "CA",
        "postal_code": "94107",
        "country": "US",
        "formatted_address": "475 3rd St\nSan Francisco, CA 94107"
      },
      "transactions": {
        "restaurant_reservations": null
      },
      "reviews": [
        {
          "rating": 5,
          "text": "My favorite taco stop in soma is Garaje! The door is a bit tricky to find initially as it's set back a bit from the sidewalk, but once you're in, you're...",
          "user": {
            "profile_url": "https://www.yelp.com/user_details?userid=c_XBoNWA2RRaKuz40ZNWuA"
          }
        },
        {
          "rating": 5,
          "text": "I've eaten here multiple times over the course of the last 5 years and almost every time I've ordered their Zapato which is delicious. It's like a pressed...",
          "user": {
            "profile_url": "https://www.yelp.com/user_details?userid=gkEQ-omRKEDrSXSGzoD0tQ"
          }
        },
        {
          "rating": 5,
          "text": "On strong insistence from my friend who is a foodie and has a history of giving strong advice, I was at Garaje in about 2 hours of landing at SFO, and left...",
          "user": {
            "profile_url": "https://www.yelp.com/user_details?userid=KoAqxqmXzpiObHBxmXDs-Q"
          }
        }
      ]
    }
  }
}
{% endhighlight %}
