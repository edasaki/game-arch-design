# Designing a 2D Side-Scrolling Multiplayer Browser Game

This document is a collection of my progress as I figure out how to design a browser game (along the lines of agar.io or mope.io).

My background is largely backend development (and very little game engine development), so it's going to be a considerable amount of trial-and-error as I figure out what works best.

## The Core Parts

There are two largely distinct parts of creating a multiplayer browser game:

1. Frontend / Browser Rendering
2. Backend / Server-side Game Logic

These can be broken down as follows:

##### Frontend
- Log player inputs and send to server
- Read server response and render accordingly

That's about it as far as frontend, but "render accordingly" is a deceptively simple term for something that's fairly complex - maybe not so much in terms of difficulty as in terms of sheer monotony, since depending on your implementation you'll have to write up code for rendering all the different entities in your game, which can be a real pain if you're using raw canvas drawings instead of sprites.

##### Backend
- Receive inputs from clients
- Process the main game loop
- Apply player inputs to entities
- Partition game world into views to display to individual clients
- Apply physics as necessary (collisions and forces)

This is considerably more involved. The physics portion may be the trickiest, since to avoid excess overhead in the interest of speed, it's probably best to implement your own mini physics engine rather than using the more heavy-duty options available. 

Partitioning the game world is also somewhat difficult in terms of creating an efficient implementation, but is crucial for good client-side performance as you want to minimize the duration of each animation frame. A 2D range tree may be the best option.

## Shared Structures

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
- Typescript = main implementation language (compiled to JS)
- SASS = css preprocessor
- Webpack = bundle typescript output
- Gulp = task runner

I am programming the frontend in VSCode.

### Player Inputs

Player inputs are logged with Javascript and sent as a message to the websocket server.

### Rendering

The client receives a JSON object from the server and draws it in the canvas by iterating through the array of entities (which are sorted server-side in order of z-index).

Each entity has a string key "type" which is used to determine the way it should be drawn.

## Implementing the Backend

For my development and implementation of the game backend, I use the following tech stack:

- Java = core language
- Java WebSockets = websocket server to interact with clients
- Maven = build

I am programming the backend in IntelliJ IDEA. I haven't yet chosen a web server library.
