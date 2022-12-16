---
layout: post
title: Why bother with a VPS?
date: 2022-12-16 17:12 -0500
---

Doing it yourself is so more time consuming and error-prone than just running `git push heroku`, so why even bother?

### Save tons of money 🤑
The main benefit. The idea is that a little upfront investment of time is going to save you BIG BUCKS over the course of a long career making many web projects. 

I see it all the time; people with $40 VPS's happily serving *multiple high traffic sites* simultaneously without breaking a sweat. $40 won't even get you a *single* 1GB RAM Heroku dyno, and that's not even talking about addons yet. The difference is massive.

### Give your unprofitable projects room to breathe
Raise your hand if your Heroku account is a graveyard of apps that you kept alive for a month or two, but which didn't make enough money to justify continuing to pay for them. 🙋‍♂️

With a VPS you can keep ALL your projects alive indefinitely without increasing your hosting bills.

### Take advantage of subdomains
Raise your hand if you've got tons of old side projects which can't be accessed because you let their domain expire. 🙋‍♂️

With a VPS, you're in control of your own webserver, which means you can use subdomains to keep a project online with a nice address, without paying yearly domain fees.

For example, my personal site is "pooriar.com", and as long as I keep paying for this ONE domain, I can host infinite side projects on its subdomains, like so: "myproject.pooriar.com", "myotherproject.pooriar.com", etc

And if that sounds strange or complicated, nah, it's stupid easy and simple when you configure your own webserver.

### Persistent storage
Heroku's ephemeral storage forces you to use other services for anything that needs to be persistent. Something as simple as accepting file uploads, in the year of our lord 2022,  you have to use AWS or some other such service. What a chore.

Same with your database. Wanna keep it simple and use SQLite? Sorry, can't do that, how about a **Fully Managed Postgres Server™** (That'll be $50/month, thank you very much 💀)

Look, I know that there are very good reasons for using these services, but sometimes you just want to keep it simple (and free), which a VPN lets you do.

### Take control of your own destiny, maaaan
Raise your hand if you've deployed sites to some service (*cough netlify cough*), and within a few years they forced some upgrade on everyone which broke your app. 🙋‍♂️ 

I can create enough headaches for myself, I don't need someone else doing it for me. With a VPS I don't have to worry as much about things outside of my control, such as forced upgrades and greedy price hikes.

### Learn something useful
I know time is short and we're all in a rush to get shit done™ and not waste time with anything unnecessary, but I think it's pretty cool to have a fuller understanding of how websites really work. I've been a web developer for 7 years, have deployed dozens of sites, but I haven't had to ssh into a remote server until just last week, with this VPS.

That's a big testament to the quality of tools available to make my job easy, and I am not some anti-convenience puritan who thinks we should all do everything the hard way.

But, within reason, learning fundamentals pays off. It makes you more resourceful and self-sufficient, less reliant on someone else to sugarcoat and spoon-feed you.

And I can say that personally, learning this stuff feels very empowering. I'm having fun and feeling slightly more like a crafty, unstoppable hacker. 👨‍💻

## What about other PaaS providers? (Render, Railways, etc)
I've tried a few of the cheaper Heroku alternatives people throw around. I found they haven't matured enough to give you the turnkey convenience of Heroku (which is probably why they're cheaper). 

So while they are still fiddly, it doesn't matter that they're a bit cheaper, because If you're going to have to fiddle to get stuff working, might as well do that fiddling on your own VPS and reap the benefits.

Are they perfect for some use cases? Sure, go ahead and experiment to find out. Will they reach Heroku-level convenience one day? Hopefully. Will they then jack up their prices? Probably. 

## Why NOT to use a VPS
I should be clear that I don't think managing your own VPS is right for everyone. If you have a profitable application running on Heroku, stick with that. If you prioritize speed of development, or consistent convenience above all else, stick with Heroku. 

But for me personally, as much as I love Heroku's service, I want to make lots of little projects which may or may not be profitable, without having to sell my house to keep them online for the foreseeable future. (I don't have a house)

And I'd like to have the freedom to choose simplicity and sustainability, rather than having a platform force me into something that's more complicated than what I need.

## But how can we do this VPS stuff without it... you know... sucking...
Word. I'm extremely hassle-avoidant. I hate being stuck in some cryptic tangle of configuration bullshit. That stuff just makes me give up and walk away. 

Unfortunately, there's a bit of that, can't be avoided. We are going from an idiot-proof environment (Heroku) to the realm of linux devops nerds 🤓. This is unfamiliar territory for me, so I'm figuring out the best way to reach a sane, happy setup as directly as possible. I'm vigilant to avoid adding any tools or processes that add more complexity than they're worth. 

I know I'm making choices that an experienced devops guy would cringe at. Breaking best practices, cutting corners, fuck yeah[^1] 🤘. The point is to learn incrementally and intentionally, as little as is necessary to solve the next problem, and no more.

If you want to learn along with me, I'll be sharing my process as I go.

---

[^1]: As long as my websites aren't keeping people alive or handling sensitive data, I'm okay with some breakage

