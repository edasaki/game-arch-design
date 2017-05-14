# Designing a 2D Side-Scrolling Multiplayer Browser Game

This document is a collection of my progress as I figure out how to design a browser game (along the lines of agar.io or mope.io).

My background is largely backend development (and very little game engine development), so it's going to be a considerable amount of trial-and-error as I figure out what works best.

## The Core Points

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
