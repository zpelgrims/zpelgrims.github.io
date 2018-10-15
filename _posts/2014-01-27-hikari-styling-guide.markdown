---
layout: post
title:  "On the correct computation of the f-stop"
date:   2018-10-14 22:00:00
---

#### the incorrect way

Recently I noticed that the f-stop calculations of [pota](www.github.com/zpelgrims/pota) were off.


$$f-number={\frac {focal length}{effective aperture}}\$$


#### the correct way



{% raw %}
  $$\theta = tan^-1 \fraq{x}{y}$$
  $$fstop = \fraq {1.0}{sin(2\theta)}
{% endraw %}

{% highlight c++ %}
float theta = std::atan(positiondata[1] / positiondata[0]);
float fstop = 1.0 / (std::sin(theta)* 2.0);
{% endhighlight %}
