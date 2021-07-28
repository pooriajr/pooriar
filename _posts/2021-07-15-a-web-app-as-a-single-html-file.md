---
layout: post
title: A web app as a single HTML file
date: 2021-07-15 17:04 -0500
---

Can you bring an app idea to life with nothing but a single index.html file?

That's what I wanted to find out with my newest project. The scope of the app was small, so it was a perfect low-stakes experiment. Would I build it faster and simpler without any external dependencies? Or would I realize halfway through that I had made a terrible mistake?

## Good constraints

The good thing about restricting myself to a single index.html file is that it forces a lot of restraints on me. 

For example, with no external data store, I can use localStorage to store user data. This lets me skip user authentication completely. Of course it also means that people can't access their data from devices, but that's OK for the initial prototype.

It also forces me to simplify the UI. A fancy UI takes a lot more code when there's no frontend library pulling the weight. So I must choose simple and consistent UI patterns that save me time and sanity. This stops me from trying to get too clever for my own good.

## Reinventing the wheel

One of the first things I did was make my own version of jQuery's dollar sign selector, purely for convenience.

```js
function $(selector) {
  return document.querySelector(selector)
}
function $$(selector) {
  return document.querySelectorAll(selector)
}

HTMLElement.prototype.$ = function(selector){
  return this.querySelector(selector);
};
```

Is it silly to do this when you can just import jQuery? It's definitely less heavy, but there were plenty of other jQuery functions that could have come in handy in this project too. From a productivity standpoint, using jQuery would have saved me time. But I had decided not to introduce dependencies, so these convenience functions were great for making my code cleaner.

## Splitting it up

Pretty quickly I found that developing in a single index.html file was quite annoying. Inlining the CSS and JS made the file too long and hard to navigate. I decided to extract the JS and CSS into their own files for my own sanity. But I stuck to the original idea such that if I wanted to, I could simply copy and past the JS and CSS right back into index.html without any changes, and it would work. No compilation/transpilation/conflagration needed.

## Dependencies exist for a reason

It shouldn't come as a surprise, but working on this project definitely hammered this lesson into my head: "We have libraries/frameworks/languages/etc for a reason."

Coming up with a consistent way to render dynamic templates in HTML without any dependencies was one of the trickier parts that took a decent chunk of time. And that's a problem that has been solved many times. [My solution]({% post_url 2021-06-28-dangerously-dynamic-html %}) ended up being pretty cool for its simplicity, but I still would have been better off using an existing templating language.

## Decision fatigue

Adding new features has this extra step of "How am I going to implement this?" That's a fun question sometimes, but it adds this friction into every little step. Opinionated libraries/frameworks will answer those questions for you, and save you some mental work.

## No holdups and no surprises

If it sounds all bad and slow and painful, here's the golden redeeming factor:

I never once scratched my head and wondered why things weren't working the way I expected. I didn't get tripped up on version incompatibilities. I didn't have to scour bad documentation or go digging through someone else's confusing code.

Every single line of code was clear, highly visible, and easy to understand, because it was all mine.

Once again, the caveat is that on a larger project, my own code would start to be just as confusing as someone else's code, as my brain can only keep so much in RAM. 

But for a small project like this one, it was delightful to effortlessly have complete knowledge of my entire codebase.

## Was it worth it?

In the end I finished the project. [It's live](https://undrift.me) and it works. I enjoyed the challenge and I improved my skills with plain old vanilla web stuff.

So yeah, it was definitely worth it. But would I do it again? No, probably not.

## Not again

Time is too precious to waste on novelty. It's cool to make something in a novel and clever way, like a web app written as a single index.html file. But it's not worth the cost.

I'm glad I did it and satisfied my curiosity. And sometimes you have to do things a "bad" way in order to truly appreciate the better ways. But next time, I'll just use Svelte or something. 

