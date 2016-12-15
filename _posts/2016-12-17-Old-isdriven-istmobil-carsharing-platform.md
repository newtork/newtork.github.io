---
layout: post
title:  "Revisited: IsDriven / IstMobil - a carsharing platform"
date:   2016-12-17 07:00:00 +0000
modified: 2016-12-17 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/17/old-isdriven-istmobil-carsharing-platform/
categories: project
tags: website java project demo tomcat isdriven
---

I would like to take the time and present a website project I've worked on together with a couple of good friends up until early 2013. Here, the promising idea was to bundle the most interesting topics of carsharing to one highly interactive website: [IsDriven.com][isdriven]

<!--more-->

This is not supposed to be an open source project contribution, so I'm only going to outline some interesting facts. Feel free to explore the demo [IsDriven.com][isdriven]. I recommend the city selection for *Berlin*, since it is the most diverse.

![start][screen-start] ([zoom][screen-start-link])

## Carsharing rental

A couple of years ago the carsharing industry started to expand in Berlin, Germany. In just a few months, multiple newly founded companies started to put cars onto the streets, which could be rented spontaneously for different rates. The costs are usually calculated by the duration and the distance traveled. But often or not the price is also depending on daytime, weekday, holiday, parking time and car type.

![rank][screen-calc] ![rank][screen-comps]

When the question is asked, which carsharing company you should be subscribed to, an answer isn't easily be found. Any person is using his or her car differently. I for one am renting cars only once in a while when the public transportation does not suffice, especially during night time or when there is a strike or constuction work going on. Since I don't want to be subscribed to every company in my city, I'd need to find the most fitting one.

![rank][screen-rank] ([zoom][screen-rank-link])

This is where our carsharing rates *calculator* comes in handy. It factors in every possible aspect of renting price configurations. The *calculator* is highly customizable for user inputs; it gives detailed price calculations, average numbers and individual company rankings. Here, the [website][isdriven] is only using a subset of possible *calculator* features, but it does it in a suitable manner for visitors on the web.

## Carsharing opportunity

As soon as you are subscribed to more than one carsharing company, you'll face the problem of switching between multiple apps or websites to find a nearest, rentable car. That's why we introduced a combined *map*, with several filtering and visibility options. The *map* features selections for companies, areas of business and rental cars and stations.

![map][screen-map-a] ([zoom][screen-map-a-link])

The *map* includes layers from [OpenStreetMap][osm], an open source, collaborate mapping software, as a great alternative to Google. The markers on the map, so called pinnacles, are dynamically clustered in the backend. Every map panning and zooming yields a custom clustered data set. A single marker gives either a carsharing car or station. A click opens a popup with further information, e.g. car type, capacity, condition.

![map][screen-map-b] ([zoom][screen-map-b-link])



## Carsharing knowledge

Besides the interactive part, we focussed on an accessible *website*. Whether you'd browse with a phone, tablet or personal computer, we wanted to provide the most satisfying experience while reading about the carsharing subject. We had a news blog to gather recent activities on geographical levels like the city, country and world. And there were articles to provide help with rental details, such as liability cases and customer claims.

![intro][screen-intro] ([zoom][screen-intro-link])


### Background

This private software project [IsDriven][isdriven] was created by:

 - Philip Konrad (Idea, Marketing, Organisation)
 - Paul Konschake (Design, Concept, Graphics)
 - Alexander Dümont (Development, Full Stack, Realization)
 - Karl Fischer (Development, Full Stack, Operation)



{% include tags/hint-start.html %}
Back when we started developing, we used `SVN` for version control, `Trac` for documentation, `Struts2` as backend framework under `Java` programming language run in a `Tomcat` application server. We used `H2` as a database service, connected via a `JDBC` driver to the `Hibernate` Entity-Relation-Mapper framework. To manage source code, we applied `Maven`, for blogs and articles it was `WordPress`.
{% include tags/hint-end.html %}

Next to the webside, we implemented a web application for administration purposes. This administration software might be publicly released later in a simplified, abstract version.



[screen-calc]: /content-images/isdriven-calc0.jpg
[screen-comps]: /content-images/isdriven-comps0.jpg
[screen-map-a]: /content-images/isdriven-screenshot-map1.jpg
[screen-map-a-link]: /content-images/isdriven-screenshot-map0.jpg
[screen-map-b]: /content-images/isdriven-screenshot-map3.jpg
[screen-map-b-link]: /content-images/isdriven-screenshot-map2.jpg
[screen-start]: /content-images/isdriven-screenshot-intro1.jpg
[screen-start-link]: /content-images/isdriven-screenshot-intro0.jpg
[screen-rank]: /content-images/isdriven-screenshot-rank1.jpg
[screen-rank-link]:  /content-images/isdriven-screenshot-rank0.jpg
[screen-intro]: /content-images/isdriven-screenshot-intro1.jpg
[screen-intro-link]: /content-images/isdriven-screenshot-intro0.jpg
[isdriven]: http://isdriven.com
[osm]: https://www.openstreetmap.org/