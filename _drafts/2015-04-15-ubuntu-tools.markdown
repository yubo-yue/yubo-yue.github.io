---
layout: post
title:  "Ubuntu tools"
date: 2015-04-15 11:50:13
categories: Ubuntu Utility Tools
---

## update-alternatives

  Take install multiple verison of MVN as an example

{% highlight bash %}

update-alternatives --display mvn

update-laternative --list mvn

update-alternatives --config mvn

 update-alternatives --install /usr/bin/mvn mvn /usr/share/maven32/bin/mvn 320 --slave /usr/bin/mvnDebug mvnDebug /usr/share/maven32/bin/mvnDebug

{% endhighlight %}


