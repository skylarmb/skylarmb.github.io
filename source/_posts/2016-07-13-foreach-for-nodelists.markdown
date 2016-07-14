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

It turns out that `getElementsByClassName`, `querySelectorAll` etc do not return an array. They return a `NodeList` which doesn't have a `forEach` function like an array does. 

##### Workarounds

###### Option 1: Array prototype
So, if you google this problem you will find a lot of people suggesting you use the Array prototype `forEach`.

~~~js
var myNodeList = document.getElementsByClassName('my-class');
Array.prototype.forEach.call(myNodeList, function (item) {
  // do stuff with item
});
// or, more simply
[].forEach.call(myNodeList, function (item) {
  // do stuff with item
});
~~~

This is ugly, and also confusing in my opinion, especially if you use the second option... What if you do 

~~~js
[1,2,3].forEach.call(myNodeList, function (item) {
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

So, this may seem crazy and it is. I have no idea the implications of this and would probably never do it in a production environment until I knew more about it, but ...

~~~js
NodeList.prototype.forEach = Array.prototype.forEach;
document.getElementsByClassName('my-class').forEach(function(){
  // woah! magic! ... but please don't
});
~~~

###### Other 'Solutions'

There are a number of other weird workarounds and 'solutions'. I wont go into them but here are a couple.

~~~js
var myNodeList = document.getElementsByClassName('my-class');
var myArrayFromNodeList = [].slice.call(myNodeList);
myArrayFromNodeList.forEach(function(item){
  // do stuff with item
});
~~~
or

~~~js
var forEach = function (array, callback, scope) {
  for (var i = 0; i < array.length; i++) {
    callback.call(scope, i, array[i]); // passes back stuff we need
  }
};
var myNodeList = document.getElementsByClassName('my-class');
forEach(myNodeList, function (index, value) {
  // do stuff with index and value
});
~~~
##### The Solution

I would suggest (and would use) the first variant of option 1. It may be confusing-ish but at least it't not hacky. 

So, you would see this is any code I write. 

~~~js
var myNodeList = document.getElementsByClassName('my-class');
Array.prototype.forEach.call(myNodeList, function (item) {
  // do stuff with item
});
~~~