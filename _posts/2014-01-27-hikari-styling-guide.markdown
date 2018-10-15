---
layout: post
title:  "On the correct computation of the f-stop"
date:   2018-10-14 22:00:00
---

#### the correct way


{% raw %}
  $$a^2 + b^2 = c^2$$
{% endraw %}

{% highlight c++ %}
float theta = std::atan(positiondata[1] / positiondata[0]);
float fstop = 1.0 / (std::sin(theta)* 2.0);
{% endhighlight %}
