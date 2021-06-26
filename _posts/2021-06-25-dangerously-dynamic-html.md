---
layout: post
title: Dangerously dynamic HTML
date: 2021-06-25 20:47 -0500
---
Heads up, this article tries overly hard to be beginner-friendly. It starts with an explanation of static vs. dynamic. Then there's examples of a few standard ways you might create dynamic HTML if you're a responsible developer. 

If you want to skip straight to danger, [right this way 🤘.]()

## What is dynamic, *really*?

Let's say you're making an app to help keep track of personal relationships (\*cough\* [undrift.netlify.app](undrift.netlify.app) \*cough\*). You probably need to create HTML elements that correspond to various people. 

Let's say I want to add 3 people into the app: My friend Alice, my coworker Bob, and my Cat (ok that's not a person but I was going for an A,B,C thing).

I'm the programmer, goddamnit, so I can just write them into the HTML like so:

```html
<ul id="people">
  <li class="person">Alice</li>
  <li class="person">Bob</li>
  <li class="person">Cat</li>
</ul>
```
But if I want to add a person, or change a name, or remove one, I have to go edit my code every time. More importantly, my app is quite useless for you or anyone else, unless you also have an Alice, Bob, and Cat in your life. 

This is *static*, the opposite of dynamic.

A dynamic version of this would allow anyone to add their own list of people, and edit it to their leisure without me, the programmer, having to touch code.

As you can imagine, so it's pretty dang important in the world of websites, and anything fancier than a digital brochure will need dynamic elements.

# A million ways to fill the gap

HTML doesn't offer a great way to create dynamic elements. The language was designed to show documents. Like literally, digital brochures. And now here you are wanting to write a fancy dynamic application with it? You crazy?

## JS to the rescue?

Ok well this is what Javascript is for right? Interactivity? Please?

Well, yes. But it's not exactly pretty. Here's a basic "by the book" way of creating dynamic elements in pure Javascript. 

```js
var people = ['Alice', 'Bob', 'Cat']

people.forEach(function(person){
  var element = document.createElement('li')
  element.innerText = person
  element.className = 'person'
  document.getElementById('people').appendChild(element)
})
```
Before executing the JavaScript, our HTML features a lonely `<ul>`, mouth open to the sky. Waiting, just waiting.

```html
<ul id="people"></ul>
```

This implementation is comically simplified for your sake and mine. We are just looking at dynamic *creation* and completely leaving out edits and deletion. In real world applications with complex logic, this approach often transforms from code into spaghetti. Bon appétit! 🍝

## Templating Languages

HTML's shortcomings in this department have led to decades of developers deciding they can do better, and creating "templating languages". These are alternative languages that are nice for humans to write dynamic elements. But browsers can't read these alternative languages, only HTML, so they get translated by some other program, such as a build script running on a server.

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

Templating languages are loved by millions of developers, but they always have a cost. They add complexity. They add dependencies. And it's usually totally worth it.

So, why wouldn't you use one?

## Let's get dangerous

Let's say you're making an app that keeps track of personal relationships (cough undrift.netlify.app cough).

It's just for fun. There's no stakes.

And for whatever reason, you want to try making it with 0 external libraries.

And security is **not a factor**.

Then boy, do I have a fun idea for you.

## Never use Eval()!

You know it's going to be fun when the MDN page has a [whole section telling you not to use it](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!).

I didn't even know there was an `eval` function in JS. We're off the map.

```js
var monster = null
eval('monster =' + '"franken' + 'stein"')

console.log(monster) // 'frankenstein'
```

Eww what is that? Eval simply takes any string you pass in and executes it like real javascript. It's simple but dangerously powerful, *especially* if you use it with user-generated data. And that's *exactly* how we'll use it. 😎

## But really, be careful

Here's the deal with `eval`. It's a red carpet for malicious 3rd party code. In my app, there aren't any untrusted 3rd parties. Interaction between users is impossible, so someone can't set their First Name to 'initiateSelfDestruct()' to blow up your phone when you visit their profile. They could blow up their own phone, but whatever, their call.

I don't run ads, so no code can slip in there. There are no browser extensions for it. If any of those things changed, I'd be better off removing `eval`.


