---
layout: post
title: Eliminating duplicate logic with Hotwire
date: 2021-06-15 23:55 -0500
---

I want to share a simple example where Hotwire helped me eliminate duplicate logic in a recent Rails app.

My Rails app involves voting for things. You can click on an element to cast a vote for it, or click again to delete your vote. Similar to Instagram "likes". 

Let's call this clickable element a "Votable". 
![](/assets/img/votable.gif)

In this article we only care about 3 dynamic parts of a Votable: 

1. The heart icon can be either filled or outlined, signalling whether or not you cast a vote.
2. The vote count changes based on the number of votes, including yours.
3. The HTTP request that gets sent when you click the Votable can either be a POST request to create a vote, or a DELETE request to delete a vote. The URL of the request also varies.

I have a week to build the app, so I'm sticking to Rails convention for speed and simplicity, using server-rendered templates. The template contains the logic to render the  dynamic parts of a Votable. On the initial page load, everything looks and works exactly as it should.

But what happens when someone clicks on a Votable? That triggers a change in the back-end (creating or destroying a record in the database), so how do we update all those dynamic parts on the front-end to reflect that change?

## An Easy Way

The easiest way is to simply re-render the whole page. The initial page load is always in sync, so just show an initial page load after every interaction.

With Turbolinks (now Turbo Drive), this *might* be fast and seamless, depending on the size of the page render. In my case this approach is too slow once I have many Votables on the same page. A full page reload also resets the scroll position, which I don't want.

I need to update the DOM without reloading. Time for a little JS.

## A Standard Way

A standard approach with JavaScript is to listen for click events on Votables, and then update the DOM like so:

1. Depending on the state of the Votable, set the appropriate heart icon
2. Depending on the state of the Votable, increment or decrement the vote count by 1
3. Depending on the state of the Votable, set the appropriate HTTP method and action on the button.

As you can see, my JavaScript is very concerned with state. That's because now I can't rely purely on my server-rendered template computing state from the database. I have to track state on the front-end separately.

Now I have duplicate logic. Logic from my server-rendered template must be mimicked by my front-end JS. And it's not just translating Ruby to JavaScript. One is an ERB template, the other is imperative DOM manipulation. 2 very different approaches that must be kept in sync to render the same HTML. 

There's even more duplication. What if the request fails on the back-end? I need front-end logic to handle that, even though I already have it on the back-end. Ah well, duplicate it. I also might need to handle the "in-between" state where I'm waiting to hear back from the server on whether the operation succeeded. Browsers already have that, so now I'm even duplicating browser logic.

I can either suck it up and write/maintain this duplicate logic, or eliminate duplication by moving *all* the view logic out of templates and into the front-end with something like a React component. But this merely trades one set of problems for another and adds complexity. Plus I'm short on time, and these changes aren't trivial. Is there a better way?

## A Hotwire Way

Hotwire has a feature called Turbo Frames. Turbo Frames are similar to the method from the Easy Way I mentioned earlier, where you do a full page reload with your server rendered template. But Turbo Frames don't reload the *whole* page, rather just the specific parts that need it, leaving the rest alone.

So by wrapping each Votable in its own Turbo Frame, it doesn't matter whether a page has 5 Votables or 50 Votables, clicking on a Votable only updates that one Votable and nothing else, keeping page updates small and fast.

I can remove all that DOM manipulation JavaScript. Turbo Frames use the same server-rendered templates that I already have. Now all my view logic is in one place and guaranteed to stay in sync with my database. For users, it feels fast and responsive, like a SPA.

Best of all, getting these benefits is a breeze. Ease of use is clearly a top priority for Hotwire. You just add a few wrappers with ID's to your templates and the library handles the rest. 

![](/assets/img/i-love-magic.png)

## Going forward

This was a small example, but there's more to gain from using Hotwire. For example, Turbo Streams, another part of Hotwire, would let me update the vote counts on everyone's screen in real time as *other people* cast votes. 

Hotwire is still in beta. I'm excited to see how it opens up a new way to build painless modern front-ends while retaining the simplicity of server-rendered templates. Check it out at [hotwire.dev](https://hotwire.dev)
