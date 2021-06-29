---
layout: post
title: Dangerously dynamic HTML
date: 2021-06-25 20:47 -0500
---

HTML isn't designed for dynamic content, and it shows. Templating languages and front end libraries pick up the slack. But what if you want dynamic HTML without dependencies?

## Why be dynamic at all?

Let's say you're making an app to keep track of personal relationships. You probably need to create HTML elements that correspond to various people. 

I want to add 3 people into the app: My friend Alice, my coworker Bob, and my Cat (yeah that's not a person but I was going for an A,B,C thing).

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

A dynamic version of this would allow anyone to add their own list of people, and edit it without me, the programmer, having to touch code. Dynamism is crucial in the world of websites, and anything fancier than a digital brochure needs it.

## JS to the rescue?

Here's a basic "by the book" way of creating dynamic elements with pure JavaScript. It's awkward, but it works.

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

If your app doesn't have a lot of dynamic elements, the example above might be a good route. It adds 0 dependencies to your code. You can inline it in your HTML and serve it as-is without a hint of build tooling or additional backend dependencies.

If your app is highly dynamic, then you should get help from an appropriate templating language or library. The added dependency will be well worth it.

But what if you want the power of a templating language without the added dependency?

## Don't try this at home

The method I'm going to share is simple and powerful. It lets you generate dynamic HTML content with no dependencies. It uses JavaScript, but unlike the example I showed earlier, it lets you keep your HTML as HTML, rather than turning it into a series of DOM methods and assignments. 

The downside is that this method is a security nightmare. It uses the `eval` function, which has a [whole section on MDN telling you to never use it](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!)

But if you're making an app which doesn't involve sensitive data, or which no one else uses, this is a fast and fun way to prototype dynamic HTML.

## Eval, JavaScript's dirty secret

It's funny that I didn't even know there was an `eval` function in JS until recently, and I've been writing it for years. While learning Ruby, `eval` was in every beginner tutorial. But in JavaScript no one talks about it.

So how does it work?

```js
var monster = null
eval('monster =' + '"franken' + 'stein"')

console.log(monster) // 'frankenstein'
```

Eval takes any string you pass in and executes it like real javascript. That's it? Why all the fuss? 

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

Notice how there's not one `if` statement in that code, and yet we create 3 unique strings based on the `gender`, with a fallback for when it's undefined. Pretty neat right? But there's a vulnerability here. What if a malicious user signs up and inserts a chunk of valid javascript as their "name":

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
// safe ... for now
function logIntro(user) {
    var introFunction = eval(`$user.gender`Intro)
    var intro = introFunction(user.name, user.hobby)
    console.log(intro)
}

```

It's  Instead we use `eval` programmatically to peice together the name of the appropriate function to call. It even has a neutral fallback for when `gender` is undefined. This is a type of metaprogramming that is popular in some other languages like Ruby. 

But 



If an application uses  In my app, there aren't any untrusted 3rd parties. Interaction between users is impossible, so someone can't set their First Name to 'initiateSelfDestruct()' to blow up your phone when you visit their profile. They could blow up their own phone, but whatever, their call.

I don't run ads, so no shady code can slip in there. There are no browser extensions for it. If any of those things changed, I'd be better off removing `eval`.


