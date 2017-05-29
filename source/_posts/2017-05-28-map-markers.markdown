---
layout: post
title: "Animated markers on an SVG map"
date: 2017-05-28 17:07:03 -0700
comments: true
categories: Less, CSS Animation, Keyframe Animation, Sass
---

I recently started my new job at [Headnote](https://headnote.com). My first project was to redo all of the public facing pages to update the branding and advertising language. The one piece of code I would like to share from the experience is the animated markers that pop up in various places on a map. Now, I am new to Less (purely a Sass guy before this gig), so this was a great learning experience for me about what advanced features Less supports compared to Sass. There were definitely some challenges. The main one being that Less is much less flexible (no pun intended) in terms of interpolation and apparently is very very picky about using variables / interpolation on the _left_ side of a definition (I dont know what else to call it)... For example:

```scss

@side: left;

.my-class {
  margin-@{side}: 5px;
}

```

This particular example works... however when I tried to use this sort of interpolation for keyframe animation<!--more-->, Less complained endlessly. Admittedly this is a bit of an edge case. I tried every possible combination of escaping, string concatenation, interpolation, etc and could get neither the `%` sign needed for defining keyframes nor inline math to work. In my experience, [Sass is a bit more forgiving in this regard](https://gist.github.com/skylarmb/258236d9887041b2e78e26c02e963a11), although I havent tried re-creating this exact code in Sass.

```scss

.map-keyframe-set(@base-percent, @offset) {
  @{base-percent}% { // syntax error!
    opacity: 0;
  }
  (@{base-percent})% { // syntax error!
    opacity: 0;
  }
  @{base-percent}\% { // escaping doesnt work!
    opacity: 0;
  }
  @{base-percent}~"%" { // this escaping doesnt work either!
    opacity: 0;
  }
  "@{base-percent}%" { // Nope!
    opacity: 0;
  }
  @{base-percent} + @{offset} { // math doesnt work!
    opacity: 0;
  }
  (@{base-percent} + @{offset}) { // math doesnt work!
    opacity: 0;
  }
  //...
}
```

Believe me, these are not all the syntaxes I tried. I spent almost an hour being stubborn about this... I ended up going with a simple but extremely un-satisfying solution. Pass values in as percentages and do the percentage math first, then use the resulting value in the definition.

```scss


.map-keyframe-set(@base-percent, @offset) {
  @start: (@base-percent + @offset);

  @{start} { // OK!
    opacity: 0;
  }
  //...
}
```

Anyways, here is the result! PLEASE open this in a new tab, you cannot see the whole map in this tiny iframe

<p data-height="580" data-theme-id="0" data-slug-hash="KmLeXd" data-default-tab="css,result" data-user="skylarmb" data-embed-version="2" data-pen-title="KmLeXd" class="codepen">See the Pen <a href="https://codepen.io/skylarmb/pen/KmLeXd/">KmLeXd</a> by Skylar Brown (<a href="https://codepen.io/skylarmb">@skylarmb</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>



