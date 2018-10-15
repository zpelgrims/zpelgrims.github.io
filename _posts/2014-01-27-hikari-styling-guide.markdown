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
  $$\theta = tan^-1 (\frac{y}{x})$$
  $$fstop = \frac {1}{2sin(\theta)}$$
{% endraw %}

{% highlight c++ %}
float theta = std::atan(y/x);
float fstop = 1.0 / (std::sin(theta) * 2.0);
{% endhighlight %}
