---
layout: post
title: Disable UIWebView bounce for iOS Phonegap
---


Though this may be a bit out of date, I’m in the process of cleaning out my code snippets and I’ll be tossing a couple ones that were really helpful here.

Anyways, this was a necessary for PhoneGap apps on iOS for when you didn’t want to have have the elastic effect that you get in WebView or Safari.

*After PhoneGap 1.5.0 and later this is not needed. Just use the plist file. Just set UIWebViewBounce to false*

If you want to hard code it in (not recommended but could be useful!) you’ll need to go to the MainViewController.m and find webViewDidFinishLoad function.

Add

```objective-c
[[theWebView.subviews objectAtIndex:0] setBounces:NO];
```

in the method.


That’s it! You can also use Javascript to disable touch but it can really hurt the rest of your app if you have touch features (bascially don’t use unless really needed).

```javascript
document.onload = function(){
document.ontouchmove = function(e){ e.preventDefault(); }
};
```
