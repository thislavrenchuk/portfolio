---
layout: post
title:  "For Future"
author: ana 
categories: [ Unreal, solo-dev ]
image: assets/images/For_Future_Cover_2.png
comments: false
---

**Name**: For Future

**Status**: In-Progress

**Goal**: Develop a mechanic inspired by The Last Of Us for using arrows for close melee combat as well as throwables. 

**Team Structure**: Solo-project. Designed the system, combat mechanic, and art direction. 

<div id="toc_container">
    <p class="toc_title">Contents</p>
    <ul class="toc_list">
        <li><a href="#MainObjectives">Main Project Objectives</a>
            <ul>
                <li><a href="#CombatMechanic">Combat Mechanic Design</a></li>
            </ul>
        </li>
        <li><a href="#Technical">Technical Implementation</a>
            <ul>
                <li><a href="#ArrowsMelee">Arrows as melee weapons</a></li>
                <li><a href="#ZoomIn">Zoom functionality for long-range attacks</a></li>
            </ul>
        </li>
        <li><a href="#Story">Story</a></li>
        <li><a href="#ArtAndDesign">Art & Design</a></li>
            <ul>
                <li><a href="#Inspiration">Inspiration</a></li>
            </ul>
        <li><a href="#3Cs">The 3Cs</a></li>
        <li><a href="#EnemyDesign">Enemy Design</a>
            <ul>
                <li><a href="#EnemyImplementation">Enemy Implementation</a></li>
            </ul>
        </li>
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

<h1 id="Technical">Technical Implementation</h1>

<h3 id="CombatMechanicImplementation">Combat Mechanic Implementation</h3>

<h4 id="ArrowsMelee">1. Arrows as melee weapons</h4>

I was able to implement a stab functionality using arrows through the use of Trigger Boxes and `AnimNotify`. A `TriggerBox` has been added around every enemy that is small enough that the Player Character has to be very close to the enemy to be able to use the Stab mechanic. 

If the Player Character is within the trigger box and is therefore "within range", upon triggering the Stab mechanic, the appropriate `AnimMontage` runs, and with the help of a custom `AnimNotify`, the damage is dealt at the moment the arrow stab is performed. Note: damage is dealt only if the line trace of the arrow's `SkeletalMeshComponent` hits another Actor. 

<div style="display: grid; place-items: center;" class="zoom">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Stab-functionality.gif" width="500" alt="Using arrows in melee combat">
    </figure>
    <p style="text-align: center;"><i>Using arrows in melee combat.</i></p>
</div>

<a href="https://github.com/thislavrenchuk/for_future_project/blob/main/Source/Hunter/StabNotify.cpp">*StabNotify.cpp*</a>

<div>
    <pre style="height: 500px; overflow: scroll;">
        <code>
void UStabNotify::Notify(USkeletalMeshComponent* MeshComp, UAnimSequenceBase* Animation)
{
    // while animation is playing check if damage has been dealt
    FVector ArrowTipLocation = MeshComp->GetSocketLocation("ArrowTip");
    FVector ArrowBaseLocation = MeshComp->GetSocketLocation("ArrowEnd");
    FVector DamageAngle = (ArrowBaseLocation - ArrowTipLocation);

    // Ignore certain actors
    FCollisionQueryParams Params;
    Params.AddIgnoredActor(MeshComp->GetOwner());

    // Create a line trace
    FHitResult HitResult;
    bool bSuccess = GetWorld()->LineTraceSingleByChannel(HitResult, ArrowTipLocation, ArrowBaseLocation, ECollisionChannel::ECC_GameTraceChannel1, Params);
    
    if (bSuccess) 
    {
        AActor* HitActor = HitResult.GetActor();
        if (HitActor != nullptr) 
        {
            FPointDamageEvent DamageEvent(StabDamage, HitResult, DamageAngle, nullptr);
            HitActor->TakeDamage(StabDamage, DamageEvent, MeshComp->GetOwner()->GetInstigatorController(), MeshComp->GetOwner());
        }
    }
}
        </code>
    </pre>
</div>

<h4 id="ZoomIn">2. Zoom functionality for long-range attacks</h4>

To add a smooth transition from an "idle" camera position to a zoomed-in camera position when a Player is triggering the Zoom (e.g. to aim an arrow), I implemented a `TimelineCurve` as seen in tutorials online (e.g. <a href="https://nerivec.github.io/old-ue4-wiki/pages/timeline-in-c.html">Timeline in C++</a>). The result was a smooth transition to and fro the zoomed-in & zoomed-out camera positions that also allowed for interruptions. If the Player triggered the zoom-in functionality during the zoom-out transition (i.e. from zoomed-in to zoomed-out camera position) or vice versa, the transition remains smooth and does not distract from the gameplay. 

Having a zoom-in functionality was important to allow for easier aiming at targets at long-range, but also to bring the Player "closer to the combat" and make them connect to the action more effectively.

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Zoom-In-functionality.gif" width="500" class="center" alt="Zoom In functionality in long-range combat">
    </figure>
    <p style="text-align: center;"><i>Zoom In functionality in long-range combat.</i></p>
</div>

<a href="https://github.com/thislavrenchuk/for_future_project/blob/main/Source/Hunter/Characters/BaseCharacter.cpp">*BaseCharacter.cpp*</a>
<div>
    <pre style="height: 500px; overflow: scroll;">
        <code class="language-scala">
// Set Timeline Curve
ABaseCharacter::ABaseCharacter()
{
    auto XCurve = ConstructorHelpers::FObjectFinder&lt;UCurveFloat&gt;(TEXT(&quot;/.../C_AimZoom_X.C_AimZoom_X&apos;&quot;));
    if (XCurve.Object)
    {
        XFloatCurve = XCurve.Object;
    }
    auto YCurve = ConstructorHelpers::FObjectFinder&lt;UCurveFloat&gt;(TEXT(&quot;/.../C_AimZoom_Y.C_AimZoom_Y&apos;&quot;));
    if (YCurve.Object)
    {
        YFloatCurve = YCurve.Object;
    }
    auto ZCurve = ConstructorHelpers::FObjectFinder&lt;UCurveFloat&gt;(TEXT(&quot;/.../C_AimZoom_Z.C_AimZoom_Z&apos;&quot;));
    if (ZCurve.Object)
    {
        ZFloatCurve = ZCurve.Object;
    }
    // ...
}

// Set up Timeline Component
void ABaseCharacter::BeginPlay()
{
    Super::BeginPlay();
    FOnTimelineFloat onTimelineCallback;
    FOnTimelineEventStatic onTimelineFinishedCallback;

    if (XFloatCurve != NULL &amp;&amp; YFloatCurve != NULL)
    {
        auto Timeline = NewObject&lt;UTimelineComponent&gt;(this, FName(&quot;TimelineAnimation&quot;), EObjectFlags::RF_NoFlags, nullptr, false, nullptr);
        MyTimeline = Timeline;

        MyTimeline-&gt;CreationMethod = EComponentCreationMethod::UserConstructionScript; 
        this-&gt;BlueprintCreatedComponents.Add(MyTimeline); // Add to array so it gets saved
        MyTimeline-&gt;SetNetAddressable(); 
        MyTimeline-&gt;SetPropertySetObject(this);
        MyTimeline-&gt;SetDirectionPropertyName(FName(&quot;TimelineDirection&quot;));
        MyTimeline-&gt;SetLooping(false);
        MyTimeline-&gt;SetTimelineLength(1.0f);
        MyTimeline-&gt;SetTimelineLengthMode(ETimelineLengthMode::TL_TimelineLength); 
        MyTimeline-&gt;SetPlaybackPosition(0.0f, false);

        // Add the float curve to the timeline and connect it to the timelines&apos;s interpolation function
        onTimelineCallback.BindUFunction(this, FName{TEXT(&quot;TimelineCallback&quot;)}); // See function below
        onTimelineFinishedCallback.BindUFunction(this, FName{TEXT(&quot;TimelineFinishedCallback&quot;)});
        
        MyTimeline-&gt;AddInterpFloat(XFloatCurve, onTimelineCallback, FName{TEXT(&quot;XZoom-PropertyName&quot;)}, FName{TEXT(&quot;XZoom-TrackName&quot;)});
        MyTimeline-&gt;SetTimelineFinishedFunc(onTimelineFinishedCallback);
        MyTimeline-&gt;RegisterComponent();
    }
...
}      

// This function is called for every tick in the timeline.
void ABaseCharacter::TimelineCallback(float interpolatedVal)
{
    float position = MyTimeline-&gt;GetPlaybackPosition();
    if (SpringArmComponent != nullptr)
    {
        SpringArmComponent-&gt;SocketOffset.X = XFloatCurve-&gt;GetFloatValue(position);
        SpringArmComponent-&gt;SocketOffset.Y = YFloatCurve-&gt;GetFloatValue(position);
        SpringArmComponent-&gt;SocketOffset.Z = ZFloatCurve-&gt;GetFloatValue(position);
    }
}
        </code>
    </pre>
</div>

***

<h1 id="Story">Story</h1>

With my focus on Combat Mechanic Design and Level Design, I wanted to keep the story bite-sized but straightforward. The player needed a sense of urgency and a reason to want to reach the end goal by confronting Enemy Characters (EC). From personal experience, players seem to be effectively affected when the story goal is relatable, so I chose a trope I knew to be effective - the parent-child dynamic. 

The goal of the game was to be simple: **secure the life-giving medicine for your child at all costs**. 

Due to time constraints, I knew the game would need to be fairly small, a single scene/level at most. How do you make a simple one-level game feel more urgent? You add a timer. 

<div style="display: grid; place-items: center;">
    <figure class="figure-shadow">
        <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_Story.png" width="500" class="center" alt="Story Notes">
    </figure>
    <p style="text-align: center;"><i>Original story draft.</i></p>
</div>

***

<h1 id="ArtAndDesign">Art & Design</h1>

<h2 id="Inspiration">Inspiration</h2>

To give the game a *darker* feel, I took inspiration from the Noir Comic style, pulling images from various sources to get a better understanding of the different colour scheme options and greyscale.

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/archer-art-direction.png" width="500" class="center" alt="Initial Mood Board">
    </figure>
    <p style="text-align: center;"><i>The Original Noir Comic inspiration board.</i></p>
</div>

<h4 id="UseOfColour">Use of Colour</h4>

The idea for *pops of colour* seen in some images (also reminiscent of games like *Mirror's Edge*) would help alleviate the monotony and attract the player to objects of interest, such as enemies and special items. 

<h4 id="SpeechBubbles">UX Add-ons</h4>

A key addition that I wanted to add to the game, faithful to the source of inspiration, was to add comic-esque speech bubbles during PC's inner monologue, on top of the voiceover. 

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/speech_bubble_GIF.gif" width="500" class="center" alt="Comic-style speech bubble monologues">
    </figure>
    <p style="text-align: center;"><i>Comic-style speech bubble monologues.</i></p>
</div>


***

<h3 id="3Cs">The 3Cs</h3>

Having limited time for modelling characters I was limited to using freely available assets online, but the overall design for the character pivoted on making visible the story's *VIP*, i.e. the baby called Future the Player Character is carrying. This was inspired in part by the Deliveryman Sam from *Death Stranding*.

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_PlayerCharacter.png" width="500" class="center" alt="Player Character Design Notes">
    </figure>
    <p style="text-align: center;"><i>Original Player Character design</i></p>
</div>

The camera was to be in the third person, as in *Death Stranding* and *The Last Of Us* to allow the player a good view of the character, the *child* they are carrying (and therefore sympathise with the emotional stakes better) and, of course, the *combat mechanics*.

The controls were to be intuitive and centred around the basic TPS standard with room for customisations based on test player feedback.

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/keyboard_controls.jpg" width="500" class="center" alt="Keyboard Control Diagram">
    </figure>
    <p style="text-align: center;"><i>Keyboard controls.</i></p>
</div>

***

<h3 id="EnemyDesign">Enemy Design</h3>

Enemy Characters (EC) were a key part of the story narrative and had to provide a significant threat to the Player Character on their way into the house. The ECs were to be varied, and pose different levels of threat. As the PC moved through the game, the EC would become more difficult, and encourage the PC to seek out weapons or items to help them through the level (as prompted by the inner monologue of the PC).

> [!NOTE]
> The models chosen for the ECs has a pop of orange colour to signify danger, and would be identifiable within the general greyscale colour palette.

The 3 Enemy tiers I came up with were as follows:

<h4>1. "Point and Shoot"</h4> 
A basic *introductory* enemy encountered at the beginning of the level to get the Player accustomed to the combat controls. Straightforward and quick to kill on the off chance that the PC only had a limited number of weapons (or none at all).

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/PointAndShoot.png" width="500" class="center" alt="Lower-level Enemy Design">
    </figure>
    <p style="text-align: center;"><i>Lower-level Enemy design</i></p>
</div>

<h4>2. "Tank-O"</h4> 
This enemy would be difficult to eliminate, with plenty of health, but weak on damage. These would be positioned at "gateways" to other parts of the level, e.g. the staircase leading to the next floor, or doorways to important rooms. 

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Tank-O.png" width="500" class="center" alt="Tank Enemy Design">
    </figure>
    <p style="text-align: center;"><i>Tank Enemy design</i></p>
</div>

<h4>3. "Multiplier"</h4> 
The most dangerous EC due to the tendency to multiply. The gas canisters positioned by their shoulders - used as a defense mechanism - when pierced cause hallucinations and cause clones to appear. They are easy to kill but are best dealt with at long range and require good aim (encouraging skill from the Player).

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Multiplier.png" width="500" class="center" alt="High-level Enemy Design">
    </figure>
    <p style="text-align: center;"><i>High-level Enemy design</i></p>
</div>

<h3 id="EnemyImplementation">Enemy Implementation</h3>

The implementation of the Point-And-Shoot and Tank-O Enemy Characters was a simple matter of inheriting the `BaseEnemy` class and adjusting the amount of `Health`, the `Damage` the Enemy could deal and setting up the appropriate mesh and animation.

The implementation of the Multiplier Enemy, however, was more elaborate and required custom code. 

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/multiplier_GIF.gif" width="500" class="center" alt="Multiplier Functionality">
    </figure>
    <p style="text-align: center;"><i>Multiplier "cloning" Functionality.</i></p>
</div>

The Multiplier Enemy inherits the `BaseEnemy` functionality but is spawned with two additional `StaticMeshes` (<a href="https://github.com/thislavrenchuk/for_future_project/blob/main/Source/Hunter/Bubble.cpp">Bubble.cpp</a>) that represent the gas cannisters that cause the hallucinations resulting in what the Player Character sees as "cloning". 

Upon being damaged, the "canister" kicks off the `Multiply()` function and disappears. 

<a href="https://github.com/thislavrenchuk/for_future_project/blob/main/Source/Hunter/Bubble.cpp#L75-L95">*Bubble.cpp*</a>

```
float ABubble::TakeDamage(float DamageAmount, struct FDamageEvent const& DamageEvent, class AController* EventInstigator, AActor* DamageCauser)
{
	float DamageToApply = Super::TakeDamage(DamageAmount, DamageEvent, EventInstigator, DamageCauser);

	// When an arrow hits the Bubble, it should (1) spawn another Multiplier enemy AND (2) burst/die
	
	// 1. Spawn Multiplier upon being hit
	Multiply();

	// 2. Switch off capsule collision
	StaticMeshComponent->SetCollisionProfileName(TEXT("OverlapAll"));
	this->SetActorEnableCollision(false);
	// 3. Disappear
	StaticMeshComponent->SetVisibility(false);

	return DamageToApply;
}
```

The `Multiply()` function is responsible for spawning a new Enemy (i.e. a clone) in a location that is visible to the Player and does not overlap with either the Player or the other Enemies. 

<a href="https://github.com/thislavrenchuk/for_future_project/blob/main/Source/Hunter/Bubble.cpp#L112-L160">*Bubble.cpp*</a>
<div>
    <pre style="height: 500px; overflow: scroll;">
        <code>
void ABubble::Multiply()
{
	// Spawn a new Multiplier somewhere nearby
    FNavLocation SpawnLocation;
	UNavigationSystemV1* NavigationSystem = FNavigationSystem::GetCurrent&lt;UNavigationSystemV1&gt;(GetWorld());
	FVector OriginalEnemyLocation = GetActorLocation();
    APawn* PlayerPawn = UGameplayStatics::GetPlayerPawn(GetWorld(), 0);
	if (!NavigationSystem)
	{
		return;
	}
	
    bool bFoundGoodSpot = false;
    bool bSuccessfullyGenerated = false;
	bool bSpawnLocationInFrontOfPlayer = false;
	bool bNoOverlapWithOriginalEnemy = false;
	bool bNoOverlapWithPlayer = false;
	
	do
	{
		bSuccessfullyGenerated = NavigationSystem-&gt;GetRandomReachablePointInRadius(OriginalEnemyLocation, 500.0f, SpawnLocation);
    	// Check that it's visible to player
		bFoundGoodSpot = CheckSpawnLocationLineTraceToPlayer(SpawnLocation.Location);
		// Check that it's in front of the player
		bSpawnLocationInFrontOfPlayer = CheckSpawnInFrontOfPlayer(SpawnLocation.Location);
		// Check that it's not overlapping with original enemy 
		double DistanceBwOriginalEnemyAndTwin = (OriginalEnemyLocation - SpawnLocation.Location).SizeSquared();
		bNoOverlapWithOriginalEnemy = labs(DistanceBwOriginalEnemyAndTwin) &gt;= 100000;
		// Check that it's not overlapping with player 
		double DistanceBwPlayerAndTwin = (PlayerPawn-&gt;GetActorLocation() - SpawnLocation.Location).SizeSquared();
		bNoOverlapWithPlayer = labs(DistanceBwPlayerAndTwin) &gt;= 100000;
	} while (!bFoundGoodSpot || !bSuccessfullyGenerated || !bSpawnLocationInFrontOfPlayer || !bNoOverlapWithPlayer || !bNoOverlapWithOriginalEnemy);

	// If successful, spawn new enemy
	FActorSpawnParameters EnemySpawnParameters;
	EnemySpawnParameters.SpawnCollisionHandlingOverride = ESpawnActorCollisionHandlingMethod::AdjustIfPossibleButAlwaysSpawn;
	EnemySpawnParameters.bNoFail = true;
	// Spawn an actor that will fall in an arch to where a new Enemy will spawn	
	AActor* Sphere = GetWorld()-&gt;SpawnActor&lt;AActor&gt;(this-&gt;GetActorLocation(), this-&gt;GetActorRotation(), EnemySpawnParameters);
	if (ParticleEffect)
	{
		UGameplayStatics::SpawnEmitterAttached(ParticleEffect, Sphere-&gt;GetRootComponent(), NAME_None, Sphere-&gt;GetActorLocation(), Sphere->GetActorRotation(), EAttachLocation::SnapToTarget, false, EPSCPoolMethod::AutoRelease);
		FVector NewVector = FMath::VInterpTo(Sphere-&gt;GetActorLocation(), SpawnLocation.Location, ParticleDeltaTime, ParticleInterpSpeed);
		Sphere-&gt;Destroy();
	}

	// Spawn Enemy
	ABaseMultiplierEnemy* MyTwin = GetWorld()-&gt;SpawnActor&lt;ABaseMultiplierEnemy&gt;(MultiplierClass, SpawnLocation.Location, GetOwner()-&gt;GetActorRotation(), EnemySpawnParameters);
}
        </code>
    </pre>
</div>

The functionality responsible for spawning the new "clone" in a location visible to the player took multiple iterations to get right. In the end, the dot product worked best, as shown in the code snippet below.

<a href="https://github.com/thislavrenchuk/for_future_project/blob/main/Source/Hunter/Bubble.cpp#L97-L110">*Bubble.cpp*</a>

```
bool ABubble::CheckSpawnInFrontOfPlayer(FVector SpawnLocation)
{
	// First vector is the player forward vector
	APawn* PlayerPawn = UGameplayStatics::GetPlayerPawn(GetWorld(), 0);
	FVector FirstVector = PlayerPawn->GetActorForwardVector();
	// Second vector is the direction vector from player to target enemy location
	FVector SecondVector = SpawnLocation - PlayerPawn->GetActorLocation();
	SecondVector.Normalize();
	// Workout the dot product
	double DotProduct = FVector::DotProduct(FirstVector, SecondVector);
	// DotProduct >= 0 if the enemy is in front of player
	// and DotProduct < 0 if the enemy is behind the player
	return DotProduct >= 0;
}
```

***

<h3 id="LevelDesign">Level Design</h3>

With the above in mind, I envisioned a setting that varied in available space, to allow for both long-range and close-quarters combat in which the player would have the opportunity to use the varied melee attacks. A house, through which they can discreetly make their way, get rid of enemies, to ultimately reach the top floor where the goal of the game lies. 

<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_LevelDesign_StoryPoints.png" width="500" class="center" alt="Level Pre-requisites">
    </figure>
    <p style="text-align: center;"><i>First Draft Level Pre-requisites</i></p>
</div>
<div style="display: grid; place-items: center;">
    <figure>
        <img src="{{ site.baseurl }}/assets/images/Archer_Milanote_LevelDesign_Map.png" width="500" class="center" alt="First Draft Level Design">
    </figure>
    <p style="text-align: center;"><i>First Draft Level Design</i></p>
</div>

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


***

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