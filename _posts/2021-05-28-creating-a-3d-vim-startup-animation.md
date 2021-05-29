---
layout: post
title: Creating a 3D vim startup animation
date: 2021-05-28 20:26 -0500
---

I'm new to 3D modeling, so I wanted to make something simple for practice. 

I chose the Vim logo because it's got this psuedo-3D effect anyway, and lately I'm just obsessed with vim. 

![vim logo](/assets/images/posts/vim-logo.png)

## Modeling

First I modeled the V and diamond shape by tracing the geometry over a reference image

I chose not to include the 'im' part of 'Vim' to save time. The V is the important part anyway.

![vim logo](/assets/images/posts/logo-wireframe-1.png)
![vim logo](/assets/images/posts/logo-wireframe-2.gif)

## Materials and Lighting

Then I added some material properties to the V and diamond to give them color and reflectivity.

But without lights in the scene, you wouldn't even see anything, so I added a "Sun light" that lights the whole scene, and a green "Rim light" which is like a spotlight aimed at the back. It gives a nice effect around the edges.

![vim logo](/assets/images/posts/vim-lights.png)

A generous portion of "Bloom" is responsible for the overall "glowiness" of the whole scene. See how it looks with Bloom on and off:

![](/assets/images/posts/bloom-toggle.gif)

## Animation

I was inspired by the old Gamecube intro animation. How can you not love the part at the end?

![](/assets/images/posts/gamecube.gif)

My first attempt was promising, but didn't feel as lively as I envisioned.

<video controls>
    <source src="/assets/images/posts/without-tilt.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

Looking back at the Gamecube logo I realized I was missing the forward tilt on impact. After adding that, with a little extra bounce, it felt better.

<video controls>
    <source src="/assets/images/posts/with-tilt.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

This side view clearly shows the forward tilt when the diamond hits the V. It also looks kind of silly from the side. Angles matter.

<video controls>
    <source src="/assets/images/posts/tilt-side.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

## Putting it all together

I spent hours tweaking everything to try and get a satisfying final render. 

The first pass had a LOT of bloom and felt too low contrast.
<video controls>
    <source src="/assets/images/posts/first-pass.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

Second pass I went for a dramatic effect with glare and contrast, but it felt like too much.
<video controls>
    <source src="/assets/images/posts/second-pass.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>
Third pass toned down the bloom but added a bit of contrast. I accidentally turned on motion blur, but ended up liking it and kept it in! 


<video controls>
    <source src="/assets/images/posts/third-pass.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

I know I could make it look a lot better if I kept messing with it, but I want to move on to other projects so I'm calling this one complete.

If you know anything about 3D rendering and have a suggestion for how I could improve it, shoot me an email at [pooria@hey.com](emailto:pooria@hey.com) and I'll give it a shot!

Thanks! for reading!
