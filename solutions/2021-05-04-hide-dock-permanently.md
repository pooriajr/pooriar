---
title: Hide OSX dock permanently
permalink: hide-osx-dock-permanently.html
---

I don't need the dock on OSX at all. I use Alfred to launch apps and open folders, and the Command-Tab task switcher to switch and see what's running.

So the dock is just a nuisance for me, and even when I hide it, it likes to pop up in my way when I try to click something at the edge of the screen. 

I finally got tired of it and fixed the problem with a simple terminal command

```
defaults write com.apple.dock autohide-delay -float 42; killall Dock
```

The important thing here is that this doesn't actually disable the Dock. The dock has some important functions so we can't do that.

Instead this just increases the delay before the dock would normally show.

And this requires that you have Auto-Hide *ON* in the dock settings.

By setting that number to 42 or any other large number, you effectively eliminate any chance of the dock popping up.

The `killall Dock` command simply restarts the Dock to allow the new delay time to take effect.

If you want to reverse this, you can reset the delay to the system standard (0.5 seconds)

```
defaults write com.apple.dock autohide-delay -float 0.5; killall Dock
```

At the time of writing, this works for me using Mojave, version 10.14.6
