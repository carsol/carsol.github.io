---
layout: post
title: Tips for debugging Node.js
---

Here are some tips for debugging node.js. I had some trouble debugging multiple processes early on, but I realized a couple things I want to note. Hopefully they can help!

### Debugging mulitple node processes
I like running node processes with the `--debug` flag. I know there are other ways (like using `node-debug app.js`) but I found doing this was best for dealing with many processes, mostly because you can still use node-inspector or terminal without much issue. [If you need extra info here might be a good spot to start.](https://github.com/node-inspector/node-inspector#2-enable-debug-mode-in-your-node-process)

It should look like this:

{% highlight javascript %}
node --debug file_to_debug.js
{% endhighlight %}


**At this point you can debug via node-inspector or terminal.**

### Using Node-inspector:
[Node-inspector](https://github.com/node-inspector/node-inspector) can attach itself to different processes if needed by changing the port. All you have to do is run `node-inspector` in another terminal window then go to

`http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=<debugging port>`

You can then change the debugging port to inspect different processes. Start from port 5858 and move 1 up for every process.


### Using Terminal:
Open another terminal window and run:

```
node debug localhost:<debugging port>
```

Then you can basic terminal commands once debugging:<br>
`repl` - Open debugger's repl for evaluation in debugging script's context <br>
`cont`, `c` - Continue execution on a break point (`debugger;`) <br>
`next`, `n` - Step next <br>
`step`, `s` - Step in <br>
`out`, `o` - Step out <br>
`pause` - Pause running code (like pause button in Developer Tools) <br>

You can find all of the terminal commands at The [Node.js Documentation for Debugger](https://nodejs.org/api/debugger.html)

After happy debugging!
