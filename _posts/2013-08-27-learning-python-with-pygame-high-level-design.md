---
layout: post
title: Learning python with pygame part 1 High level design
date: 2013-08-27 03:54
author: farezv
comments: true
categories: [Software Engineering, game programming, Software architecture]
---
While searching for internships, I came across a job posting from a very successful game studio here in Vancouver. One of the big requirements for this internship is "Strong Python skills." At first, I was a bit discouraged since I had no experience in python at that point (until I made <a href="https://github.com/farezv/pyrez/commit/9b459bc2e67936532905d07625aa77fce2cc72d1" target="_blank">this commit</a>). But I realized I've been programming for exactly 2 years now, and I have a bunch of Java and some C/C#/C++ experience. I also understand object-oriented programming pretty well and I'm familiar with some high level software design principles (thanks to UBC), so I shouldn't have any trouble picking up python.

<a href="http://farezca.files.wordpress.com/2013/08/pyrez_latest.png"><img class="size-medium wp-image-1375" alt="latest build of my first python game" src="http://farezca.files.wordpress.com/2013/08/pyrez_latest.png?w=300" width="300" height="173" /></a> 
`Latest build of my first python game`

So I started tinkering with pygame and I'm working on a little 2D platformer. So far, I only have a couple of images being rendered on the screen and I can move the player horizontally using arrow keys. As I started thinking about more movement related things to add to my player sprite, I realized I desperately needed to organize my code. Then I realized the bigger mistake I had made. I didn't plan. I don't have a UML diagram, and no idea how various modules of this game will work together, or what these modules even are.

<h2>Defining Modules</h2>

I've done some game programming in Unity, but never coded a game "from scratch" like this before. My software engineering classes have taught me a very "top down" approach to programming, especially if you're starting from scratch (which you rarely do in a full-time job setting). This approach involves coming up with a list of "features" or eliciting them from a customer. Since this is a simple 2D game, my feature list will be simple:
<ul>
	<li>I should be able to move horizontally, and jump on platforms. Gravity will bring me down</li>
	<li>I should be able to kill enemy sprites</li>
	<li>I need to be able to progress through levels, even if I only make a couple of levels to start with</li>
</ul>
In order to implement these features, I need a few modules (aka classes).
<ul>
	<li>A <strong>main</strong> file will start the game up, contain the game loop and call other functions as necessary</li>
	<li>A <strong>constants</strong> module, so I can tweak all static/global constants from a single file</li>
	<li>An <strong>enemy</strong> module allows me to spawn them easily, and add different types of enemies in the future (using inheritance!)</li>
	<li>A <strong>player</strong> class will keep track of player stats. There will only be one instance of this class at any time</li>
	<li>A <strong>level</strong> class also makes sense. Perhaps this class will take a text file as a parameter and turn that into a level of tiles</li>
</ul>
That seems to be a pretty good list so far. The only thing I'm confused about is whether I need an "animation" module. In Unity, there's this notion of an "animation set" which is essentially a collection of animations. For instance, <em>player_jump</em> could have two image frames played for 15 frames each for a 30 fps game. Add in <em>player_shoot</em>, <em>player_die</em> etc. and you have an animation set. You can create a new animation instance based on some conditions and pass any of these animations as a parameter in order to play that sequence of images. So an animation module makes sense. This is what my class UML looks like so far:

<a href="http://farezca.files.wordpress.com/2013/08/part-1-high-level-design-uml.png"><img class="aligncenter size-full wp-image-1377" alt="Part 1 High Level Design UML" src="http://farezca.files.wordpress.com/2013/08/part-1-high-level-design-uml.png" width="640" height="480" /></a>

<h2>Module Relationships</h2>

The classes we came up in the above diagram seem to capture all the things necessary for the "logic" aspect of the game. I'm not going to define separate modules for things like "audio" and "renderer" etc. because pygame provides me with those classes. However, the above diagram tells us nothing about how these modules are related to each other. So let's fix that. If you're not familiar with class diagrams and class relationships, I'll let you <a href="http://en.wikipedia.org/wiki/Class_diagram" target="_blank">read this page</a>.
<ul>
	<li>The main module has a strong "has-a" relationship with the rest of the modules since the main represents the core instance of the game. Think of the game as being <em>composed</em> of constants, enemies, a player, animation sets and a few levels</li>
	<li>As mentioned above, the enemy class could be the "abstract" class for different types of enemies but for now, I'll keep it simple</li>
	<li>Constants and level are likely related in that constants will keep track of how many levels have been successfully completed. So a general dependency makes sense here</li>
</ul>
Here's the refined UML

<a href="http://farezca.files.wordpress.com/2013/08/part-1-high-level-design-uml-relationships.png"><img class="aligncenter size-full wp-image-1380" alt="Part 1 High Level Design UML Relationships" src="http://farezca.files.wordpress.com/2013/08/part-1-high-level-design-uml-relationships.png" width="640" height="480" /></a>

This makes sense because all the modules together form the main instance of the game. None of the other modules can exist without main although the main instance of the game can exist without constants or levels (maybe we're in the menu), or player (maybe our player died and hasn't respawned yet), or enemy (no enemies currently alive).

I'm happy with this so far, although I may tweak it later if I decide to add extra modules or refactor code into its own class. I guess I should start implementing this now.

<em>Note: High level design can be very tricky. There are likely better ways to organize the code base for a game like this. If there are, please suggest them in the comments below. I'm always open to constructive feedback!</em>
