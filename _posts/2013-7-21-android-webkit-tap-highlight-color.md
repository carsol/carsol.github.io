---
layout: post
title: Android WebKit Tap Highlight Color
---

One aspect of PhoneGap developing apps is that any `a href` links on Android app give you a highlighted effect when you touch it. This only happens on Android, but after a little research I was happy I could take it off.

**Default:**

[![Android -webkit-tap-highlight-color]({{ site.baseurl }}/images/android-webkit-tap-highlight-color/1.png)]({{ site.baseurl }}/images/android-webkit-tap-highlight-color/1.png)

**With -webkit-tap-highlight-color CSS**

{% highlight css %}
html {
-webkit-tap-highlight-color: rgba(0,0,0,0);
 -webkit-tap-highlight-color: transparent;
}
{% endhighlight %}

[![Android -webkit-tap-highlight-color]({{ site.baseurl }}/images/android-webkit-tap-highlight-color/2.png)]({{ site.baseurl }}/images/android-webkit-tap-highlight-color/2.png)
