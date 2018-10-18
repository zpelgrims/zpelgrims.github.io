---
layout: post
title:  "On accurate computation of the f-stop"
date:   2018-10-31 22:00:00
---

#### the constants

The following table of optical elements, taken from [patent GB157040A](https://worldwide.espacenet.com/publicationDetails/originalDocument?CC=GB&NR=157040A&KC=A&FT=D&ND=3&date=19210120&DB=&locale=en_EP), is standardized to a focal length of 1 inch ($\approx$ 25.4 mm). The patent describes an aperture of f/2.

Sidenote, this lens, the uncoated Cooke Speed Panchro designed by Horace W. Lee in 1920, is incredibly significant in the history of cinema.


| radius  	| thickness 	| material 	| ior    	| abbe 	| housing-radius 	|
|---------	|-----------	|----------	|--------	|------	|----------------	|
| 16.59   	| 2.1234    	| abbe     	| 1.6118 	| 59   	| 6.5            	|
| 76.809  	| 0.35306   	| air      	| -      	| -    	| 6.5            	|
| 11.3309 	| 2.4765    	| abbe     	| 1.6118 	| 59   	| 6              	|
| 100000  	| 1.06172   	| abbe     	| 1.576  	| 41   	| 6              	|
| 7.1856  	| 1.7691    	| air      	| -      	| -    	| 4.65           	|
| infinite  | 1.7691    	| aperture  | -      	| -     | 4.65          	|
| -7.2872 	| 1.06172   	| abbe     	| 1.576  	| 41   	| 4.65           	|
{:.mbtablestyle}
*Note that the housing radii are not usually described in optics literature. In this case it was matched visually by overlaying the patent's lens drawing with my own so small errors are to be expected*
{:.caption}

#### f-number

So, how do we compute the f-stop ourselves, given the constants above?

Using the first equations we find on the internet, it seems straightforward:

$$N={\frac {f}{D}}\$$

where $N$ is the fstop, $f$ the focal length and $D$ the effective aperture radius.
{:.caption}

However, quickly placing in the numbers shows that this can't be the full story:

$$N = {\frac {25.4}{2 * 4.65}}  \approx 2.73$$

Note that we're talking about the **effective** aperture here. This is the aperture as viewed from the sensor, which might be occluded by other lens elements. So, to take that into account we need to do some raytracing: In this case I start tracing parallel rays with increasing height until the ray is blocked by any of the lens elements.

We're now interested in the position on the entry pupil of the last ray that was able to pass as this describes the effective aperture:

<img src="{{ site.baseurl }}/assets/img/1920-cooke-speed-panchro.png">
*In this image I scaled the lens to a focal length of 100mm. This makes it a little bit easier to visualise. If you scale the radius of curvature, thickness and housing radius by the same constant, you can adjust the focal length of the lens by that same constant.*
{:.caption}

Plugging this value into the equation:

$$N = {\frac {100}{2 * 20.5}}  \approx 2.439$$

Closer.. But clearly still incorrect. 

#### numerical aperture

Instead, the following equation for the numerical aperture should be used:

$$\theta = tan^{-1} (\frac{y}{x})$$

$$NA = \frac {1}{2sin(\theta)}$$


Substituting now, this brings us much closer:

$$NA = \frac {1}{2sin(tan^{-1} \frac{20.5}{79.5})} \approx 2.002$$


I think the main idea to take away from this, is that there's an issue regarding the nomenclature regarding this popular subject. The word f-stop is passed around rather carelessly. In optics literature, usually the numerical aperture is used. In photography, the f-number is used. It is incredibly confusing they have the same f/~ notation.

¯\\\_(ツ)\_/¯
{:.caption}

<!-- 
{% highlight c++ %}
  float theta = std::atan(y/x);
  float fstop = 1.0 / (std::sin(theta) * 2.0);
{% endhighlight %}
-->
