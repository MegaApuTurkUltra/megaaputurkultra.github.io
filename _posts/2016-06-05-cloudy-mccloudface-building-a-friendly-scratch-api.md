---
layout: post
title: "Cloudy McCloudFace: Building a friendly Scratch API"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[Cloudy McCloudFace](https://scratch.mit.edu/projects/109772772/) is a project I made that provides an API for making realtime cloud multiplayer games in Scratch. The reasoning behind it was that Scratch makes it really, really difficult to do cloud multiplayer with its quirks, and I think Scratchers have the potential to make cool multiplayer projects if only they were able to do so without having to deal with quirks.

It's designed a lot like APIs in other languages, with API calls and callbacks. Here's the "user" code that runs the demo I made.

![Scratchblocks for CMCF demo](http://image.prntscr.com/image/95bd9d5b3c184ce991493e4d239d9e27.png)

This post will basically go over how it works and the design of what I consider to be a friendly Scratch API.

## The API ##

The API itself is somewhat self explanatory. The two major premises behind it are using callbacks and (ab)using Scratch variables as a hashmap. For example, in Javascript you might see an API like this

```javascript
connectToServer(function success(){}, function error(){})
```

Similarly, CMCF uses callbacks for connection success and error.

The hashmap part is the way the API handles cloud data. Basically, there are psuedo-variables that each connected multiplayer user has a copy of. You set your own copy of your psuedo-vars, and recieve them from other players. For example, for a 2d game you might have the psuedo-vars "x position," "y position," and "direction." These are the variables that are cloud-list-encoded into each user's cloud variable. To make the API easy to use, I wanted to allow naming these variables instead of simply indexing into the encoding list. So instead of
<pre class="blocks">
set my psuedo-var (1) to [something]::custom
</pre>
CMCF has
<pre class="blocks">
cmcf.SetVar [x position] [something]::custom
</pre>
where "x position" is the name of your pseudo-variable.

Obviously, Scratch is limiting, but the best APIs are the ones that give you the most freedom. And with hacked blocks, it's possible to make things pretty friendly.

## The Hacks ##

In terms of function, CMCF is very similar to what I did in [Jetski Escape](https://scratch.mit.edu/projects/47027704/), but the code is a bit more cleaned up.

### Cloud List ###

For cloud list encoding and decoding, I essentially built a new cloud list engine from Scratch (pun intended - heh). It's very concise (if you don't count the sprite that contains all the ascii code variables)

![cmcf.decode block](http://image.prntscr.com/image/e77e28f744dc47a6b70a4958dc5794fc.png)

![cmcf.encode block](http://image.prntscr.com/image/c4b604b7a3cd4a6bbedce5de09328c36.png)

cmcf.encoder is a sprite that contains 2 types of variables for all ascii characters in the range 0-127

- Variable is named for its character; value is 2 digit hexadecimal ascii code
- Variable is named for its 2 digit hexadecimal ascii code; value is character

This allows for efficient encoding and decoding using Scratch's variables hashmap.

I don't think the decoder needs much explanation. 0xFF is what I use to signify a new list item, but other than that it simply goes through the variable (skipping the "0x") and decodes by pairs of hexadecimal digits.

The encoder is a bit more complicated. It uses the list hack to allow arbitrary length lists to be encoded without the 10k character limit on the join block. Individual characters are added to the list one at a time, and the list reporter then joins them. The only catch is that inserted characters must explicitly be casted to a string. A single number as a list item will screw it all up and space separate the characters in the list reporter. The script uses the join block to cast numbers to strings.

Both blocks are hacked with menus (obviously), so feel free to use just the cloud list engine of this project if you want to.
&lt;/shameless-plug&gt;

### Keeping track of users and establishing presence ###

CMCF needed to be robust, and that means keeping track of when people leave the room, and detecting which cloud variable you can hop on to when connecting. To keep track, I assumed that "cloud variable updated within the last 2 seconds" means that someone is on that variable. CMCF has a list called "updatedt" which counts the seconds since the last update for each cloud var. To do this, it caches the last known value of the variable and then compares.

Now here's our second super-annoying Scratch quirk: long numbers can be "equal" when they aren't really (because of loss of precision).

![Example of long numbers being "equal"](http://image.prntscr.com/image/50b685074f2c4a409590b0b75f268582.png)

And even more annoyingly, Scratch will aggresively cast your strings back to numbers if you aren't careful.

![Example of aggressive casting](http://image.prntscr.com/image/b6a7b4df88db42d39627fdcb4ec94d4d.png)

The solution is to append letters to the beginning of the number.

![Example of how to solve](http://image.prntscr.com/image/6e8bc236572348979c0afecf6bde4407.png)

Now, to make sure that your cloud variable *is* updated even if the psuedo-vars aren't being updated is to add a random number to the cloud list and resend it as often as possible.
<pre class="blocks">
add (username) to [my list v]
add (pick random (0.01) to (1.01)) to [my list v]
add all the psuedo-vars::grey
encode::custom
</pre>

Update time is kept track of using the days since 2000 block, so the timer is free for other usage.

### Sending and receiving data ###

Sending data is pretty straightforward. The API has blocks for setting your own variables, or you can directly set the (cmcf.myslot) item of the list for the psuedo-variable you want to modify. Each psuedo-variable is stored locally as a list, where indicies in the list correspond to cloud variable IDs for each user. So (&#9729; cloud1) is in item 1, (&#9729; cloud2) in item 2, etc.

When receiving data, CMCF updates the items in these lists, so they can easily be accessed (or you can use the get variable API block).

### Connecting and the fallback ###

Here's the last Scratch quirk: the HTTP fallback mode.

Now this mode is really annoying because when even one person is on the fallback mode, not only do they lose the ability to spam cloud updates as fast as possible, but *everyone else* who's connected also loses that ability. So, we need to make sure that nobody is on the fallback mode. The connection procedure looks like this:

- Start caching cloud variables and checking for changes
- Pick the first open cloud var (one that hasn't changed)
- Wait for other players to join
- Start spamming your cloud var for a few seconds
- If you stop receving updates, you're on the fallback
- Otherwise, you're connected!

Fallback testing is pretty simple

![Fallback tester script](http://image.prntscr.com/image/191fef18aa83464d8a4be1507f0880b8.png)

And here's the script for connecting

![Connecting script](http://image.prntscr.com/image/49db94e2b5a444d0a85ce034b0cf8c23.png)

I tried to make things as readable as possible. Obviously with the hacks and all it isn't always possible, but I tried my best.

## Conclusion ##

Basically, here are the stupid things I had to work around here

- The 10k join limit
- Numbers being "equal"
- The HTTP fallback

And here are the non-stupid things I had to work around

- Efficiently encoding and decoding cloud lists
- Checking for people online

I encourage you to try and make a Scratch API too! There are so many things in Scratch where quirks provide a ridiculous bar for users to be able to use them in projects. Having things in fluent APIs is nice for people on Scratch who are just learning about advanced projects, and it's a great learning experience for the maker as well :)