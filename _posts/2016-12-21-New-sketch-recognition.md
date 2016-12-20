---
title:  "New: Sketch Recognition"
date:   2016-12-21 07:00:00 +0000
modified: 2016-12-21 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/21/new-sketch-recognition/
categories: project 
tags: javascript css html project demo dtw subsequence
---

Two years ago I worked on a subsequence matching algorithm, to solve a difficult monitoring problem with tight algorithmic time and space complexity requirements. I went back to restart documenting this case. Now, instead of publishing my old results, I decided to make up a new challenge, to test and improve my findings. I created this task around a little web project, in order to show and use it as an interactive example: [Sketch Recognition][sketch].

<!--more-->

First of all, feel free to look into the [demo][demo].

![eight][image8]


## Task

*Create an interactive website to arbitrarily draw lines and curves. An algorithm shall recognize predefined forms and shapes while they are being drawn. It should give assumptions on known figures and words while they are being written.*

Requirement:

 - *A figure can be freely drawn in a different size and varying rotation.*
 - *For every additional point being drawn, use* `O(c)` *space/time, where* `c=const`


## About the solution

I'm not able give you a finished solution yet. This might happen during the upcoming weeks and months. There are some extensive routines to implement, even with my previous project in mind.


### Tackle the requirements

Of course without the additional requirement you could simply use 2d coordinate system to save and compute polygons in. But infact the requirement makes this task so interesting.


#### Angular velocity

So instead of operating on coordinates, I decided to calculate the *angular velocity* at every point during drawing. It describes the change of the cursor's movement angle, not over time, but over the distance passed. This leads to some advantages:

 - ignorable coordinate starting location
 - ignorable initial and continous scaling
 - ignorable initial and continous rotation
 - substituted plane coordinates to discrete angular information
 - numerical reduction of input data dimensions, from `2` to `1`
 - thus more accessible to *classic* sequence algorithms

On the side of disadvantages there are:
 - unable to retrieve the original image with correct rotation and scale
 - most characters are appearing very similar when obsevering the *angular velocity*, e.g. `6`, `0`, `c` and `o`.

![descr][angular]

 
#### Subsequence Matching

As of today, the [demo][demo] shows matching sequences by using an *online* [DTW][dtw-wiki] variant, which does not save any matrices but two vectors of constant sizes for every pattern. This is the most basic approach to give a decision on matchable figures.

I recorded the single numbers, some words and a figure with various success rates. Since the algorithm is not trained or configured by any means, it will often give false-positives. But even in its primitive state it works surprisingly well on some of the characters. There will be much greater achievements, once additional features have been implemented: *thresholds, conditionings, graduations and associations*.

Look out for some upcoming posts.

![one][image1]

![two][image2]

![four][image4]

[sketch]: https://github.com/newtork/sketch-recognition
[demo]: http://newtork.de/sketch-recognition/01-userinput-canvas/canvas.html
[dtw-wiki]: https://en.wikipedia.org/wiki/Dynamic_time_warping
[image1]: /content-images/sketch04.png
[image2]: /content-images/sketch02.png
[image4]: /content-images/sketch03.png
[image8]: /content-images/sketch01.png
[angular]: /content-images/sketch00.png