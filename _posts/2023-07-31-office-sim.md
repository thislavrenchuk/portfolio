---
layout: post
title:  "The Oddity Office (Game Jam)"
author: ana 
categories: [ Unreal, 2D, Game Jam ]
image: assets/images/office-sim.png
comments: false
---

# Introduction:

**Name**: The Oddity Office

**Goal**: learn about 2D game development, and create a basic quest system in C++.

**Team structure**: Developer in a 3 people team working to a 5-day deadline. 

**Inspiration**: Stanley Parable, Monsters, Inc.

<div id="toc_container">
    <p class="toc_title">Contents</p>
    <ul class="toc_list">
        <li><a href="#Result">The Game</a></li>
        <li><a href="#MainObjectives">Project Objectives</a></li>
        <li><a href="#Implementation">Implementation</a>
            <ul>
                <li><a href="#Story">Story & Quest System</a></li>
                <li><a href="#3Cs">The 3Cs</a></li>
                <li><a href="#LevelDesign">Level Design</a></li>
            </ul>
        </li>
        <li><a href="#Challenges">Challenges</a></li>
        <li><a href="#FutureWork">Future Work</a></li>
    </ul>
</div>

***

<h1 id="Result">The Game</h1>

<h3>Playthrough</h3>

{% include youtube.html id="hj4TxRTAuak" %}

<h3>Itch.io</h3>

<iframe src="https://itch.io/embed/2135977" width="552" height="167" frameborder="0"><a href="https://anasaurus.itch.io/the-oddity-office">The Oddity Office by Ana Lavrenchuk</a></iframe>

***

<h1 id="MainObjectives">Project Objectives</h1>

The game was a result of a *5-day* Game Jam challenge to create a pixel game on the theme "you are the monster". I joined the Game Jam with the following objectives in mind: 
1) Understand the 2D game development process in Unreal
2) Learn to implement a simple quest system, and
3) Package and submit the game within the time-limit. 

I signed on as an Unreal (C++) developer and was joined by two others: a 2D Artist, and a Sound Engineer. 

***
<h1 id="Implementation">Implementation</h1>

<h2 id="Story">Story & Quest System</h2>

As a team, we settled on an idea of creating an office sim game. The office would be full of "monster" workers, reminiscent of *Monsters, Inc.* animated movie, and the human Player Character (PC) would be viewed by the NPCs as the "monster", though pleasantly surprised to find the PC friendly and approachable. 

Taking further inspiration from the office sim setting and the *Stanley Parable* game, I wanted the story to start out unassuming, even funny, and turn darker and creepier as the story progressed. 

I decided the easiest way to do this would be to contain each level as a single "day in the office" with a set of tasks to perform each day that would uncover smaller snippets of story as the Player progressed. The overall game would span 5 days in the office, contained in 5 individual levels. Each day would focus on one or more colleagues and their stories and get more unhinged as the levels progressed: 

<figure class="figure-shadow">
    <img src="{{ site.baseurl }}/assets/images/office-sim-story.png" alt="Story Draft">
</figure>
<p style="text-align: center;"><i>Original story draft.</i></p>

The quest system would incorporate different variotions of dialogue with the NPCs that depend on the actions the Player has preformed so far.

<h2 id="3Cs">Camera and Controls</h2>

<h4>Camera</h4>

While at first we contemplated implementing an isometric camera viewpoint, after conferring with the 2D artist, we decided - based on the artist's level of experience and our time restriction - that a side-on game perspective would allow them to do more in a short amount of time. With this in mind we settled on a side-on platformer perspective. The camera would follow the character at a distance that would allow good visibility of the rest of the level to encourage exploration and engagement with the world.

<figure class="figure-shadow">
    <img src="{{ site.baseurl }}/assets/images/office-sim-camera.png" alt="Camera">
</figure>

<h4>Controls</h4>

For simplicity, the game followed a classic platformer setup that stressed exploration through movement and interaction with the world and the NPCs. 

<figure class="figure-shadow">
    <img src="{{ site.baseurl }}/assets/images/office-sim-keyboard_controls.png" alt="Keyboard Controls">
</figure>
<p style="text-align: center;"><i>Keyboard Controls.</i></p>

<h4>Character</h4>

With one of the focuses of the game being exploration, I wanted the Player to be able to interact with random objects in the world. This came together in a narration-style pop-up in the top-left of the screen (to differentiate from the dialogue pop-ups when interacting with NPCs).

My intention for these interactions was to give a strong sense of identity for the Player Character (PC), so reading a signpost would reflect not the signpost's contents, but rather the inner dialogue of the PC. This gave the PC depth, and individuality which would help with the telling of the Story narrative.

<figure class="figure-shadow">
    <img src="{{ site.baseurl }}/assets/images/object_interact-office-sim.gif" alt="Interaction with world & objects">
</figure>
<p style="text-align: center;"><i>Interaction with world & objects.</i></p>

<figure class="figure-shadow">
    <img src="{{ site.baseurl }}/assets/images/npc_interact-office-sim.gif" alt="Interaction with NPCs">
</figure>
<p style="text-align: center;"><i>Interaction with NPCs.</i></p>


<h2 id="LevelDesign">Level Design</h2>

I wanted the levels to reflect the general theme and story of the game discussed above. They would start linear, more in line with what an office should feel like (e.g. level floors, stairs, lifts, doors), but get stranger and more unusual with every level. I wanted to incorporate levels with ability to jump on clouds, to sink into liquid to reach another floor, and perhaps even the more cliche jumping onto moving platforms. 

As discussed in the <a href="#Challenges">Challenges</a>, however, the final level implemented was a simplified version of the original level design and only involved the first level - a typical level platform, with only a touch of flare at the end in the form of a storage room, where the PC has to climb a mountain of office furniture.

<figure>
    <img src="{{ site.baseurl }}/assets/images/photocopy-room.gif" alt="Photocopy Room Playthrough">
</figure>
<p style="text-align: center;"><i>Photocopy Room Playthrough.</i></p>

***

<h1 id="Challenges">Challenges</h1>

The Game Jam was a great learning experience. The main Lessons Learnt were as follows:

<h4> 1) Time constraints and the importance of a good plan </h4>

The allocated time for the Game Jam flew by quicker than expected. I had prepared a rough plan of work before starting the Game Jam but did not allocate nearly enough time to testing and debugging. This being my first foray into 2D development I faced new issues and bugs on a daily basis and although I am confident in debugging technical issues, the lack of allocated time meant that the implementation progressed slower than expected. This bring me to the next lesson:

<h4> 2) Prioritisation </h4>

Due to time constraints and the main goal of delivering a completed project by the deadline, I decided to cut the implementation to just one level. Although it meant I was not able to build out the story of the game further, I was still able to deliver a game with a single level with all the features that I set out to complete within the time frame. 

<h4> 3) Team work and scheduling </h4>

Partnering up with a team of strangers online came with its risks. One of them being that due to personal changes of circumstance, team members had to drop out before the deadline. Although they were able to put in the initial work with the Art and Sound direction, I had to deliver the game to the finish line on my own. Managing the different time zones and the team's availability required being flexible and adapative, which I managed to do well, delivering the final product in time for the deadline. 

***

<h1 id="FutureWork">Future Work</h1>

1) I would like to expand the Story further as per my initial Story plans and deliver a compelling narrative. 

2) This would also mean expanding and improving on the level design with a focus on level complexity that would reflect the progression of the story. 

3) Finally, I would like to work on creating custom 2D sprites, to tie the Art with the Story and make the feel of the game more cohesive. 