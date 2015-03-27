---
layout: post
title:  "JS and Javascript Tips"
date: 2015-03-27 11:50:13
categories: js, jquery
---

{% highlight javascript linenos %}
$(document).ready(function(){...}) ;
$(function() {....});
/* jQuery iFrame Tabs Plugin
 *  Version: 0.5.01
 *  Author: think8848 
 *  Contact: QQ: 166156888 Blog: http://think8848.cnblogs.com
 *  Company: http://www.cleversoft.com */
;
$.fn.cleverTabs = function(options) {
    var self = this;
    var options = $.extend({
setupContextMenu: true
}, options || {});
var tabs = new CleverTabs(self, options);
if (options.setupContextMenu) {
    tabs.setupContextMenu();
}
return tabs;
};
//定义全局变量，鼠标触动的tab
{% endhighlight %}
