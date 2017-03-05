---
layout: post
title: Android EmailComposer PhoneGap 2.6
---

***I know this is late but could be useful for someone.*** So lately I’ve had trouble implementing an EmailComposer Plugin for an Android version of PhoneGap 2.6 because there weren’t any instructions on the official plugin’s github page. I figured I would explain the steps to how I implemented it myself.

First off thank you to [Dpa99c](http://stackoverflow.com/users/777265/dpa99c) from answering this [question](http://stackoverflow.com/questions/17387853/getting-error-while-adding-email-composer-plugin-in-phonegap-error-adding-plugi) it really helped! **Remember this is for PhoneGap 2.6. I haven’t tested it a newer version of PhoneGap but I’m sure I’m not too far off.**

1. Of course you have to have PhoneGap installed and ready to go. Make sure you’ve gone through the import setup for Eclipse also.

2. Add files:

  add **emailcomposer.js** into your www folder and include it in your index.html

  add **EmailComposer.java** to your build. I added it into
  ```HTML
  src/com/phonegap/plugins/emailComposer/EmailComposer.java
  ```

3. Edit **config.xml** to include plugin by adding
```html
<plugin name="EmailComposer" value="org.apache.cordova.plugin.EmailComposer"/>
```


4. Then finally you can add the javascript to make it work. You would use something like this (I passed in a couple variables in this example):
```javascript
cordova.require('cordova/plugin/emailcomposer').showEmailComposer(
                function() { console.log( 'successfully called email composer' ); },
                function() { console.log( 'failed to call email composer' ); },
                "Look at this photo",
                "Take a look at <b>this<b/>:",
                ["fred@blogs.com", "peter@pan.com"],
                [],
                [],
                true,
                ["image.jpg", "file.zip"]
            );
```

Works just fine on my Nexus 7. Just for safe measure here is the callable interface:
```javascript
window.plugins.emailComposer.showEmailComposerWithCallback(callback,subject,body,toRecipients,ccRecipients,bccRecipients,isHtml,attachments);
```

You can find more information from the official plugin github [here](https://github.com/csolares23/phonegap-plugins/tree/master/Android/EmailComposerWithAttachments). You can also download my sample project [here](https://github.com/csolares23/Android-EmailComposer-Phonegap-2.6).

