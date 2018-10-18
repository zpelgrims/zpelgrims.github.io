---
layout: post
title:  "On installing visual studio command line tools on a build server"
date:   2018-10-22 22:00:00
---

When writing shaders, I often need to compile them for various operating system flavors. Windows is always the sore thumb of the lot, so I spent some time finding a better way to deal with this.

Instead of emulating windows on linux/macOS, I rent a windows server instead (at [vultr.com](www.vultr.com)).

Open up cmdprompt.exe

Install **Chocolatey** (a life-saving package manager for windows - *who knew the time would come!?*)

{% highlight bat %}
  @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
{% endhighlight %}

Install the visual studio build tools:

{% highlight bat %}
    choco install "visualstudio2017buildtools" --yes --passive
    choco install "visualstudio2017-workload-vctools" --yes --passive
{% endhighlight %}

Then we need to load the visual studio command line tools environment:
{% highlight bat %}
    "C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
{% endhighlight %}

**Et voila**, a build environment set up with minimal effort. All that needs to be done now is to compile whatever you needed to compile, zip it up and send it back to your local machine.