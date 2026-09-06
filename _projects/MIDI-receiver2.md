---
title: "Let's Build: Musical Keyboard MIDI Receiver!"
difficulty: "Beginner"
toc: true
toc_sticky: true

---
## Introduction

This is a great first project if you're new to the world of electronics.

Not only is it beginner-friendly, requiring only a few components, but it's also extremely useful, and could mark the first step of building your own musical synthesiser, or software instrument / VST plugin!

**So, what is a "MIDI receiver"?**
{: .notice--primary}

* A MIDI receiver is a device which can **read, understand** and **act on** messages sent by a **MIDI source.**
* A MIDI source is often is a piano or keyboard. In a musical context, this is the instrument or surface that the user interacts with in order to produce sound.
* Today, we're going to build a device which recognises key presses on your piano keyboard, and identifies the **pitch**, **duration,** and **velocity** (loudness) of each note played on a computer.
* MIDI receivers are often programmed to send signals to other pieces of hardware. For example, in a synthesiser, you might want your MIDI receiver to change the pitch of your *oscillator* or change the volume output of your *amplifier*.

**MIDI**, which stands for **Musical Instrument Digital Interface**, is a standard that allows musical instruments to communicate with other pieces of hardware. It was jointly developed in the 1980s, in a rare collaboration between Japanese and American synthesiser manufacturers such as Roland and Moog, with the aim to reduce development time and costs by standardising connections to musical instruments. MIDI proved to be popular and robust, and is basically identical to the version of MIDI used today.

MIDI messages are made up of binary bits (either a 0 or 1) organised into bytes (groups of 8 bits). Each MIDI byte will carry information such as the **type** of message being sent, and the **pitch** and **velocity** of any notes that are played. A typical MIDI message might look like the following:

**10010000 00111100 01111111**

* The first byte here is the code for a **Note On** event (1001) on MIDI channel 0 (0000). This indicates that a note has been pressed on your keyboard.
* The second byte represents the **pitch** of the MIDI note related to the Note On event as a binary number, prefixed by a zero. In this example, MIDI note 60 is being sent, which corresponds to middle C (C4).
* The third byte represents the **velocity** of the note played as a binary number, prefixed by a zero. In this example, a note with velocity 127 is being sent. This is the maximum value possible, indicating that middle C is being played as hard as possible.

MIDI is a type of **serial** protocol. This means that instead of sending all the bits at the same time, all the bits of each MIDI message are sent one after the other very quickly. Serial protocols are a good way of sending data between devices as they reduce the hardware needed, and eliminate the risk of errors due to data sent down two wires at the same time arriving at the receiver at slightly different times. In this project we'll leave a microcontroller to deal with all the serial processing, but if you're looking for a challenge you can make your own serial interface.

If you want to know more about the specifics of MIDI, here is a link to the official 86-page MIDI v1.0 Detailed Specification.

<div class="notice--primary">
  <p>Download the MIDI v1.0 specification here (not recommended for bedtime reading):</p>
  <a href="https://midi.org/midi-1-0-detailed-specification">https://midi.org/midi-1-0-detailed-specification</a>
</div>

The reason that this project is marked with all three of the "Beginner", "Intermediate" and "Advanced" tags is because the possibilities of what you can do with a MIDI receiver are virtually endless. Read on to learn more.

---

## Ingredients

* **A laptop or computer**. Any computer will work fine here really, whether it runs Windows, MacOS or Linux.
* A **microcontroller**. I'd recommend an Arduino Uno or a similar clone (like the Elegoo Uno) if you're new to microcontrollers. This would also work well with an Arduino Nano or an ESP32 if you have one of these, but you'll probably need to attach these to a small breadboard to connect up stuff later.

{: .notice--primary}

<div class="notice--primary">
  <a href="https://www.amazon.com/ELEGOO-Controller-ATmega328P-Compatible-Arduino/dp/B0B6VV7MS7">
    <img src="/assets/images/20260829-elegoo-r3-standalone.jpg" class="align-left" style="width: 10%;" alt="Starter Kit">
  </a>
  <p><strong>ELEGOO UNO R3</strong></p>
  <p>An Arduino Uno clone, which works identically to an Arduino Uno but is much cheaper.</p>
  <a href="https://www.amazon.com/ELEGOO-Controller-ATmega328P-Compatible-Arduino/dp/B0B6VV7MS7" class="btn btn--primary">View on Amazon.com</a>
</div>
<div class="notice--primary">
  <a href="https://www.amazon.com/ELEGOO-Boards-Mini-B-Cables-ATmega328P/dp/B0F6Y7GS4Q">
    <img src="/assets/images/20260829-elegoo-nano-standalone.jpg" class="align-left" style="width: 10%;" alt="Starter Kit">
  </a>
  <p><strong>ELEGOO Nano USB-C</strong></p>
  <p>An Arduino Nano clone, which works identically to an Arduino Nano but is much cheaper. This one comes with three boards for $15! You'll need to attach this to a breadboard to connect jumper wires to it.</p>
  <a href="https://www.amazon.com/ELEGOO-Boards-Mini-B-Cables-ATmega328P/dp/B0F6Y7GS4Q" class="btn btn--primary">View on Amazon.com</a>
</div>

<div class="notice--primary">
  <a href="https://www.amazon.com/HiLetgo-ESP-WROOM-32-Development-Microcontroller-Integrated/dp/B0718T232Z">
    <img src="/assets/images/20260829-esp32.jpg" class="align-left" style="width: 10%;" alt="Starter Kit">
  </a>
  <p><strong>ESP32</strong></p>
  <p>If you already have some experience with microcontrollers, the ESP32 is a great choice. Again, you'll need a separate breadboard. The ESP32 is more powerful, with more configurable outputs, and hardware serial interfaces and can still be programmed like an Arduino. Some of the more advanced things later in this project are recommended for an ESP32 as they will not work on a standard Arduino-based microcontroller.</p>
  <a href="https://www.amazon.com/HiLetgo-ESP-WROOM-32-Development-Microcontroller-Integrated/dp/B0718T232Z" class="btn btn--primary">View on Amazon.com</a>
</div>

* A **USB cable**. This is to connect your microcontroller to your computer. More often than not, this will be USB-C. I trust you already have plenty of these lying around, but make sure it has sync capability (i.e. it's not just a charging cable and can transfer data). These will often come with your microcontroller if you buy one of those starter kits.
* A **keyboard**, **synthesiser**, **piano** or similar. It **must** have either a **USB-B** socket or a socket labelled "**MIDI OUT**" on the back in order for it to be suitable for this project.

<div class="notice--warning">
  <p><strong>Warning:</strong> This project takes one of two paths depending on whether your keyboard has a USB-B socket or MIDI OUT socket. Check this first now before reading on. If your keyboard has both, take your pick.</p>
</div>

---

### If you have a **USB-B socket**:


* A **USB-B** cable. This is for connecting your keyboard to your computer. This is in addition to your other USB cable used to connect your computer to your microcontroller.
* That should be everything :)

---

### If you have a **MIDI socket**:

If you have a MIDI socket, things become a little more fun and involved. If it's any consolation, this is the correct equipment required to build a MIDI receiver, as detailed in the official MIDI v1.0 specification. USB-B is a bit cheaty, this is the proper way to do it!

* A **6N137** optocoupler.

<div class="notice--primary">
  <img src="/assets/images/20260829-6n137.jpg" class="align-left" style="width: 10%;" alt="Starter Kit">
  <p>The 6N137 is a specialised integrated circuit. The deeper into electronics you go, the more components you'll require that can't be found on Amazon. Amazon is a general-purpose consumer marketplace, not a dedicated electronics supplier.</p>
  <p>My advice is to search for the keyboard "6N137 optocoupler DIP" into your search engine. Your search engine will automatically recommend some suitable sites for your region. I live in the UK; some sites I can recommend here for cheap postage and fast delivery are Bitsbox, Cricklewood Electronics, Switch Electronics and RS Components.</p>
</div>

<div class="notice--warning">
  <p><strong>Warning:</strong> It is essential that you use this type of optocoupler for this project. Cheap optocouplers like the PC817 and 4N25 that often come in beginner electronics kits are not able to keep up with the speed of MIDI messages and you'll end up with garbled data. I've tested this myself, please take my word for it.</p>
</div>

* A **breadboard**. This is for connecting up our little circuit centred around our 6N137 optocoupler.
* Two **resistors**. You'll need one 220 ohm, and one 10 kiloohm resistor.
* A **diode**. A 1N4148 or 1N914 will do the trick. Do not use a Schottky diode, Zener diode or LED. Search term would be either "1N4148 diode" or "1N914 diode". Try to get this from the same website as your optocoupler if you can, if you want to save on postage.
* Some **jumper wires** to connect things together.
* Some **soldering equipment** to connect your MIDI cable to your circuit.

<div class="notice--primary">
  <a href="https://www.amazon.com/ELEGOO-Starter-Tutorial-Compatible-Official/dp/B01DGD2GAO">
    <img src="/assets/images/20260829-elegoo-kit.jpg" class="align-left" style="width: 10%;" alt="Starter Kit">
  </a>
  <p><strong>Money-saving tip: </strong>To get the breadboard, jumper wires and the resistor values you'll need along with the microcontroller, I'd recommend buying one of these Arduino starter kits on Amazon. Reminder that you'll still need to buy the diode separately as it's not included. </p>
  <a href="https://www.amazon.com/ELEGOO-Starter-Tutorial-Compatible-Official/dp/B01DGD2GAO" class="btn btn--primary">View on Amazon.com</a>
</div>

If you're a university, college or vocational student, I'd highly recommend getting in touch with your engineering department. From my experience, more often than not they'd be happy to give or lend you some of this equipment. Basically everything except the 6N137 is very cheap and common so there's a solid chance they'll have them :)
{: .notice--primary}

---

## Getting started

First, you'll need to install a software for writing and uploading microcontroller code on your computer. I'd recommend **Arduino IDE** as it's easy to get started with.

<div class="notice--primary">
  <a href="https://docs.arduino.cc/software/ide/">
    <img src="/assets/images/20260829-arduinoide.png" class="align-left" style="width: 10%;" alt="Starter Kit">
  </a>
  <p><strong>Download Arduino IDE from the official website here.</strong></p>
  <a href="https://docs.arduino.cc/software/ide/">https://docs.arduino.cc/software/ide/</a>
</div>

If you have some experience with PlatformIO in VSCode, that's fine too, but make sure your framework is set to "arduino" in your platformio.ini file after you create a project (even if you have an ESP32).

Once you've installed Arduino IDE via the executable and opened the program, you might be asked to give permission to a bunch of stuff. Click "OK" for all of this. All going well, it should look something like this.

<div>
  <img src="/assets/images/20260830-arduinoideui.png" style="width: 80%;" alt="Arduino IDE UI">
</div>


1. Connect your Arduino or other microcontroller to your computer using a USB cable.
2. Click **"Select Board"** above the code editor window. Your computer should have recognised that *something* has connected to one of its USB ports (COMx) but you need to tell Arduino IDE what kind of microcontroller it is.
3. Select either **Arduino Uno**, **Arduino Nano** or **ESP32 Dev Board** from the drop-down depending on which microcontroller you have.


Your microcontroller will not support the MIDI library by default. You'll need to install a library to be able to process MIDI messages.
{: .notice--primary}

1. Click on the books icon in the left hand toolbar. This should open the **Library Manager** and you should be able to search for a library.
2. Search for "MIDI library Francois Best" in the search bar. The library in the image below should be the correct one. Click **Install** to get the library onto your computer.

<div>
  <center>
  <img src="/assets/images/20260830-midilibrary.png" style="width: 20%;" alt="Arduino MIDI library">
  </center>
</div>
<br>

(If you're using PlatformIO instead of Arduino IDE, your platformio.ini config file should look like this. PlatformIO will automatically install the MIDI library if it's included in this file.)
{: .notice--primary}

<div>
  <center>
  <img src="/assets/images/20260830-platformioconfig.png" style="width: 60%;" alt="platformio.ini">
  </center>
</div>

All the required software and libraries should now be installed :) Before we start writing some code, if you're using a MIDI cable we'll need to build our little optocoupler circuit first.

---
## Next steps - if you're using a MIDI cable

Connect up your 6N137 optocoupler on your breadboard as shown like this. Place your 6N137 in the little groove in the middle - this is meant for ICs like this one.
<div>
  <center>
  <img src="/assets/images/20260830-optocoupler-circuit.jpg" style="width: 50%;" alt="optocoupler circuit">
  </center>
</div>

<br>
...or if you'd prefer a schematic diagram, it looks like this.
<div>
  <center>
  <img src="/assets/images/20260830-optocoupler-schematic.png" style="width: 60%;" alt="optocoupler schematic">
  </center>
</div>

All integrated circuits, including this one, have their pins labelled in an anticlockwise fashion starting from the bottom left. This means that the bottom left pin is Pin 1, and the top left pin is Pin 8.
{: .notice--primary}

* Ensure that the top left hand pin of the optocoupler (pin 8) is connected to the "3V3" socket on your microcontroller, and that the top right hand pin (pin 5) is connected to one of the "GND" sockets on your microcontroller. This can be done with jumper cables either directly, or via the breadboard power rails (as I've done here).
* The diode should be connected so that the cathode (marked with a black stripe) is connected to pin 2, and the anode (the other end) is connected to pin 3.
* If the legs on some of the components are too long, use a pair of scissors or wire cutters to trim them down.
* The output of the optocoupler is pin 6.
* Pin 1, 4 and 7 can be safely left unconnected.

I realise we haven't actually talked about what this circuit we've made is for, or what an optocoupler is. Let's talk about this now.

### What is an optocoupler?
An optocoupler is a device made up of an LED and photodiode placed very close to one another. 
* Your keyboard sends binary MIDI messages as a train of high and low voltages. 
* When a high voltage pulse reaches the LED in your optocoupler, a tiny LED inside the chip lights up, and the photodiode detects the LED's light and outputs a high voltage of the same size as the input.
* Likewise, when a low voltage pulse is received, the internal LED turns off, the photodiode detects the LED has turned off, and outputs a low voltage of the same size as the input.
* (this is an extremely simplified model of how an optocoupler works.)

It sort of seems like nothing useful is happening here, as the optocoupler output is identical to its input. That is, until you realise that the optocoupler input (our keyboard) is *completely electrically isolated* from the output (our microcontroller). There is no direct wire connection from our keyboard to our microcontroller.

This has one key benefit:
* **The keyboard's ground connection is kept separate from the microcontroller's ground connection**. Running long, shared ground connections is generally pretty bad for signal quality, as the two devices on either side of the connection will have slightly different ground potentials due to the resistance of the wire. This creates something known as a "ground loop" where a current flows in the ground wire, picking up random electromagnetic interference. Isolating the ground of the keyboard and the microcontroller reduces the chance of errors.
* In addition, in the unlikely event that the keyboard experiences some sort of electrical fault causing a large current to flow to ground, your microcontroller will be kept intact as no current can flow to it.

### Connect up your MIDI cable








