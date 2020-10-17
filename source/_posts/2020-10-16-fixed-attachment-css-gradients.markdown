---
layout: post
title: "Fixed attachment CSS gradients"
date: 2020-10-16 17:40:30 -0700
comments: true
categories: 
---

Yeah it's been a while since I last posted. I need to start posting more small things instead of convincing myself that only really big cool things are blog worthy. So, here's something small.

I was recently admiring the gradient backgrounds on messages in Facebook messenger. Specifically, I liked how the gradient colors stayed relative to your viewport even as you scrolled. I thought it would be fun to attempt to replicate this in CSS.

The way I implemented this was simply to use the desired gradient as a `background-image` on the element, which allows us to use the powerful css properties that are available for background images. There are quite a few interesting ones that can be used for cool things, like `background-position` and `background-clip`, which I may explore some other time. The only property we need to use in this case is `background-attachment: fixed;` on the message elements. This works much like `fixed` in other contexts in CSS. 

From MDN on `background-attachment`:

> `fixed`: The background is fixed relative to the viewport. Even if an element has a scrolling mechanism, the background doesn't move with the element.

Try scrolling on the messages to see the effect in action. Also try switching the color scheme for some more dramatic gradients.

<p class="codepen" data-height="700" data-theme-id="dark" data-default-tab="css,result" data-user="skylarmb" data-slug-hash="WNxwzoR" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Messenger background gradients">
  <span>See the Pen <a href="https://codepen.io/skylarmb/pen/WNxwzoR">
  Messenger background gradients</a> by Skylar Brown (<a href="https://codepen.io/skylarmb">@skylarmb</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Shoutout to some cool and fun tools I used:

Emojify: https://matthewmiller.dev/experiments/emojify/

Corporate Ipsum: https://www.cipsum.com

uiGradients: https://uigradients.com/
