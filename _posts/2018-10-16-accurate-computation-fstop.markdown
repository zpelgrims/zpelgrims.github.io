---
layout: post
title:  "On accurate computation of the f-stop"
date:   2018-10-16 22:00:00
---

#### TODO

- swap out table for lens that accurately describes the fstop
- make some nice drawings
- write styling for small captions

#### the constants

The following table of optical elements, taken from [patent 2701982](https://patentimages.storage.googleapis.com/2f/75/dd/b76ccb73bc0f44/US2701982.pdf), is standardized to a focal length of 100 millimeters. The patent describes an fstop of f/1.15.

| curvature radius 	| thickness 	| mat   	| index  	| vno   	| radius   	|
|-------------	|-----------	|-------	|--------	|-------	|------	|
| 164.13000   	| 10.99     	| SF5   	| 1.6751 	| 32.3  	| 54   	|
| 559.20000   	| 0.23      	| air   	|        	|       	| 54   	|
| 100.12000   	| 11.45     	| BAF10 	| 1.6689 	| 46.70 	| 51   	|
| 213.54000   	| 0.23      	| air   	|        	|       	| 51   	|
| 58.04000    	| 22.95     	| LAK9  	| 1.6913 	| 53.8  	| 41   	|
| 2551.10000  	| 2.58      	| SF5   	| 1.6751 	| 32.3  	| 41   	|
| 32.39000    	| 15.66     	| air   	|        	|       	| 27   	|
| 10000.00000 	| 15        	| aperture  	|        	|       	| 25.5 	|
| -40.42000   	| 2.74      	| SF15  	| 1.6992 	| 30.2  	| 25   	|
| 192.98000   	| 27.92     	| SK16  	| 1.6204 	| 60.2  	| 36   	|
| -55.53000   	| 0.23      	| air   	|        	|       	| 36   	|
| 192.98000   	| 7.98      	| LAK9  	| 1.6913 	| 53.8  	| 35   	|
| -225.30000  	| 0.23      	| air   	|        	|       	| 35   	|
| 175.09000   	| 8.48      	| LAK9  	| 1.6913 	| 53.8  	| 35   	|
| -203.55000  	| 55.742    	| air   	|        	|       	| 35   	|
{:.mbtablestyle}
*Note that the housing radii are not usually described in optics literature. In this case it was matched visually by overlaying the patent's lens drawing with my own so small errors are to be expected*

#### the incorrect way

So, how do we compute the f-stop ourselves, given the constants above?

Using the first equations we find on the internet, it seems simple:

$$N={\frac {f}{D}}\$$

where $N$ is the fstop, $f$ the focal length and $D$ the effective aperture radius.

However, quickly placing in the numbers shows that this can't be the full story:

$$N & = {\frac {100}{2 * 25.5}}  \approx 3.92$$

Note that we're talking about the **effective** aperture here. This is the aperture as viewed from the sensor, which might be occluded by other lens elements. So, to take that into account we need to do some raytracing: In this case I start tracing parallel rays with increasing height until the ray is blocked by any of the lens elements.

We're now interested in the last vertex position (position on the entry pupil) of the last ray that was able to pass as this describes the effective aperture:

## insert image

Plugging this value into the equation:

$$N & = {\frac {100}{2 * 29.0}}  \approx xxx$$

Closer.. But still no cigar. Instead, the following equation should be used:

$$\theta = tan^-1 (\frac{y}{x})$$

$$fstop = \frac {1}{2sin(\theta)}$$

Substituting now, this brings us much closer:

$$fstop = \frac {1}{2sin(tan^-1 \frac{29.0}{58.0}} \approx 1.11$$

<!-- 
{% highlight c++ %}
  float theta = std::atan(y/x);
  float fstop = 1.0 / (std::sin(theta) * 2.0);
{% endhighlight %}
-->
