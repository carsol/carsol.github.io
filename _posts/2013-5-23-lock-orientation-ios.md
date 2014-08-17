---
layout: post
title: Lock orientation portrait/landscape iOS Phonegap
---

So in PhoneGap I had an issue that I’m sure many of you guys have had when working with a client. I had the normal PhoneGap app working in XCode and part of the functionality involves clicking a link, taking you to a ChildBroswer to view the link. The client wanted the app to be **locked completely to portrait** even when it is in the ChildBroswer. Luckily it’s not too hard but we do need to mess with some of the XCode project code.

I found this answer on [StackOverflow](http://stackoverflow.com/questions/12813013/make-a-uiviewcontroller-stay-in-portrait-mode-ios6-vs-ios5) FYI, thanks goes to [mayuur](http://stackoverflow.com/users/593336/mayuur).

In **AppDelegate.m** add this to the bottom.

{% highlight objective-c %}
- (NSUInteger)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
{
     return (UIInterfaceOrientationMaskAll);
}
{% endhighlight %}

Then in the **MainViewController.m** you can add this if you want only Portrait:

{% highlight objective-c %}
- (BOOL)shouldAutorotate
{
     return YES;
}
- (NSUInteger)supportedInterfaceOrientations
{
     return (UIInterfaceOrientationMaskPortrait);
}
{% endhighlight %}

For Landscape do this instead:

{% highlight objective-c %}
- (NSUInteger)supportedInterfaceOrientations
{
    return (UIInterfaceOrientationMaskAllButUpsideDown);
    //OR return (UIInterfaceOrientationMaskAll);
}
{% endhighlight %}

Now, if you want to do some changes when Orientation changes, then use this function.

{% highlight objective-c %}
- (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
{
}
{% endhighlight %}
