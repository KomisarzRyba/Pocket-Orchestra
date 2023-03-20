---
title: Pocket Orchestra
---

# 🎻 Pocket Orchestra

**Midterm project for CIM433 Augmented Reality**

## Description

Pocket Orchestra is an interactive musical experience, that allows the user to shape the soundtrack by choosing different instruments that perform it in real time.

User is given four playing cards - one for each track, and uses them to cover chosen parts of a printed marker.

## Storyboard

![Step1](/images/step1)

## AR
### To button or not to button?

My initial idea for the interaction did not include the playing cards. I planned for the user to simply touch the specified areas on the marker to play their respective tracks. That did not feel very engaging though.

Making the user need to put a physical object on the marker makes the actual interaction more meaningful, and more suitable for an AR application. If you think about it, simply pressing buttons is a very artificial task, completely detached from reality. Introducing the element of persistence (marker needs to remain covered for the music to play) adds a physical representation of the state of the application, therefore better connecting the physical and the virtual world.

### 🚀 Space!

I really wanted to make the app feel like the sound is coming from the "orchestra". Adding sound spacialization allows just for that.

The spatial effect is very subtle though. The limitations of marker based AR do not allow the user to get their device close enough to the marker, without losing tracking. I imagine this problem could be solved by using multiple markers in a very well lit room in addition to positional tracking of the user's device. I leave this for future iterations.

## Implementation issues...

Designing a good marker is the key to a successful AR experience. It took a few iterations to arrive at the final solution. Making the card slots have the highest contrast and detailed patterns printed solved all of the issues I had with the markers I initially envisioned.
