---
layout: post
title: 'Dynamic "Time Ago" in Jekyll'
date: 2021-05-07 15:46 -0500
slug: dynamic-time-ago-in-jekyll
---
My site has a Recent Writing section which shows my latest blog posts. Rather than displaying "Published May 5, 2020", I want to show "Published 2 days ago":

![](/assets/images/posts/dynamic-dates-1.png)

There's a [Jekyll plugin](https://github.com/markets/jekyll-timeago) for this, with a catch. It only sets the times when Jekyll builds the site, and can't update them until the next rebuild. This is the nature of a static site generator.

I'm hosting the site on Netlify, and it rebuilds each time I push a commit to `master` on the Github repo.

If I push a commit with my new post to Netlify on Monday, the site will correctly display "Published today". 

If I don't push more commits, on Tuesday it will **still** say "Published today". Same for Wednesday. It won't update until I commit and trigger a new build. 

I need an automatic way to trigger a Netlify build once daily, so all my "Time Ago" dates stay updated and accurate. 

We're in luck, because Github and Netlify both provide free tools that make this easy.

What we have to do is:
1. Add a "build hook" in Netlify that can trigger a build programmatically
2. Set up a Github Action that triggers the Netlify build hook on an automatic daily schedule.

## Creating a Netlify build hook

Go into deploy settings and find the "Build hooks" section.

Create a new build hook. Mine is called "REBUILD_CRON" because we're going to set up a "cron job" in the next step to schedule our Github action.

Netlify helpfully provides a `curl` command once you've created your build hook, hit the little caret in the corner to see it.

![](/assets/images/posts/build-hooks.png)

## Scheduling the Github Action

Github actions have a lot of functionality, but right now we're only interested in scheduling and running a command-line.

We need to add 2 directories and a YAML file to set up Github actions:

:t the root level of your Jekyll site's git repo, create a `.github` directory. Add a `workflows` directory inside `.github`. Inside `workflows`, you need a YAML file. Call it whatever you like; mine is `github-actions.yml`

```
├── .github
│   └── workflows
│       └── github-actions.yml
```

Add the following code to `github-actions.yml` , replacing the last line with the `curl` command that Netlify provided in step 1

{% highlight yaml linenos %}
name: Daily Netlify Build

on:
  schedule: 
    - cron: "0 0 * * *"

jobs:
  netlify-build:
    runs-on: ubuntu-latest
    steps:
        - run: curl -X POST -d {} https://api.netlify.com/build_hooks/60965c4992791520b1620110
{% endhighlight %}

A few points:
1. `cron: "0 0 * * *"` is a scheduler that represents once a day at midnight, in the UTC timezone.

2. The `Daily Netlify Build` on line 1 and `netlify-build:` on line 8 are just names I chose, you can change them to whatever you want.

3. The `curl` command sends an HTTP POST request to the build hook URL, which triggers the build. 

Stage the new file, commit, and push to Github. You should see your new action on your repo's Github page now.

![](/assets/images/posts/github-actions.png)

## Sit back, relax

That's all! Now you can wait for 00:00 UTC time and watch your site get automatically rebuilt with proper "Time Ago" labels. 

Thanks for reading!
