---
layout: post
title: How to spine for optimization
image: \images\howtospine\exportsettings.png
date: 2024-07-26
category: tutorial
excerpt: "A small tutorial with several advices for spine optimization for small games."
tags: [tutorial, spine]
---

Spine Animation for Playable Projects


### Spine Animation for Playables
_early access_

_Text not proofread, read at your own risk_

Preparing Spine animations for playables has several differences compared to preparing animations for other types of projects. The two main principles to follow are lightweight animations and small atlases.

### Key Points

A playable game usually should not exceed 5 MB. In rare cases, it can be around 2-2.5 MB.

Within this 5 MB limit, you need to fit the code, static assets (UI, backgrounds, props, effects), sound, music, and finally, Spine animations.

For animators, this means they have 1-2 MB available for animations. Not much room to maneuver, but it's workable.

Another crucial point is that the animations themselves need to be as lightweight as possible. Animators should limit the use of features like meshes and constraints.

#### Why?
These animations are played within a game engine or browser, essentially as a "game within a game" pulled from external sources. Such games need to run equally well across all devices — from old calculators and toasters to washing machines and irons. Maintaining this performance is challenging if your skeleton has more than 50 bones, meshes with more than four vertices, or more than two weights per vertex. More on this below.

Example metrics from a lightweight character with room for optimization:

![](/images/howtospine/spinemetrics.png)

### Weight and Skeleton Organization

The export weight depends on two things: the atlas size and the animation JSON size.

The JSON will be lighter if the project isn't overloaded with bones, which depends on the number of animations, bones, and keys used in those animations.

A simple rule: arms consist of three bones — shoulder (hand), forearm (arm), palm (palm). Legs also consist of three bones — thigh (hip/leg), shin (shin), foot (boot). The body should have one or two bones, the neck should be merged with the body if possible, and the head should be separate.

Merge pupils and facial features like eyebrows together, and organize emotions using attachment lists for slots. If hair or cloth animation is needed, limit it to 1-3 bones per entity.

A typical HC character skeleton:

![](/images/howtospine/skeletonexample.png)

Save yourself some trouble by creating a bone under the root called "container," "scale," or "main." In the screenshot above, there's both a Container and Main bone. This is because character art often comes in sizes like x2, x1.5, x3, and animators need to scale the skeleton accordingly. The root bone should never be altered in any animation (no rotation or scaling), so use the Container or Main bone for scaling.

All other skeleton parts should be under these bones. The Container should be scaled in the setup, while the Main can be adjusted within animations if needed.

### Naming Conventions and Skins

Short names with three to four letters work better for long-term projects. For instance, naming the left and right body parts as seen from the viewer: HandLeft, ArmLeft, PalmLeft for the left arm, and HandRight, ArmRight, PalmRight for the right arm.

Sometimes developers request specific naming conventions, such as using lowercase letters and separating words with hyphens: ArmLeft becomes arm-left. This can be easily changed using Spine's search tool:

![](/images/howtospine/searchusing.png)

When using IK, add the "IK_" prefix to the bone name. For example, BootLeft used for leg IK can become IK_BootLeft.

![](/images/howtospine/ikreglament.png)

In the example above, I used four IK bones: two for the feet and two for the hands. Usually, only feet are necessary, but in this case, there were many animations involving weapons and objects, requiring more control.

In projects with many skins, frequent renaming of attachments is needed. One easy solution is to add a suffix like _base, _hero, or _main to the base skin, and other suffixes to specific skins, like _knight.

For example:

![](/images/howtospine/skinsexample.png)

### Animation and Mesh Optimization

A playable animator is more of a technical animator than a creative one. The fewer keys, bones, and meshes, the better.

Ideally, the rule should be followed: three bones per body part, and emotions should be attached to a single slot and swapped in animations.

When exporting animations to JSON, don't forget to check the "Animation Cleanup" option.

Idle animation can be controlled with just three bones:

![](/images/howtospine/idleexample.png)

Playable animators aren't about rich animations but about technical execution and strict limitations.

Meshes are a cornerstone of animation optimization. Use them only when absolutely necessary, such as for complex clothing or hair. Keep vertex weights low:

![](/images/howtospine/meshesexampleone.png)

Another example:

![](/images/howtospine/meshesexampleuno.png)

And a more complex case:

![](/images/howtospine/meshesexampletwo.png)

### Art Optimization

Large sprites should be sliced into smaller pieces. Duplicate or similar sprites should not be used across skins or within a skeleton.

For example, if left and right hands are similar, only use one sprite:

![](/images/howtospine/palms.png)

The same applies to legs and boots:

![](/images/howtospine/shins.png)

Reduce white space around sprites and ensure the sprite boundaries are tight. Here's an example of improper vs. proper sprite usage:

![](/images/howtospine/boots.png)

The final atlas should ideally be 512x512 or smaller. Check "Power of Two" in export settings.

![](/images/howtospine/exportsettings.png)

Thanks for reading! Have questions? Feel free to reach out!

![](/images/howtospine/wheretowrite.png)
