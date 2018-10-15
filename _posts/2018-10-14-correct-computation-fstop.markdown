---
layout: post
title:  "On the correct computation of the f-stop"
date:   2018-10-14 22:00:00
---

The following table of optical elements, taken from [patent 2701982](https://patentimages.storage.googleapis.com/2f/75/dd/b76ccb73bc0f44/US2701982.pdf) is standardized to a focal length of 100 millimeters. The patent describes an fstop of

| curvature radius 	| thickness 	| mat   	| index  	| vno   	| radius   	|
|-------------	|-----------	|-------	|--------	|-------	|------	|
| 164.13000   	| 10.99     	| SF5   	| 1.6751 	| 32.3  	| 54   	|
| 559.20000   	| 0.23      	| air   	|        	|       	| 54   	|
| 100.12000   	| 11.45     	| BAF10 	| 1.6689 	| 46.70 	| 51   	|
| 213.54000   	| 0.23      	| air   	|        	|       	| 51   	|
| 58.04000    	| 22.95     	| LAK9  	| 1.6913 	| 53.8  	| 41   	|
| 2551.10000  	| 2.58      	| SF5   	| 1.6751 	| 32.3  	| 41   	|
| 32.39000    	| 15.66     	| air   	|        	|       	| 27   	|
| 10000.00000 	| 15        	| IRIS  	|        	|       	| 25.5 	|
| -40.42000   	| 2.74      	| SF15  	| 1.6992 	| 30.2  	| 25   	|
| 192.98000   	| 27.92     	| SK16  	| 1.6204 	| 60.2  	| 36   	|
| -55.53000   	| 0.23      	| air   	|        	|       	| 36   	|
| 192.98000   	| 7.98      	| LAK9  	| 1.6913 	| 53.8  	| 35   	|
| -225.30000  	| 0.23      	| air   	|        	|       	| 35   	|
| 175.09000   	| 8.48      	| LAK9  	| 1.6913 	| 53.8  	| 35   	|
| -203.55000  	| 55.742    	| air   	|        	|       	| 35   	|
{}:.mbtablestyle}
> note that the radius is not usually described in optics patents. In this case it was matched visually to the lens drawing. 

focal length of 100
aperture of 1.15

#### the incorrect way

using the first equations we find on the internet, it seems simple.

$$f-number={\frac {focal length}{effective aperture}}\$$

but what do we find? it's off..


#### the correct way

First, we need to do some brute-force raytracing to find out the effective aperture of the entry pupil.

{% raw %}
  $$\theta = tan^-1 (\frac{y}{x})$$

  $$fstop = \frac {1}{2sin(\theta)}$$
{% endraw %}

{% highlight c++ %}
  float theta = std::atan(y/x);
  float fstop = 1.0 / (std::sin(theta) * 2.0);
{% endhighlight %}
