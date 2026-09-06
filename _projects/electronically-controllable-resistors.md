---
title: "Transconductors - electronically controllable resistors made from transistors"
difficulty: "Expert"
toc: true
toc_sticky: true
---
## Introduction

I'm sure you've come across cases where you need a resistor to change its resistance under certain conditions, and have resorted to either manual control with a potentiometer, or used a single BJT or FET restricted by the heavily non-linear behaviour of these in their "linear" region.

For example, in my polyphonic synthesiser project, I found myself needing to make an integrator-based square to sawtooth wave converter that maintains a constant amplitude with frequency. In order to do this, you must vary the current supplied to your capacitor, as you must charge a sawtooth output to its maximum voltage more quickly at high frequencies.

The logical solution from my 1st and 2nd year electronics knowledge would have been to use some sort of current mirror, where a third transistor is used to set a reference current and the current mirror sources this current for the capacitor.


Let's suppose for some reason you need a resistor which is:

* Electronically controllable
* Its resistance must cover a large range
* Perfectly linear in its region of operation
* Insensitive to temperature changes

How would you go about this? In this article we'll discuss some increasingly complex but increasingly accurate methods for making electronically controllable resistors.

## Directions

1. Preheat the oven to 350 F.
2. In a medium bowl, whisk flour with baking soda, nutmeg and salt.
3. In a large bowl, beat butter with sugar and brown sugar until creamy and light. Add vanilla and eggs, one at a time, and mix until incorporated.
4. Gradually add dry mixture into the butter-sugar wet blend, mixing with a spatula until combined. Add chocolate chips and nuts until just mixed.
5. Drop tablespoon-sized clumps onto un-greased cookie sheets. Bake for 8-12 minutes, or until pale brown. Allow to cool on the pan for a minute or three, then transfer cookies to a wire rack to finish cooling.
