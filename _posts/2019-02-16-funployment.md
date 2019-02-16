---
layout:     post
title:      Real World Interaction on Deployment
date:       2019-02-16
summary:    Creating real world interactions when deploying to master
categories: pi raspberry raspberry-pi deployment codeship fun funployment node python 
---

# Interfacing With The Real World On Deploying

## Why tho'?

When I started at ProdPad, my colleague had an idea about using a raspberry pi to interact with the real world so we could know when a deployment was successful. I thought to myself 'that's a cool idea, and it's super easy...'. But I only just started my new position and I was overwhelmed, so after 3-4 months, when the dust settled I decided that I had nothing better to do one weekend and threw something together in a few hours.

## Some considerations

At ProdPad we use Codeship as a CI/CD deployment service, and they have an public accessible API that you can use which is great for this; to my knowledge they also have no limit on api calls so you can actually request build states pretty often. We've been considering switching to another service, so this was something I had to think about. What if I did all this hard work and we just upped and changed service just as I finished?

So, I wanted this to be open to any CI/CD. For my use, I only created a Codeship integration but this it's really easy to add others so if we do change I only have to create a new script for that service. 

## Where do I want to look
So we want to find out when there is a successful build, we can look at any state in codeship (failed, started, success) and monitor this; we can even monitor exact branches (just a note, you can't target branches and exact builds with their front end image url states; I confirmed this with their support), however you can do this in the API!

![Image%202019-02-16%20at%202.17.30%20pm.png?X-CloudApp-Visitor-Id=3134923](https://d2ddoduugvun08.cloudfront.net/items/3C2m3B1p0g0K0Q20330Z/Image%202019-02-16%20at%202.17.30%20pm.png?X-CloudApp-Visitor-Id=3134923)

## How do we monitor the states
Every few seconds we ping a GET request to the Codeship API to get all their builds.

![Image%202019-02-16%20at%202.23.18%20pm.png?X-CloudApp-Visitor-Id=3134923](https://d2ddoduugvun08.cloudfront.net/items/3A3g0h3d3r2Q2g3O0e1D/Image%202019-02-16%20at%202.23.18%20pm.png?X-CloudApp-Visitor-Id=3134923)

And what comes back down is the build states, great! We can use this to start a list of builds.

![Image%202019-02-16%20at%202.24.17%20pm.png?X-CloudApp-Visitor-Id=3134923](https://d2ddoduugvun08.cloudfront.net/items/1J0c3y1y3E3Z0O431q3e/Image%202019-02-16%20at%202.24.17%20pm.png?X-CloudApp-Visitor-Id=3134923)

Now we can start storing them. 

![Image%202019-02-16%20at%202.26.09%20pm.png?X-CloudApp-Visitor-Id=3134923](https://d2ddoduugvun08.cloudfront.net/items/3c0c2e3Q1f0n34092o2D/Image%202019-02-16%20at%202.26.09%20pm.png?X-CloudApp-Visitor-Id=3134923)

Codeship will only send you the top 30 newest builds, for simplicity sake we say they only send the newest 3. So we call again, and it looks like someone has sent up some new changes to the repository. Cool! These are new builds, we can match any existing uuid's in the new call and see if they exist in their current build list to make sure we aren't checking old builds. Any that do match, we throw them away since we don't care (or we could update them if we fancy the extra effort). So we take all these new builds and start monitoring them.

![Image%202019-02-16%20at%202.28.53%20pm.png?X-CloudApp-Visitor-Id=3134923](https://d2ddoduugvun08.cloudfront.net/items/2t1Y311v2Z3j3o2R0D1P/Image%202019-02-16%20at%202.28.53%20pm.png?X-CloudApp-Visitor-Id=3134923)

So we store the new ones in 'new builds'. These are all we care about, we can now do some fancy magic and decide what to do with them. Obviously if it's successful we know we can start any real world interfaces. But this isn't usually the case since we're updating fairly often. So, let's consider the started build.

![Image%202019-02-16%20at%202.31.01%20pm.png?X-CloudApp-Visitor-Id=3134923](https://d2ddoduugvun08.cloudfront.net/items/2Q2p3W3E1r2j3S1N1J0s/Image%202019-02-16%20at%202.31.01%20pm.png?X-CloudApp-Visitor-Id=3134923)

Again, we make another call. This time our build 'Y' has changed in this new call. It's finished! Again we do some fancy matching magic with uuid's and decide it's a build we care about. Since it's changed to successful we probably want to move it into builds and then fire up our real world interfaces. 

## Ok, but we have another problem?
We can recognise that we have some new builds, and we could use the function that handles this logic to fire up to every real world interface. But that's a bit shit really - we'd have to modify this code _every_ time we wanted to make a new one. No one wants to do that - and it violates the open/closed principle. Yeah, that's right, I'm introducing standards into this project now. 'But Arran... how do we do this?'. Fuck if I knew, I've never done this before. So I Googled a few things like the best developers do and found a sweet ass solution. **EventEmitters**, hell yeah! That does exactly what I want and it's sooo easy. They're easy? What? Yeah they fucking are. You know how easy? 4 lines, and that's me being generous.

```node
import {EventEmitter} from 'events';

class EventStream extends EventEmitter {};

const Events = new EventStream();

export default Events;
```

Look how easy that is! You can change some options and all this crap but who cares it's not needed.

Is that _really_ it?

**No.**

Got ya. Although it's only really a two lines more. You didn't think it was that easy did you?

```node
Import EventStream from '../myawesomeclass.mjs';

EventStream.on('mr_event_name', (data) => {
  if(data) {
    //we can turn some stuff on here
  } else {
    //let's turn it off
  }
}
```

There's probably better ways; but this seems like a great solution. So now we have a way to listen to the events. We just need to create them. Again easy, you could probably even get your mother to do it write this for you even though she can't turn on the printer. _I don't do tech support ma'!_. Where am I? Oh yeah, just do this.

```node
Import EventStream from '../myawesomeclass.mjs';

EventStream.emit('mr_event_name', true)
```

Boom, you're done. Thank me later. So now we can interface! So we just have to make the interfaces now... 

Ugh, there's another problem.

## Mo' solutions mo' problems - Notorious B.I.G.

Last time I did interfacing I used [wiring-pi-node](https://github.com/WiringPi/WiringPi-Node) for this but the last commit was over 2 years ago! If only I realised this straight away. I thought fuck it, this was easy to use before so I'll use it again and glimpsed right over that. However now there's problems. It's pretty much abandoned now and doesn't work. Someone should fix that repo, not me though. I only used this before out of my hatred for Python, but everyone uses Python for raspberry pi interfacing. _Why?_. So I caved, there's a simple module that you can use to execute Python scripts, and since I'm not rewriting all this logic in Python I thought fuck yeah, this'll work. [python-shell](https://github.com/extrabacon/python-shell) for anyone who wants to use it. 

Perfect, now I can use RPi.GPIO for all my GPIO needs. This solves everything.

### You've played with my heart enough, where's the next problem?

There isn't. We've achieved our end goal. At the moment this isn't an open sourced project. I need to scrub some data from my commit histories and clean up some code so anyone can use it. Keep an eye on my [GitHub](https://github.com/ArranGravestock) if you're interested in using it. If you can't wait, tough luck sister I don't do dates. 
