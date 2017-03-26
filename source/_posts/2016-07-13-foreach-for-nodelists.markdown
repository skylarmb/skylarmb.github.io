---
layout: post
title: "forEach for NodeLists"
date: 2016-07-13 22:14:02 -0700
comments: true
categories: 
---

##### The Problem
So jQuery is great... Want to loop over all `my-class` elements?

~~~js
$('.my-class').each(function(){
	// do stuff with $(this)
});
~~~

Now as great as it is, you might not want to use jQuery on your site. There are [many](http://blog.garstasio.com/you-dont-need-jquery/) [reasons](http://youmightnotneedjquery.com/) [why](http://stackoverflow.com/questions/5099949/what-are-some-empirical-technical-reasons-not-to-use-jquery).
No problem, lets just use vanilla javascript.

~~~js
document.getElementsByClassName('my-class').forEach(function(){
  // do stuff ... ?
});
~~~

But whats this? 

```Uncaught TypeError: document.getElementsByClassName(...).forEach is not a function(…)```

<!--more-->This is because `getElementsByClassName`, `querySelectorAll` etc do not return an array. They return a `NodeList` which doesn't have a `forEach` function like an array does.

##### Workarounds

###### Option 1: Array prototype
So, if you google this problem you will find a lot of people suggesting you use the Array prototype `forEach`.

~~~js
var myNodeList = document.getElementsByClassName('my-class');
Array.prototype.forEach.call(myNodeList, elem => {
  // do stuff with item
});
// or, more simply
[].forEach.call(myNodeList, elem => {
  // do stuff with item
});
~~~

This is ugly, and also confusing in my opinion, especially if you use the second option... What if you do 

~~~js
[1,2,3].forEach.call(myNodeList, elem => {
  // what are we looping over here? [1,2,3] or myNodeList?
});
~~~

###### Option 2: A for loop

A good ol' for loop

~~~js
var myNodeList = document.getElementsByClassName('my-class');
for (var i = 0; i < myNodeList.length; i++) {
  // do stuff with myNodeList[i]
}
~~~

Well... this works, but feels primitive in the age of high level languages...

###### Option 3: Hack those prototypes!

So, this may seem crazy and it is. But [Bling.js]() gave me this idea, and it totally works...

~~~js
NodeList.prototype.__proto__ = Array.prototype // does not work in IE9
document.getElementsByClassName('my-class').forEach(elem => {
  // woah! magic!
})
~~~

##### The Solution

I personally use the the third approach, although YMMV depending on which browsers you need to support.

