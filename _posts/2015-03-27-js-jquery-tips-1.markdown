---
layout: post
title:  "JS and Javascript Tips"
date: 2015-03-27 11:50:13
categories: js, jquery
---

## Js&JQuery贴士一 

    如下两种方式相同
{% highlight javascript linenos %}
$(document).ready(function(){...}) ;
$(function() {....});
{% endhighlight %}

***

## Js&JQuery 贴士二

    扩展JQuery $ wrapper

{% highlight javascript linenos %}
$.fn.disable = function() {
    
}
{% endhighlight %}

***

## Js&JQuery 贴士三
    Execution Context and Scope Chain

{% highlight javascript %}
var color = "blue";
function changeColor() {
    var anotherColor = "red";

    function swapColor () {
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
    }

    swapColor();
}
changeColor();
{% endhighlight %}

> Execution Context and Scope Chain :
> *global*, *function* and *eval* execution context.

### Execution context for previous snippet code
    window
        color
        changeColor
            anotherColor
            swapColor
                tempColor

***

## Js&JQuery 贴士四
    Function Internal

在Javascript 的function当中，有两个特殊的objects: **arguments**和**this**。

- **arguments**
{% highlight javascript %}
    function factorial(num)
    {
        if (num <= 1)
            return 1;
        else
            return num * factorial(num - 1);
    }
    //is equal to
    function factorial(num)
    {
       if (num <= 1) 
           return 1;
        else
            return num * arguments.callee(num - 1);
    }
{% endhighlight%}

- **this**

> it is a reference to the context object that the function is operating on -- often called the this value.
