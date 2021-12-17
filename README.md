# simulation



It took me a while to figure out my reality. 

I'm honestly don't like being "too aware" of my surroundings.  

It's starting to become a conterproductive powerup.  

Whatever it is, I landed here. I think this is it.

The last directory  .




## Introduction

Hello everyone, 

I thought this would be an interesting way to introduce myself to users, members, and speculators of the community as a form of contribution in form to work alongside the projects using DeSo.


My name is Chris. 

I'm searching to take the mask off "god" and seek out the true reason for human life and who the existence is behind this incredible journey.

I'm by no means an expert in any way about anything.  I wear a noob badge as a pledge of honor.  Instead, one can only claim to comprehend very little across a lot. 

Education wasn't quite for me.  By education I'm referring to the education sysem in America.  Learning and experimenting with different technologies is what drives me in life.  Money has no value to me.  Whether it be fiat or crypto.  I only work to keep a healthy lifestyle and my everyday learning.  My belief here is to provide some sort of value.  As a citizen of this planet, I expect to do my part and contribute in any way I can for humanity.

The words "Microsoft" and "blockchain" are two words that share a mutual frequency I once discovered many years ago.  One at a very young age, and the other quite recent if you think in terms of years.  While the tactic to note mutal relationship with big tech might gain traction in one form of society, it does no good in this one.  This one, where are we? How are you processing this data?  How and why is this possible?  It is so because of ideas.  Some perhaps might contradict with another.  Entities who collide react a certain way from their relative positions.  The greater the force, the greater distance of the oppsing reaction.  Linux, Mozilla, Microsoft, Apple, Google, and many of the most common by not so often recognized resources that help make the building efforts a bit easier were technological growth proven by Metcalfe's law such as bluetooth, wifi, lan, dns, and many others.

"Cheat codes" are what I call them.  "Hacks" or "Hax", depending on where you're coming from.  As a child, my obsession was in technology.  Punishment was the effect after dismantling the family VHS tape player one day.  Understanding the inner workings to redesign and model for my fit was a common thing.  The ideas generated and discovering ways to connect complete opposites often delivered mental peace.  Ideas begin to floor and my pre-frontal begins to tickle.  Chills run down my spine and happiness begins to refil an empty heart.  This is my safe place.  My home.  It's where I feel where I need to be.

Perhaps there is no reality.  Maybe it's all just fake.  Hopefully not testnet fake.  But since we're sharing experiences and ideas I'll go ahead and say it...



Yea right! 🙄




lol...you're such a noob


haha...






wait...

fr?

?!





is the whole thing really just a siumlation?

To be continued...

`simulation` is a complete [Cube 2: Sauerbraten](http://sauerbraten.org/) experience in the web delivered as a single Docker image.


## Project goals

## Getting started

All you need is Docker and [Earthly](https://earthly.dev/) to build. Just run `earthly +image` and it will make the `sour:latest` image.

To use the `simulation` image, expose ports `1234` and `28785` when you run a container like this:

```bash
docker run --rm -it -p 1234:1234 -p 28785:28785 sour:latest
```

It's worth nothing that you can change the first mapping (`1234:1234`) to whatever you want (e.g `80:1234`) but the second one has to be `28785`, since the frontend expects the proxy service to be at that port.

## Architecture

Here is a high level description of the repository's directory structure:
* `services/game/cube2`: A fork of [BananaBread](https://github.com/kripken/BananaBread), which was kripken's initial attempt at getting a version of Sauerbraten running using Emscripten. He forked Sauerbraten at the mainline [r4059](https://sourceforge.net/p/sauerbraten/code/4059), I upgraded to [r4349](https://sourceforge.net/p/sauerbraten/code/4349), then finally upgraded to the latest mainline at the time [r6519](https://sourceforge.net/p/sauerbraten/code/6519). Contains a handful of modifications and restrictions to make sure it can run well in the web.
* `services/game/assets`: A checkout of Sauerbraten's `packages/` directory, which contains all of the game's default assets. Also includes Sour's asset bundling mechanism to generate prepackaged Emscripten file bundles for each game map.
* `services/server/`: A fork of [QServCollect](https://github.com/deathstar/QServCollect), which is a dedicated Sauerbraten server.
* `services/proxy/`: A fork of [wsproxy](https://github.com/FWGS/wsproxy) which I changed to only allow proxying from TCP `28785` to UDP `28786`. This was the quickest way I found to get client/server communication working, though presumably you could just do this in a Python script.
* `services/client/`: A React web application that glues together the compiled Sauerbraten code and our asset fetching mechanism.

The resulting Docker image will run services on these ports when started:
* `tcp:1234`: An nginx server that serves up the `client`, compiled Sauerbraten binaries, and assets.
* `tcp:28785`: The hacky `wsproxy` version we use that proxies WS connections to the container's UDP port `28786`.
* `udp:28786`: A real QServCollect server. Typically you don't expose this, but if you want to do crossplay (connect with real Sauerbraten) you absolutely can.

## Contributing

All contributions are welcome.

To hack on simulation:
1. Run `earthly +game` (compiles the game), `earthly +assets` (builds its assets for the web,) and `earthly +image-slim` (builds the Sour image without assets.)
2. If you want to work on multiplayer, run this Docker container: `docker run --rm -it -p 28785:28785 sour:slim`. It makes the QServCollect server available (over a WebSocket) on `28785`.
3. Start the web client.
    1. If you have [warm](https://github.com/cfoust/warm/blob/master/warm), run `warm shell`, `cd services/client` and run `yarn serve`.
    2. If you do not have `warm`, run `docker run --rm -it --volume=$(pwd):$(pwd) -w $(pwd) --init --network=host node:14.17.5 /bin/bash`, `cd services/client` and run `yarn serve`.
4. Navigate to `http://localhost:1234`. To recompile the game, just do `earthly +game` and refresh the page. The same applies to `earthly +assets`. If you change the client code, also just refresh.

Check out the roadmap below to see what you might be able to help with.
* [.] Integrate DeSo Protocol
* [ ] Landing page
* [ ] DeSo user profile
* [ ] Discord or Slack Server
* [ ] Front game cover
* [X] Domain Name
* [ ] DeSo node
* [X] Node server
* [ ] Graphic design
* [ ] NFT Game Assets
* [ ] NFT Marketplace & Minting Landing Page
* [ ] NFT Game Characters
* [ ] Story line development
* [ ] Game board designer
* [X] Fix performance issues
* [X] Ensure the base assets are loaded prior to starting the game
* [X] Evict the previous map from memory when we change maps
* [ ] Better development experience with simple docker-compose setup
* [ ] Better documentation on services, how to build assets, et cetera
* [ ] Support all player models (right now it's just snout)
* [ ] Add CTF assets to the base game
* [ ] Explore running Sour in a Web Worker rather than the rendering thread
* [ ] Investigate differences in font colors between the real Sauer and Sour
* [ ] Make sure `getmap` and `sendmap` don't break the game server
* [ ] Allow for players to create custom matches
* [ ] Allow Sour to read from the real master server and connect to real Sauerbraten servers
* [ ] Support saving and loading `.ogz` maps from the user's device
* [ ] Save demos for any game played to IndexedDB and allow for download
* [ ] Demo player with seek/play/pause
* [ ] Stretch goal: generate gifs in the browser





simulation
======================

A port of the Cube 2/Sauerbraten 3D game engine/first person shooter to the
web, compiling C++ and OpenGL to JavaScript and WebGL using Emscripten.

See the **[FAQ](https://github.com/kripken/BananaBread/wiki/FAQ)**.


Demos
-----

**[A live demo is available](https://kripken.github.io/BananaBread/cube2/bb.html)**. Note that it requires [WebAssembly](http://webassembly.org/).


Overview
--------

Cube 2 is a compact and efficient 3D game engine. By compiling it
to JavaScript and WebGL you can run a complete first person
shooter in your web browser, using only standard web APIs and
without any plugins.

Features:

 * A multitude of visual effects including water reflection/refraction,
   parallax mapping, glare, particle effects,
   lightmaps, skeletal animation (on gpu), etc. etc.
 * Streamlined and quick physics system
  * Ragdoll physics
 * Bot AI with adjustable skill level
 * Integrated in-game editor
 * Fast performance both running on the web or natively
 * zlib license

The original engine also has a lot of other features not yet
enabled (but will be):

 * Multiplayer
  * Multiplayer editing
 * Shadowmapping


Building
--------

Get emscripten and its dependencies,

  http://emscripten.org

It's recommended to go through the emscripten tutorial to see that it is set
up properly.

Then do `emmake make client -j8` in `cube2/src/web` for a web build.
(You can also do `make` in `cube2/src/native` for a native linux build.)

You can then run `bb.html` (in `cube2/`) to see the output.

If you have any problems building, feel free to file an issue here or to
find us on emscripten IRC (see [emscripten site](http://emscripten.org)).

 * See needed.txt for some possibly useful scripts to package the output.


Running
-------

0. Run
     python -m SimpleHTTPServer 8888
   in cube2/

1. Load localhost:8888 in your browser


Modding
-------

To use maps of your own or make other kinds of changes or additions to the
artwork, see the
**[Modding](https://github.com/kripken/BananaBread/wiki/Modding)**
page.


Debugging
---------

If you append `,debug` to the URL of one of the levels, it will use
`bb.debug.js` instead of `bb.js`. The debug build has not been run
through closure compiler and is much more readable for stack traces
and profiling and so forth.


License
-------

### Code


### Art

