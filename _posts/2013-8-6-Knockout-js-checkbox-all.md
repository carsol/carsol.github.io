---
layout: post
title: Knockout.js Checkbox All
---


Knockout.js is powerfully awesome. I’ve been learning it for a client and I figured I could explain an example to help anyone else that is new to Knockoutjs.

I set out to create a “select all” checkbox for a list of possible checkboxes in the list.

First I created a function to define my listItem class:
{% highlight javascript %}
function listItem(name, artType, artSize) {
    return {
        isSelected : ko.observable(false),
        name : ko.observable(name)
    };
}
{% endhighlight %}

After I added the magic aka the ViewModel:
{% highlight javascript %}
var viewModel = {
    listItem : ko.observableArray([new listItem("item #1"), new listItem("item #2"), new listItem("item #3")]),
    allSelected: ko.observable(false),
    selectAll : function() {
        var all = this.allSelected();
        ko.utils.arrayForEach(this.listItem(), function(listItem) {
           listItem.isSelected(!all);
        });
        return true;
    }
};

ko.applyBindings(viewModel);
{% endhighlight %}

Of course this isn’t everything. Knockout has a templating feature that I use in my HTML to loop through `ko.observableArray` as such:
{% highlight HTML %}
<script id="liTemplate" type="text/html">
    <li>
        <input type="checkbox" data-bind="checked: isSelected" />
        <span data-bind="text: name"></span>
    </li>
</script>
{% endhighlight %}

and then call in:
{% highlight HTML %}
<ul data-bind="template: { name: 'liTemplate', foreach: listItem }"></ul>
{% endhighlight %}

That’s it! Below is the jsFiddle example:

<iframe width="100%" height="300" src="http://jsfiddle.net/csolares23/CLPKV/embedded/" allowfullscreen="allowfullscreen" frameborder="0" style="width: 620px; height: 300px;"></iframe>
