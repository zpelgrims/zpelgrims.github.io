---
layout: post
title:  "On installing visual studio command line tools on a build server"
date:   2018-10-22 22:00:00
---

When writing c++ shaders, I often need to compile them for various operating system flavours. Windows is always the sore thumb of the lot, so I spent some time finding a better way to deal with this.

Instead of emulating windows on linux/macOS, I rent a windows server instead (at [vultr.com](www.vultr.com)).

Launch a command prompt:

Install **Chocolatey** (a life-saving package manager for windows - *who knew the time would come!?*)
{% highlight bat %}
  @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
{% endhighlight %}

Install the visual studio build tools:
{% highlight bat %}
    choco install "visualstudio2017buildtools" --yes --passive
    choco install "visualstudio2017-workload-vctools" --yes --passive
{% endhighlight %}

And load the visual studio command line tools environment (64bit in this case):
{% highlight bat %}
    "C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
{% endhighlight %}

**Et voila**, a build environment has been set up with minimal effort. All that needs to be done now is to compile whatever you needed to compile, zip it up and send it back to your local machine.

---

Apart from these packages, I also install these tools before launching into the VS command line environment:

The .net framework (Pathed requirement):
{% highlight bat %}
    choco install dotnet3.5 --yes
{% endhighlight %}

wget - to download e.g the Arnold API (I uploaded the relevant API's to a Dropbox for easy access):
{% highlight bat %}
    choco install wget --yes
{% endhighlight %}

Pathed - a tool for easy adding to the {% highlight %}%PATH%{% endhighlight %}{:.inlined} envvar. I dislike {% highlight %}setx{% endhighlight %}{:.inlined} since it concatenates after 1024 characters like it's 1975.
{% highlight bat %}
    wget http://p-nand-q.com/download/gtools/gtools-current.exe
    C:\lentil-build\tools\gtools-current.exe
{% endhighlight %}

A tool to refresh the current environment, so the additions to {% highlight %}%PATH%{% endhighlight %}{:.inlined} are loaded:
{% highlight bat %}
    wget https://github.com/chocolatey/choco/raw/master/src/chocolatey.resources/redirects/RefreshEnv.cmd
{% endhighlight %}

Git:
{% highlight bat %}
    choco install git -params '"/GitAndUnixToolsOnPath"' --yes
    "C:\Program Files (x86)\p-nand-q.com\GTools\pathed" /APPEND "C:\Program Files\Git\bin" /MACHINE
{% endhighlight %}

7zip:
{% highlight bat %}
    choco install 7zip --yes
    "C:\Program Files (x86)\p-nand-q.com\GTools\pathed" /APPEND "C:\Program Files\7-Zip" /MACHINE
{% endhighlight %}