---
layout: post
title: Dangerously dynamic HTML
date: 2021-06-25 20:47 -0500
---
This article tries to be beginner-friendly. It starts with an explanation of static vs. dynamic, then meanders through safe examples before getting to *the danger*.

If you want to skip straight to danger, [right this way.🤘](#danger)

## What is dynamic, *really*?

Let's say you're making an app to keep track of personal relationships (\*cough\* [undrift.netlify.app](undrift.netlify.app) \*cough\*). You probably need to create HTML elements that correspond to various people. 

Let's say I want to add 3 people into the app: My friend Alice, my coworker Bob, and my Cat (yeah that's not a person but I was going for an A,B,C thing).

I'm the programmer, goddamnit, so I can just write them into the HTML like so:

```html
<ul id="people">
  <li class="person">Alice</li>
  <li class="person">Bob</li>
  <li class="person">Cat</li>
</ul>
```
But if I want to add a person, or change a name, or remove one, I have to go edit my code every time. More importantly, my "app" is quite useless for you or anyone else, unless you also have an Alice, Bob, and Cat in your life. 

This is *static*, and it's the opposite of dynamic.

A dynamic version of this would allow anyone to add their own list of people, and edit it without me, the programmer, having to touch code.

As you can imagine, dynamism is important in the world of websites, and anything fancier than a digital brochure needs it.

# Ways to fill the gap

HTML doesn't offer a great way to handle dynamic elements. The language was designed for documents, which are generally static. Like literal digital brochures. And now here we come wanting to write a fancy dynamic application with it? We crazy?

## JS to the rescue?

It's an awkward rescue, like having a donkey to help you move into a new house. You would have preferred a strong friend and a moving truck. But a donkey *can* help, in its own unique way. 

Here's a basic "by the book" way of creating dynamic elements with pure JavaScript. 

```js
var people = ['Alice', 'Bob', 'Cat']

people.forEach(function(person){
  var element = document.createElement('li')
  element.innerText = person
  element.className = 'person'
  document.getElementById('people').appendChild(element)
})
```
Before executing the JavaScript, our HTML features a lonely `<ul>`, mouth open to the sky, waiting. Just waiting.

```html
<ul id="people"></ul>
```

This implementation is comically simplified. Don't think you could really stop here. We are just looking at dynamic *creation* and completely leaving out edits and deletion. In real world applications with complex logic, we're gonna write a lot more code, and if we're not careful, soon we'll be knee deep in spaghetti! Bon appétit! 🍝

## Templating Languages

Developers created templating languages to make up for HTML's shortcomings. These are alternative languages that are nice for humans to write dynamic elements. But browsers can't read these alternative languages, only HTML, so they need a translator. We call that rendering - when a program sticks your data into your template and spits out HTML.

### Pug
Here's Pug, a popular templating language. Isn't that nice and short?

```
- var people = ['Alice', 'Bob', 'Cat']

ul#people
  each person in people
    li.person= person
```

### ERB

ERB is one of my favorites. It adds the full power of Ruby into the HTML you already know.

```erb
<% people = ['Alice', 'Bob', 'Cat'] %>

<ul id="people">
  <% for person in people %>
    <li class="person"><%= person %></li>
  <% endfor %>
</ul>
```

### JSX

JSX is not so much an HTML templating language as it is a React templating language. It's more concerned with *interactive components* than *dynamic HTML*, but it's good for both.

```jsx
export default function App() {
  var people = ["Alice", "Bob", "Cat"];

  return (
    <ul id="people">
      {people.map(function (person) {
        return <li>{person}</li>;
      })}
    </ul>
  );
}
```

### Everything has a cost

Templating languages are loved by millions of developers, but they always have a cost. They add complexity. They add dependencies. Pick wisely, and the benefits far outweigh the costs. Pick poorly, and you may create new problems for yourself down the line, but still benefit compared to using raw HTML.

So, when might you want to not use a templating language at all?

<h2 id="danger">Let's get dangerous</h2>

Let's say you're making an app for yourself, for fun.

And you don't want to write imperative javascript like I showed in the pure javascript example. That was the donkey analogy one.

And for whatever reason, you want to try making it with 0 external libraries.

And security is **not a factor**.

Then boy, do I have an idea for you.

## Never use Eval()!

You know it's going to be fun when the MDN page has a [whole section telling you not to use it](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!).

I didn't even know there was an `eval` function in JS, and I've been writing it for years. It's like finding a forbidden artifact.

```js
var monster = null
eval('monster =' + '"franken' + 'stein"')

console.log(monster) // 'frankenstein'
```

Eww what is that? Eval simply takes any string you pass in and executes it like real javascript. It's simple but dangerously powerful, *especially* if you use it with user-generated data. And that's *exactly* how we'll use it. 😎

## But really, be careful

Here's the deal with `eval`. It's a red carpet for malicious 3rd party code. In my app, there aren't any untrusted 3rd parties. Interaction between users is impossible, so someone can't set their First Name to 'initiateSelfDestruct()' to blow up your phone when you visit their profile. They could blow up their own phone, but whatever, their call.

I don't run ads, so no shady code can slip in there. There are no browser extensions for it. If any of those things changed, I'd be better off removing `eval`.


