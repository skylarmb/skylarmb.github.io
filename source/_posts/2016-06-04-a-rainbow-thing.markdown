---
layout: post
title: "a rainbow thing"
date: 2016-06-04 21:23:13 -0700
comments: true
categories: 
---

I came across this gif on reddit today. 
<br>
<br>
<img src="http://67.media.tumblr.com/a123606e52db443841a6bb982d93febe/tumblr_n7a40sXB7P1r2geqjo1_r1_500.gif">
<br>
<br>

No idea how it was made but thought it would be an interesting challenge to replicate it with css keyframe animation. I explicitly set out to do this with no javascript just to explore the limitations of keyframe animation, and yes, I am well aware that the problems listed below can all be solved with JS. Thats not the point.

Here are some interesting limitations of keyframe animation that I came across while doing this. 

###### Problem 1: Iteration Delays
As helpful as the `animation-delay` property can be, it only delays the start of the animation relative to page load (or to when the animation was applied to the element). There is no delay-per-iteration type property. I would love to be able to specify an animation something like this...

~~~css
.animate {
	animation: spin 1s ease-in-out infinite;
	animation-iteration-delay: 0.2s;
}
~~~

meaning the animation would loop infinitely, still taking 1 second to complete, but delaying for 0.2s before starting its 1s loop again. While searching around for workarounds I found [this](http://lists.w3.org/Archives/Public/www-style/2011May/0549.html) discussion of this exact topic. 

If I had been able to use something like that, my code below could have been a lot simpler. The way I did it was generating keyframe sets for each segment (`spin-0`, `spin-1`, etc...) that were all just different divisions of the total animation loop time (like hours on a clock). Segment 0 "plays" from noon to 1, and explicitly does nothing from 1 til midnight. Segment 1 "plays" from 1 to 2... etc. Instead of this wonkyness, you could just have a single `spin` animation (as simple as a `from {...} to {...}`) that lasts for the actual amount of time the `spin` animation runs (1h in the clock analogy) with none of this mathy percentage stuff. Each segment would just get an `animation-delay` according to it's position in the circle and an `animation-iteration-delay` equal to the total animation loop time.

###### Problem 2: The 100% keyframe

Animations always start at the 0% and end at 100%. That makes sense. However it would be pretty cool if you could [bodge](https://www.youtube.com/watch?v=lIFE7h3m40U) that a little and _if_ keyframes over 100% were defined, it would play through them at the apropriate time scale (as in an animation with 0-120% keyframes defined would take 120% of its animation time to complete). This would have come in handy at the end of the animation loop where it would be great to start the next loop of the animation before the last one was done. Overlap if you will. Of course this was only a limitation because of the above mentioned limitation with iteration delays... That would be the real way to solve this.

Anyways, it's nowhere near perfect as you can see. It doesn't loop properly for the reasons above, and I could have done better with the positioning so that when it loops there isn't a clear jump as it resets.

###### The actual code
I highly recommend opening it on codepen to see it in a less cramped view.
<br>
<br>
<p data-height="425" data-theme-id="0" data-slug-hash="ZOGMzZ" data-default-tab="css,result" data-user="skylarmb" data-embed-version="2" class="codepen">See the Pen <a href="https://codepen.io/skylarmb/pen/ZOGMzZ/">ZOGMzZ</a> by Skylar Brown (<a href="http://codepen.io/skylarmb">@skylarmb</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>