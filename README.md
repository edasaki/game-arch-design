# Designing a 2D Side-Scrolling Multiplayer Browser Game

This document is a collection of my progress as I figure out how to design a browser game (along the lines of agar.io or mope.io). The key difference from the major .io games out there is the side-scrolling approach rather than a top-down game. This introduces greater complexity due to the extra physics involved in having a vertical y axis.

My background is largely backend development (with a reasonable amount of game development), and it's going to be a considerable amount of trial-and-error as I figure out what works best.

## The Core Parts

There are two largely distinct parts of creating a multiplayer browser game:

1. Frontend / Browser Rendering
2. Backend / Server-side Game Logic

These can be broken down as follows:

##### Frontend
- Log player inputs and send to server
- Read server response and render accordingly
- Make a pretty UI and make it perform well

That's about it as far as frontend, but "render accordingly" is a deceptively simple term for something that's fairly complex - maybe not so much in terms of difficulty as in terms of sheer monotony from writing separate rendering things for every entity in your game.

##### Backend
- Receive inputs from clients
- Process the main game loop
- Apply player inputs to entities
- Partition game world into views to display to individual clients
- Apply physics as necessary (collisions and forces)
- General Game Design & Structure

This is considerably more involved. The physics portion may be the trickiest, since to avoid excess overhead in the interest of speed, it's probably best to implement your own mini physics engine rather than using the more heavy-duty options available.

Partitioning the game world is also somewhat difficult in terms of creating an efficient implementation, but is crucial for good client-side performance as you want to minimize the duration of each animation frame. A 2D range tree may be the best option.

From a higher-level perspective, it can be tricky to keep your code organized if you aren't too experienced with writing mid-sized or big projects. This depends especially on the language you choose - for example, I'm using Java for my backend and the lack of multiple inheritance is a huge hassle to deal with neatly.

## Shared Stuff

Both the client and server implement these:

### Entities

An "entity" represents some object in the game world. These include anything that can be interacted with: the ground, other players, monsters, gems, etc.

In the backend, entities are represented as POJO classes. In the frontend, entities are represented as Typescript interfaces. The classes and their corresponding interfaces have the exact same fields & types for consistency (unfortunately I can't think of a good way to keep them in sync at the moment short of a fairly ugly .json config file, so it's just a "be really careful" thing for now).

#### Entity Types

##### Fixed Entities

These are entities that have a fixed position in the game world and are not affected by any physics. They *can* be moved, but not because of other entities (for example, clouds that move horizontally). These are largely environmental entities like clouds and the ground.

##### Real Entities

Perhaps I'll think of a better name some day, but these are entities that are "active" game objects. They are (usually) affected by physics, and interact actively with other entities. These include basically everything from players and monsters to attack particles and powerups.

### Authentication

The authentication process is as follows:

__TODO__

## Implementing the Frontend

### Technologies
For my development and implementation of the game frontend, I use the following tech stack:

- HTML/CSS/Javascript = end result
- Typescript&React = main implementation language (compiled to JS)
- SASS = css preprocessor
- Webpack = bundle typescript output
- Gulp = task runner

I am programming the frontend in VSCode.

I chose Typescript as the core language because I started out with a ton of Java and have a strong preference for static typing (can't live without autocompletes haha). Plus it's just a elegant (in my opinion) language overall with a solid mix of OOP and functional paradigms. It also helps obfuscate deployed code a bit more (which is important if you want to maintain intellectual property and have any value), since you basically compile Typescript into a pretty unnatural JS file, and then can uglify the JS for a super ugly JS file to use in production.

I use React for nice and clean non-canvas UI elements.

### Player Inputs

Player inputs are logged with basic event listeners and sent as a message to the websocket server.

### Rendering

The client receives a JSON object from the server and draws it in the canvas by iterating through the array of entities (which are sorted server-side in order of z-index to ensure consistent ordering of what covers what).

Each entity has a string key "type" which is used to determine the way it should be drawn.

### UI/Design & Performance

The game panel is drawn on a full-screen canvas, which turns out to be a gigantic hassle to properly optimize. It's also incredibly important to optimize well for a simple reason - people don't play laggy games. I won't go into too much detail here since apparently this stuff is somewhat tricky to figure out and I gotta keep my competitive edge for my own game ya know :^) But I'll cover the main ideas.

Essentially, you want to 1) minimize the amount of things you draw every frame, and 2) keep things simple - certain types of drawing do not perform well (for example, shadows) and should be avoided whenever possible.

If you have other static UI elements, I highly recommend drawing them separately as actual DOM elements instead of as part of the canvas. Basically you should always keep in mind, the less stuff you draw to canvas, the better. So if you have, for example, a "Level Up" graphic that appears, that's probably never gonna change appearance so rather than redraw it on the canvas every second, you should just make it a DOM element and hide/show it with JS instead.

## Implementing the Backend

For my development and implementation of the game backend, I use the following tech stack:

- Java = core language
- Java WebSockets = websocket server to interact with clients
- Maven = build
- SparkJava = development (quickly launch local webserver with main server process)

I am programming the backend in IntelliJ IDEA.
