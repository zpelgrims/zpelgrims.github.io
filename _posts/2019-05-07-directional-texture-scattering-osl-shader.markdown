---
layout: post
title:  "On directional texture scattering in OSL"
date:   2019-05-07 11:00:00
---

<img src="{{ site.baseurl }}/assets/img/zenopelgrims_shadersxyz_challenge_10_welding_002.png">
*The burn variation could use some work ...*
{:.caption}

#### INSPIRATION

When creating shaders, sometimes random scattering of textures works like a charm and feels very natural. Other times however, there's a strong directional bias to how the surface features are scattered on an object. A good example is for example a clay shader. Our hands move in very specific ways across the surface in order to shape the sculpture.

The ability to do this procedurally has been on my "this is missing in computer graphics" list for a while.

Recently I found [this](https://hackernoon.com/https-medium-com-matteoronchetti-pointillism-with-python-and-opencv-f4274e6bbb7b) amazingly simple example on procedural pointillism using python/opencv. The really interesting part to me was his ability to align the brushstrokes. So, a little quest started towards implementing a similar technique in an OSL shader that needs to be evaluated millions of times per image (this comes with a few extra challenges!).

#### INPUT

You can basically input anything that defines a region. This could be a curvature map if you want it to flow with the edges. Since I think this is quite a common case, I'll be using this example.

#### IMAGE GRADIENT & ANGLE

The first step is to calculate the image gradients. This is the rate of change from pixel to pixel - a common application of this is edge detection. Lots of great articles have been written on this topic, such as this one: [Gradient vectors](http://mccormickml.com/2013/05/07/gradient-vectors/). In short, by calculating two rates of change per pixel, we can use some trigonometry to determine a vector.





talk about: 

- image gradients
- calculating the direction from the image gradient
- overlapping scattering in OSL (zap)
- future improvements

{% highlight c++ %}
{% endhighlight %}
