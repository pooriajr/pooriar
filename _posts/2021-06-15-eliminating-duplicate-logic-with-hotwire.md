---
layout: post
title: Eliminating duplicate logic with Hotwire
date: 2021-06-15 23:55 -0500
---

I want to share a simple example where Hotwire helped me eliminate duplicate logic in a recent Rails project.

My Rails project involves voting for things. You can click on an element to cast a vote for it, or click again to delete your vote. Similar to Instagram "likes". 

Let's call this clickable element a "Votable". 
![](/assets/img/votable.gif)

In this article we only care about 3 dynamic parts of a Votable: 

1. The heart icon can be either filled or outlined, signalling whether or not you cast a vote.
2. The vote count changes based on the number of votes, including yours.
3. The HTTP request that gets sent when you click the Votable can either be a POST request to create a vote, or a DELETE request to delete a vote. The URL of the request also varies.

The logic to handle these dynamic parts already exists in the server-rendered templates. So on the initial page load, everything looks and works exactly as it should.

But what happens when someone clicks on a Votable? That triggers a change in the database, so how do we update all those dynamic parts to reflect that?

## An Easy Way

The easiest way is to simply re-render the whole page. The initial page load is always in sync, so just show an initial page load after every interaction.

With Turbolinks, this *might* be fast and seamless, depending on the size of the page render. In my case this approach is too slow once I have many Votables on the same page. A full page reload also resets the scroll position, which I don't want.

I need to update the DOM without reloading. Time for a little JS.

## A Standard Way

A simple approach with JavaScript is to listen for click events on Votables, and then update the DOM like so:

1. Depending on the state of the Votable, set the appropriate heart icon
2. Depending on the state of the Votable, increment or decrement the vote count by 1
3. Depending on the state of the Votable, set the appropriate HTTP method and action on the button.

And by state I mean whether the Votable is either "already voted" or "not voted".

As you can see, my JavaScript is very concerned with state. That's because now I can't rely purely on my server-rendered template pulling state from the database. I have to keep track of state on the front end too. That's not ideal, because it invites bugs from the front-end and back-end falling out of sync.

Another problem is that now I have duplicate logic. Logic from my server-rendered template must be mimicked by my front-end JS. And it's not just translating Ruby to JavaScript. One is server-rendered ERB, the other is imperative DOM manipulation. 2 totally distinct approaches that have to be kept in sync to produce the same HTML. Not ideal.

Surely these are common and solvable problems for developers, but it would be great to avoid them if possible. 

## A Hotwire Way

Hotwire has a feature called Turbo Frames. Turbo Frames are similar to the method from the Easy Way I mentioned earlier, where you do a full page reload with your server rendered template. But Turbo Frames don't reload the *whole* page, rather just the specific parts that need it, leaving the rest alone.

So by wrapping each Votable in its own Turbo Frame, it doesn't matter whether a page has 5 Votables or 50 Votables, clicking on a Votable only updates that one Votable and nothing else, keeping page updates small and fast.

I can remove all that DOM manipulation Javascript. Turbo Frames uses the same server-rendered templates that I already have. Now all my view logic is in one place and always guaranteed to be in sync with my database. The end result feels fast and responsive like a front-end framework. Best of all, getting all these benefits was a breeze thanks to the effort that creators of Hotwire have put into making it easy to use.

## Going forward

This was a small example, but there's more to gain from using Hotwire. For example, Turbo Streams, another part of Hotwire, would let me update the vote counts on everyone's screen in real time as *other people* cast votes. 

And Hotwire is still in beta. I'm excited to see how it opens up a new way to build painless modern front-ends while retaining the simplicity of server-rendered monoliths. Check it out at [hotwire.dev](https://hotwire.dev)
