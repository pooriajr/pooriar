---
layout: post
title: Dangerously dynamic HTML
date: 2021-06-28 20:47 -0500
---

HTML isn't designed for dynamic content, and it shows. Templating languages and front end libraries pick up the slack. But what if you want dynamic HTML without dependencies?

Heads up! This article is beginner friendly and has a lot of setup. If you don't care to read about dynamic vs static, imperative DOM manipulation, and `eval`, you can [skip right to the punchline](#anchor). OK, let's proceed!

## Why be dynamic anyway?

Let's say I'm making an app to keep track of personal relationships. I need to create HTML elements that correspond to various people. I'll add 3 people into the app: My friend Alice, my coworker Bob, and my Cat (yeah that's not a person but I was going for an A,B,C thing).

I'm the programmer, goddammit, so I can just write them into the HTML like so:

```html
<ul id="people">
  <li class="person">Alice</li>
  <li class="person">Bob</li>
  <li class="person">Cat</li>
</ul>
```
But if I want to add a person, or change a name, or remove one, I have to go edit my code every time. And this is totally useless for anyone else, unless you just so happen to also have an Alice, Bob, and Cat in your life. 

This is *static*, and it's the opposite of dynamic.

A dynamic version of this would allow anyone to add their own list of people, and edit it without the programmer having to touch code. Dynamism is crucial in the world of websites, and anything fancier than a digital brochure needs it.

## JS to the rescue?

Here's a basic "by the book" way of creating dynamic elements with pure JavaScript. I wouldn't call it elegant, but it works.

```js
var people = ['Alice', 'Bob', 'Cat']

function renderPerson(person){
  var element = document.createElement('li')
  element.innerText = person
  element.className = 'person'
  document.getElementById('people').appendChild(element)
}

people.forEach(renderPerson)
```
Before executing the JavaScript, our HTML features a lonely `<ul>`, mouth open to the sky, waiting. Just waiting.

```html
<ul id="people"></ul>
```
This implementation is *really simplified* for brevity. It's just the tip of the iceberg. We are handling dynamic *creation* but completely leaving out edits and deletion. In real world applications with complex logic, this approach can quickly turn to spaghetti. Bon appétit! 🍝

## Templating Languages

If your app doesn't have a lot of dynamic elements, the example above might be enough. It adds 0 dependencies to your code. You can serve it as-is without a hint of build tooling or additional back-end dependencies.

On the other hand, if your app is highly dynamic, then you should use an appropriate templating language or library. The added dependency will be well worth it.

But what if, for whatever reason, you want the power of a templating language without the added dependency?

## Don't try this at home

The method I'm going to describe is simple and powerful. It lets you generate dynamic HTML content with no dependencies. It uses JavaScript, but unlike the example I showed earlier, it lets you keep your HTML as HTML, rather than turning it into a bundle of DOM methods and assignments. 

The downside is that this method is a security nightmare. It uses the `eval` function, which has a [whole section on MDN telling you to never use it](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!).

But if you're making an app which doesn't involve sensitive data, or which no one else uses, this is a fast and fun way to prototype dynamic HTML.

## Eval, JavaScript's dirty secret

It's funny that I didn't even know there was an `eval` function in JS until recently, and I've been writing it for years. While learning Ruby, `eval` was in every beginner tutorial. But in JavaScript no one talks about it.

So how does it work?

```js
var monster = null
eval('monster =' + '"franken' + 'stein"')

console.log(monster) // 'frankenstein'
```

`eval` takes any string you pass in and executes it like real JavaScript. That's it? Why all the fuss? 

`eval` is a red carpet for malicious 3rd party code. Take this code:

```js
users = [
  {name: 'Alice', hobby: 'coding',  gender: 'female'},
  {name: 'Bob',   hobby: 'writing', gender: 'male'},
  {name: 'Cat',   hobby: 'scratching'}
  ]

function maleIntro(name, hobby) {
  return `His name is ${name}. He likes ${hobby}.`
}

function femaleIntro(name, hobby) {
  return `Her name is ${name}. She likes ${hobby}.`
}

function undefinedIntro(name, hobby) {
  return `Their name is ${name}. They like ${hobby}.`
}

function logIntro(user) {
    var introFunction = eval(`{$user.gender}Intro`)
    var intro = eval(`${user.gender}Intro("${user.name}", "${user.hobby}")`)
    console.log(intro)
}

users.forEach(logIntro)
// Her name is Alice. She likes coding. 
// His name is Bob. He likes writing. 
// Their name is Cat. They like scratching.
```

Notice how there's not one `if` statement in that code, and yet we create 3 unique strings based on the `gender`, with a fallback for when it's undefined. Pretty neat right? But there's a vulnerability here. What if a malicious user signs up and inserts a chunk of valid JavaScript as their "name":

```js
users = [
  {name: '"); while(true){window.alert()}; ("', hobby: 'haxx'
  {name: 'Alice',                               hobby: 'coding',  gender: 'female'},
  {name: 'Bob',                                 hobby: 'writing', gender: 'male'},
  {name: 'Cat',                                 hobby: 'scratching'}
  ]
```

That's an infinite loop, and now `eval` will run this code, locking up the page. 

```js
undefinedIntro(""); while(true){window.alert()}; ("", "haxx")
```

This is a simple example, but with total freedom to run any JavaScript they want, they can do much worse than lock up the page. 

We can avoid this vulnerability by only ever using `eval` with strings that we know we can trust. So we could refactor the `logIntro` function like so:

```js
// vulnerable
function logIntro(user) {
    var intro = eval(`${user.gender}Intro("${user.name}", "${user.hobby}")`)
    console.log(intro)
}
// refactored to be safe ... for now
function logIntro(user) {
    var introFunction = eval(`$user.gender`Intro)
    var intro = introFunction(user.name, user.hobby)
    console.log(intro)
}
```

And as long as we have a safe set of strings as options for `gender`, then we can use this safely. But code changes so what is safe today may accidentally become unsafe in the future. `eval` is just higher risk, so people usually opt for an alternative.

<h2 id="anchor">Using Eval for native HTML templates</h2>

It's really a shame `eval` is so dangerous because it is a *beautiful* solution to writing dynamic HTML without dependencies. Let's rewrite the standard JS implementation from earlier - here it is again as a refresher:

### Standard

The HTML is an empty shell without a hint of the dynamic HTML to come:

```html
<ul id="people"></ul>
```
The JavaScript, a soup of functions and assignments:
```js
var people = ['Alice', 'Bob', 'Cat']

function renderPerson(person){
  var element = document.createElement('li')
  element.innerText = person
  element.className = 'person'
  document.getElementById('people').appendChild(element)
}

people.forEach(renderPerson)
```

### Dangerous

The HTML uses a `<template>` tag to define the exact shape of the future HTML, without rendering it on page load. Dynamic parts are defined using JavaScript's string interpolation syntax: `${expression}`

```html
<ul id="people">
</ul>

<template id="personTemplate">
  <li class="person">${person}</li>
</template>
```

The JS doesn't concern itself with the details of the markup. The `eval` statement converts a standard string into an interpolated string, inserting dynamic values as plain old JavaScript expressions. It converts the string to HTML and inserts it in the DOM at the exact location of your choice using the delightfully flexible `insertAdjacentHTML` method.

```js
var people = ['Alice', 'Bob', 'Cat']

function renderPerson(person){
  var template = document.getElementById('personTemplate')
  var HTMLstring = eval("`" + template.innerHTML + "`")
  document.getElementById('people').insertAdjacentHTML('afterbegin', HTMLstring)
}

people.forEach(renderPerson)
```

### Why I love it

This approach shines when you use it as a system for rendering all templates across an application. Instead of creating a unique render function for every type of template, you can create a general purpose template rendering function:

```js
function renderTemplate(templateId, destinationId, data, position){
  var template = document.getElementById(templateId)
  var HTMLstring = eval("`" + template.innerHTML + "`")
  document.getElementById(destinationId)insertAdjacentHTML(position, HTMLstring)
}
```
Now updating existing templates or creating brand new ones is easy as pie; just write your `<template>` elements in the HTML while respecting the string interpolation syntax. Markup changes are separated from JS changes. Best of all, your templates are consistent and predictable, free of spaghetti. Muah! 😚👌

And remember that you can put *any* JS expressions inside your HTML. You can use conditionals, or even inject dependencies into inline functions. There is a ton of power and flexibility in this approach. All without a single dependency.

## But the security...

Maybe you're thinking, "That's cool and all, but the security risks make it useless in any *real* application, so what's the point? Just bite the bullet and add a dependency that solves the problem safely." 

For a serious project you'd be totally right. But if you're making a proudly trivial app, a fun personal project, a scrappy no-stakes MVP, or you just feel like testing the limits of vanilla HTML and JS, I think this method is worth knowing about. 

I'm using it myself on a [personal project](https://undrift.netlify.app)(still in progress), and I enjoy the simplicity, while knowing full well that I'd have to change it if the app ever became a serious project that other people use. But mostly likely it wont. And either way, I'm OK with it.
