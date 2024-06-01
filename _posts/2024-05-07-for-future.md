---
layout: post
title:  "For Future"
author: ana 
categories: [ Unreal, solo-dev ]
image: assets/images/For_Future_Cover_2.png
comments: false
---

<!-- <iframe style="width:100%;" height="315" src="https://www.youtube.com/embed/Cniqsc9QfDo?rel=0&showinfo=0" frameborder="0" allowfullscreen></iframe> -->

**Project Name**: For Future

**Project Status**: In-Progress

**Project Goal**: Develop a mechanic inspired by The Last Of Us for using arrows for close melee combat as well as throwables. 

**Project Structure**: Solo-project. Designed the system, combat mechanic, and art direction. 

<div id="toc_container">
    <p class="toc_title">Contents</p>
    <ul class="toc_list">
        <li><a href="#MainObjectives">Main Project Objectives</a>
            <ul>
                <li><a href="#CombatMechanic">Combat Mechanic Design</a></li>
            </ul>
        </li>
        <li><a href="#Story">Story</a></li>
        <li><a href="#ArtAndDesign">Art & Design</a></li>
            <ul>
                <li><a href="#Inspiration">Inspiration</a></li>
            </ul>
        <li><a href="#3Cs">The 3Cs</a></li>
        <li><a href="#EnemyDesign">Enemy Design</a></li>
        <li><a href="#LevelDesign">Level Design</a></li>
        <li><a href="#Challenges">Challenges</a></li>
        <li><a href="#FutureWork">Future Work</a></li>
    </ul>
</div>

***

<h1 id="MainObjectives">Main Project Objectives</h1>

<h3 id="CombatMechanic">Combat Mechanic Design</h3>

The project's main goal was to develop a melee combat system inspired by *The Last Of Us* and expand on the versatility of weapons a Player Character (PC) has at their disposal. 

The bow-and-arrow weapon combination paired with custom longevity feature - e.g. you run out of arrows, bow breaks after N uses - is seen across many TPS games (*The Last Of Us*, *Legend of Zelda: Breath of The Wild*, etc.)

But what happens when you run out of arrows? The bow typically becomes useless. 

Why not be able to still use it to whack your enemy if you're out of options?

And why not use arrows as a shiv if your bow breaks?

With these questions in mind I set out to work on this project. My goal was to expand on the usual bow-and-arrow mechanic, incorporating the weapon-expiry concept into the individual components that make up the weapon, i.e. the bow and the arrows. The Player Character would be able to use either one separately as well as together. 

**Key Design Concepts**: 
1. The player should have the ability to pick up stashes of arrows and bow separately as found within the level. 
2. When the player picks up arrows but does not yet possess the bow item, or if the bow has "broken", the player should use the arrows as they would a shiv or dagger. 
3. Similarly, if the player possesses a bow but has used up all their arrows or hasn't found any in the first place, they would use the available bow as a bat, whipping it at their enemy. 

***

<h1 id="Story">Story</h1>

With my focus on Combat Mechanic Design and Level Design, I wanted to keep the story bite-sized but straightforward. The player needed a sense of urgency and a reason to want to reach the end goal by confronting Enemy Characters (EC). From personal experience, players seem to be effectively affected when the story goal is relatable, so I chose a trope I knew to be effective - the parent-child dynamic. 

The goal of the game was to be simple: **secure the life-giving medicine for your child at all costs**. 

Due to time constraints, I knew the game would need to be fairly small, a single scene/level at most. How do you make a simple one-level game feel more urgent? You add a timer. 

<figure class="figure-shadow">
    <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_Story.png" alt="Story Notes">
</figure>
<p style="text-align: center;"><i>Original story draft.</i></p>

***

<h1 id="ArtAndDesign">Art & Design</h1>

<h2 id="Inspiration">Inspiration</h2>

To give the game a *darker* feel, I took inspiration from the Noir Comic style, pulling images from various sources to get a better understanding of the different colour scheme options and greyscale.

<figure>
    <img src="{{ site.baseurl }}/assets/images/archer-art-direction.png" alt="Initial Mood Board">
</figure>
<p style="text-align: center;"><i>The Original Noir Comic inspiration board.</i></p>

<h4 id="UseOfColour">Use of Colour</h4>

The idea for *pops of colour* seen in some images (also reminiscent of games like *Mirror's Edge*) would help alleviate the monotony and attract the player to objects of interest, such as enemies and special items. 

<h4 id="SpeechBubbles">UX Add-ons</h4>

A key addition that I wanted to add to the game, faithful to the source of inspiration, was to add comic-esque speech bubbles during PC's inner monologue, on top of the voiceover. 

<figure>
    <img src="{{ site.baseurl }}/assets/images/speech_bubble_GIF.gif" alt="Comic-style speech bubble monologues">
</figure>
<p style="text-align: center;"><i>Comic-style speech bubble monologues.</i></p>


***

<h3 id="3Cs">The 3Cs</h3>

Having limited time for modelling characters I was limited to using freely available assets online, but the overall design for the character pivoted on making visible the story's *VIP*, i.e. the baby called Future the Player Character is carrying. This was inspired in part by the Deliveryman Sam from *Death Stranding*.

<figure>
    <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_PlayerCharacter.png" alt="Player Character Design Notes">
</figure>
<p style="text-align: center;"><i>Original Player Character design</i></p>

The camera was to be in the third person, as in *Death Stranding* and *The Last Of Us* to allow the player a good view of the character, the *child* they are carrying (and therefore sympathise with the emotional stakes better) and, of course, the *combat mechanics*.

The controls were to be intuitive and centred around the basic TPS standard with room for customisations based on test player feedback.

<figure>
    <img src="{{ site.baseurl }}/assets/images/keyboard_controls.jpg" alt="Keyboard Control Diagram">
</figure>
<p style="text-align: center;"><i>Keyboard controls.</i></p>

***

<h3 id="EnemyDesign">Enemy Design</h3>

Enemy Characters (EC) were a key part of the story narrative and had to provide a significant threat to the Player Character on their way into the house. The ECs were to be varied, and pose different levels of threat. As the PC moved through the game, the EC would become more difficult, and encourage the PC to seek out weapons or items to help them through the level (as prompted by the inner monologue of the PC).

The 3 Enemy tiers I came up with were as follows:
1. "Point and Shoot" - a basic *introductory* enemy encountered at the beginning of the level to get the Player accustomed to the combat controls. Straightforward and quick to kill on the off chance that the PC only had a limited number of weapons (or none at all).
2. "Tank-O" - this enemy would be difficult to eliminate, with plenty of health, but weak on damage. These would be positioned at "gateways" to other parts of the level, e.g. the staircase leading to the next floor, or doorways to important rooms. 
3. "Multiplier" - the most dangerous EC due to the tendency to multiply. The gas canisters positioned by their shoulders - used as a defense mechanism - when pierced cause hallucinations and cause clones to appear. They are easy to kill but are best dealt with at long range and require good aim (encouraging skill from the Player).

<figure>
    <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_EnemyDesign.png" alt="Enemy Design">
</figure>
<p style="text-align: center;"><i>Original Enemy Character design</i></p>

The models chosen for the ECs has a pop of orange colour to signify danger, and would be identifiable within the general greyscale colour palette.

<figure>
    <img src="{{ site.baseurl }}/assets/images/multiplier_GIF.gif" alt="Multiplier Functionality Test">
</figure>
<p style="text-align: center;"><i>Multiplier "cloning" functionality test.</i></p>

***

<h3 id="LevelDesign">Level Design</h3>

With the above in mind, I envisioned a setting that varied in available space, to allow for both long-range and close-quarters combat in which the player would have the opportunity to use the varied melee attacks. A house, through which they can discreetly make their way, get rid of enemies, to ultimately reach the top floor where the goal of the game lies. 

<figure>
    <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_LevelDesign_StoryPoints.png" width="500" alt="Level Pre-requisites">
</figure>
<p style="text-align: center;"><i>First Draft Level Pre-requisites</i></p>

<figure>
    <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_LevelDesign_Map.png" alt="First Draft Level Design">
</figure>
<p style="text-align: center;"><i>First Draft Level Design</i></p>

The Player would start outside the house, as dictated by the <a href="#Story"><u>Story</u></a>, and would be able to notice the enemies from afar. Through <a href="#SpeechBubbles"><u>inner monologue</u></a> they would be urged to infiltrate the house. They would be urged to avoid the front door due to the EC presence and make their way around the house where they will have opportunity to select one of two routes: an open window on the ground floor, or the back door. The door would be easier to spot but will require evading an enemy before entering. 

The first floor contains multiple opportunities for acquiring weapons. 

*The <a href="#SpeechBubbles"><u>inner monologue</u></a> of the PC will signal to the Player where to find these. E.g. upon climbing through the open window, the PC will think "My bow should just be through the door here..." or when the back door is in the line of sight, "Let's hope they didn't find the arrows I left by the back door last night." etc.*

Entering the house through the backdoor, although is the longer route, puts the Player at a slight advantage as opposed to climbing into the house through the open window. They will pick up the arrows by the door, and be directed to the kitchen by the inner monologue and the EC presence further down the corridor. The Players that choose to climb through the open window would also find the bow in the adjacent room, but are more likely to face the next EC-filled room without the accompanying set of arrows. If they survive, they will be rewarded by a stash of arrows in the corner of the room. 

At this point, regardless of the route chosen, the Player should possess both bow and arrow and upon reaching the staircase, easy clear it of ECs and reach the next floor.

The Player again will face a choice of route and race against the clock to the reach the final room and the end of the game. 

***

<h1 id="Challenges">Challenges</h1>

<h4>1. Migrating to newer versions of Unreal Engine</h4>

Although the migration process is fairly straightforward, there are hidden challenges that crop up when you least expect it. The lesson I learnt is to become much more prudent & getting acquianted with Unreal's Release Notes as soon as possible. 

<h4>2. Retargeting animations</h4>

While working on the project Unreal had released a much improved feature for Retargeting Animations, however due to being very new, there were very few online resources to help with debugging. Ideally I would like to learn more about Rigging Skeletons to better understand the problems around animation. 


<h1 id="FutureWork">Future Work</h1>

The development of this game is still in-progress, though some of the key features and functionality has already been implemented. Below are the most urgent next steps as well as ideas for future expansions beyond the current scope of the work.

<h4>1. Dedicate more time working on the **3Cs**.</h4>

Specifically working on the Camera and the transition from an outside to an inside environment. There is a lot of potential to experiment with different camera options. This would require expanding on the zoom functionality that is already implemented and figuring out the collision of the camera with walls, doors and stairs.

<h4>2. Perfect combat mechanics for a more balanced experience.</h4>

Although some preliminary choices have been made as to the Enemy design, these are yet to be tested with the player experience in mind in order to make sure the combat is balanced and enjoyable. 

<h4>3. Start work on Sound Design.</h4>

* Research Sound Design for TPS games, looking into well established games. 
* Work on recording voice-over for the Player Character's inner monologue.
* Experiment with different sound effects. 