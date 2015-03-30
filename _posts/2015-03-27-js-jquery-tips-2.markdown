---
layout: post
title:  "JS and Javascript Tips 2"
date: 2015-03-30 12:08:13
categories: js, jquery
---

## Js&JQuery贴士一  Jquery Height and width

{% highlight javascript %}
$("element").height();
$("element").width();
//default way
//width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）.
//height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）.

{% endhighlight %}

***

## Js&JQuery贴士二 Jquery box-sizing : border-box


{% highlight css %}
*::before, *::after {
   box-sizing: border-box; 
}

//setting box-sizing to border-box makes width and height calculations more intuitive by including padding and borders
{% endhighlight %}
