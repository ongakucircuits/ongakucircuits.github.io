---
title: "Let's Build: Musical Keyboard MIDI Receiver!"
difficulty: "Beginner"
toc: true
toc_sticky: true
---
## Introduction

This is a great first project if you're new to the world of electronics.

Not only is it beginner-friendly, requiring only a few components, but it's also extremely useful, and could mark the first step of building your own musical synthesiser!

**So, what is a "MIDI receiver"?**
{: .notice--primary}

* A MIDI receiver is a device which can **read, understand** and **act on** messages sent by a **MIDI source.**
* A MIDI source is often is a piano or keyboard. In a musical context, this is the instrument or surface that the user interacts with in order to produce sound.
* Today, we're going to build a device which recognises key presses on your piano keyboard, and identifies the **pitch**, **duration,** and **velocity** (loudness) of each note played on a computer.
* MIDI receivers are often programmed to send signals to other pieces of hardware. For example, in a synthesiser, you might want your MIDI receiver to change the pitch of your *oscillator* or change the volume output of your *amplifier*.

The reason that this project is marked with all three of the "Beginner", "Intermediate" and "Advanced" tags is because the possibilities of what you can do with a MIDI receiver are virtually endless. Read on to learn more.

---

## Ingredients

* **A laptop or computer**. Any computer will work fine here really, whether it runs Windows, MacOS or Linux.
* A **microcontroller**. I'd recommend an Arduino Uno or a similar clone (like the Elegoo Uno) if you're new to microcontrollers. This will also work well with an Arduino Nano or an ESP32 if you have one of these.
* A **USB cable**. This is to connect your microcontroller to your computer. More often than not, this will be USB-C. I trust you already have plenty of these lying around, but make sure it has sync capability (i.e. it's not just a charging cable and can transfer data).
* A **keyboard**, **synthesiser**, **piano** or similar. It **must** have either a **USB-B** socket or a socket labelled "**MIDI OUT**" on the back in order for it to be suitable for this project.

This project takes one of two paths depending on whether your keyboard has a USB-B socket or MIDI OUT socket. **Check this first now** before reading on. If your keyboard has both, take your pick.
{: .notice--warning}

---

If you have a **USB-B socket:**
{: .notice--primary}

* A **USB-B** cable. This should be long enough to reach from your keyboard to your computer. This means that you'll also need two USB ports on your computer - make sure you have enough (for some reason laptop manufacturers in particular love reducing the number of ports on their laptops...?)
* That should be everything :)

---

If you have a **MIDI OUT socket:**
{: .notice--primary}

This makes things a little bit more fun and involved.

* A **MIDI cable**.
* **A** **breadboard**. You'll need one breadboard for your microcontroller, and the other for the additional circuitry required.
* **A 6N137 or 6N138 optocoupler**.

It's essential that you buy an optocoupler of this type. You **must** **not** use a cheap one like the PC817 or 4N35 that are commonly found in beginner electronics kits, as these are not good enough to keep up with the speed of MIDI transmission.
{: .notice--warning}

* A **diode.**
* A few **resistors.** You'll need the following values - one 220 ohm, one 1 kiloohm, and one 10 kiloohm.
* Some **jumper wires** to connect things together
* Some **soldering equipment** for connecting your MIDI cable to some jumper wires.

This is quite a bit more challenging than using USB-B. However you might find comfort in that this is the original, authentic method of building a MIDI receiver, as detailed in the MIDI 1.0 Detailed Specification of the 1980s. USB-B is a bit cheaty, this way you're doing it properly!

{: .notice--primary}

**Money-saving tip:** If you're building your MIDI receiver in this way, you'll probably find it more cost-effective to buy [one of these starter kits. (link to Amazon.com - please find the identical product on your local Amazon eg. Amazon.co.uk)](https://www.amazon.com/ELEGOO-Starter-Tutorial-Compatible-Official/dp/B01DGD2GAO) These come with a USB cable, breadboard, jumper wires and the resistor values you need.

{: .notice--primary}
A complete "MIDI Receiver Component Kit" will shortly be available in the ONGAKU-CIRCUITS shop, with all the components you need for this project. If you'd prefer to order the components yourself, I've provided some friendly links to components that'll exactly match what you need for this project.

{: .notice--primary}
If you're a university, college or vocational student, I'd highly recommend getting in contact with your engineering department to see if they can provide you with any of this equipment. In my experience, they'll often be happy to give you equipment for free if you ask :)

## Directions

1. Preheat the oven to 350 F.
2. In a medium bowl, whisk flour with baking soda, nutmeg and salt.
3. In a large bowl, beat butter with sugar and brown sugar until creamy and light. Add vanilla and eggs, one at a time, and mix until incorporated.
4. Gradually add dry mixture into the butter-sugar wet blend, mixing with a spatula until combined. Add chocolate chips and nuts until just mixed.
5. Drop tablespoon-sized clumps onto un-greased cookie sheets. Bake for 8-12 minutes, or until pale brown. Allow to cool on the pan for a minute or three, then transfer cookies to a wire rack to finish cooling.
