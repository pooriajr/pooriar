---
layout: post
title: "Behind the scenes: 3D Vim animation"
date: 2021-05-28 20:26 -0500
---

I decided to learn 3D animation with the Vim logo because it's got this pseudo-3D effect. Actually, that's a lie, I'm just obsessed with vim. 

![vim logo](/assets/images/posts/vim-logo.png)

## Modeling

I modeled the shapes by tracing the geometry over a reference image.

![vim logo](/assets/images/posts/logo-wireframe-1.png)

I chose not to include the 'im' part of 'Vim'. Just to save time.

![vim logo](/assets/images/posts/logo-wireframe-2.gif)

Here's an interactive version of the model. You can spin it around!

<script type="module" src="https://unpkg.com/@google/model-viewer/dist/model-viewer.min.js"></script>

<div class="resp-container">
<model-viewer
    class="resp-iframe"
    src="/assets/models/vim.glb"
    poster="/assets/models/vim-poster.png"
    disable-zoom
    camera-controls
    auto-rotate-delay='0'
    interaction-prompt-threshold="1000"
    ar 
    ar-modes="webxr scene-viewer quick-look"
    auto-rotate >
</model-viewer>
</div>

## Materials and Lighting

Then I added real-world material properties to the shapes. In simple terms, I told the program that the diamond is green and reflective, while the V is rough and metallic.

Materials are only half of the equation; you need lights for the materials to react to. 

I use a "Sun light" that lights the whole scene from the camera's perspective, and a "Rim light" which is a green spotlight aimed at the back of the logo. The purpose of the Rim light is to do what *every designer must do*: make the logo "pop". 

![vim logo](/assets/images/posts/vim-lights.png)

A generous portion of "Bloom" is responsible for the overall "glowiness" of the whole scene. See how it looks with Bloom on and off:

![](/assets/images/posts/bloom-toggle.gif)

## Animation

I was inspired by the old Gamecube intro animation. How can you not love the part at the end?

![](/assets/images/posts/gamecube.gif)

My first attempt was promising, but didn't feel lively enough.

<video controls>
    <source src="/assets/images/posts/without-tilt.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

Looking back at the Gamecube animation I realized I was missing the forward tilt on impact. After adding tilt (plus a little extra bounce) the movement felt right.

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

Second pass I went for a more dramatic effect with glare and contrast, but it was overkill.
<video controls>
    <source src="/assets/images/posts/second-pass.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>
Third pass was somewhere in the middle. I accidentally turned on motion blur, but ended up liking it and kept it in! 


<video controls>
    <source src="/assets/images/posts/third-pass.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

I could keep messing with it and get it *just right*, but I want to move on to other projects. 

I'm calling this one complete.

.

.

Except I'm way too obsessed to leave it like that.

## Final Form

Last night, after publishing this article and getting ready for bed, I had a little idea:

What if I use a better render engine? 

It would be slower.

Approximately *36000%* slower.

But what if...?

![](/assets/images/posts/one-eternity-later.jpg)

OK!

The render finished. But it didn't even look much better. Perhaps with a tiny bit more work...

![](/assets/images/posts/5-hours-later.jpg)

OK!

I learned *a lot* about post-processing in Blender.

Post processing is cool because it's all happening in 2d, so changes are faster and easier than working in 3D. It also gives you a lot of control over the final result.

But it's still a lot to figure out!

![](/assets/images/posts/vim-compositing.png)

If you're totally confused, think of it like a chain of advanced Instagram filters, all building up into one final image.

And here's the *real* final result:

<video controls>
    <source src="/assets/images/posts/vim-final.mp4"
            type="video/mp4">
    Sorry, your browser doesn't support embedded videos.
</video>

I spent an unreasonable amount of time on this, but the result feels worth it.

If you enjoyed this, shoot me an email at [pooria@hey.com](mailto:pooria@hey.com). I only have a couple regular readers (Hi, Mom! 👋) so every little message means a lot!

Thanks for reading! 

`:wq`
