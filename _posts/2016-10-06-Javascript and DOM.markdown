---
layout: post
title:  "DOM and JS"
date:   2016-10-06 15:50:03 -0400
categories: WEB
---
**How to select DOM elements?**
{% highlight js %}
document.getElementById("element_id");
{% endhighlight %}


**How to add event listener to a  button?**

{% highlight js %}
 document.getElementById( "byIdButton" ).addEventListener("click", byId, false );
 var byId = function(){ ... }
 //boolean indicates that it will not call preventDefault()
{% endhighlight %}

**How to create new node in DOM?**

{% highlight js %}
var newNode = document.createElement( "p" );
newNode.appendChild( document.createTextNode( "text" ));
var currentNode = document.getElementById( "some_id" );
currentNode.appendChild(newNode); //apends nde as a child of new node
currentNode.parentNode.insertBefore( newNode, currentNode ); //inserts element before current node, you need to do it from parent though
{% endhighlight %}

**How to replace node?**
{% highlight js %}
 var newNode = document.createElement("p");
 newNode.appendChild(document.createTextNode("some long text"));
 oldNode.parentNode.replaceChild( newNode, oldNode );
{% endhighlight %}

**How to remove node?**
{% highlight js %}
   if ( currentNode.parentNode == document.body )
      alert( "Can't remove a top-level element." );
   else
   {
      var oldNode = currentNode;
      switchTo( oldNode.parentNode );
      currentNode.removeChild( oldNode );
   }
{% endhighlight %}
