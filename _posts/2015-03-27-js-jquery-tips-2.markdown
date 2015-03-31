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

{% endhighlight %}

>> Setting box-sizing to border-box makes width and height calculations more intuitive by including padding and borders

***

## JS&JQuery贴士三 CSS shorthand Properties

{% highlight css %}
//1, a value which is not specified is set to its initial value
//2, only the individual properties values can inherit
//3, shorthand properties try not to force a specific order for the values of the properties they replace.
.border-props {
    border-width: 1px;
    border-style: solid;
    border-color: #000;
    // border : 1px solid #000;
}

.margin-props {
    margin-top : 10px;
    margin-right: 5px;
    margin-bottom: 10px
    margin-left: 5px;
    // margin: 10px 5px 10px 5px
}

//rules for shorthand, TRouBLe(Top, Right, Bottom, Left)

{% endhighlight %}
>> 参考这篇文章 [shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties)

***
## JS&JQuery 贴士四


***

