---
toc: true
toc_sticky: true
classes: wide
---

My assessed university group project has completely consumed my time over the past couple of weeks. Some highlights include laying out triangular PCBs (not recommended...), measuring miniscule op-amp offsets, and blaming my horrific spaghetti code on sleep deprivation. Hence, I've struggled to find the time to work on the synthesizer. To be completely honest, it's difficult to find the motivation to work on circuits at home having spent 8 hours in the lab diagnosing uninteresting issues.

Regardless, over the past couple of weeks I've learned a few lessons, and weighed up the balance between analog and digital. More details below.

---
# Project deadline update

My deadline to complete the master VCO was the 31st May. I'm pleased to say I've met this deadline despite the unexpected volume of work from my university project. The only element of the first deadline I haven't met is the PCB design - because lead times are long and postage is so expensive, I'd like to wait until I have more designs before ordering.

| Project stage                                       | Things to do                                                                                                                                 | Deadline  |
| :-------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------: | :-------: |
| VCO and oscillator core                             | Decide on oscillator type, design wave generator circuits, prototype on breadboards, design PCBs, test initial prototype                     | 31st May  |
| MIDI control integration                            | Linking the VCO to a MIDI source, implement polyphony                                                                                        | 14th June |
| Design of envelope generator, VCA and VCF           | Decide on VCA/VCF types, decide between analog / digital implementations, prototype on breadboards, design PCBs, test initial prototype      | 11th July |
| Buffer period for integration testing / parts delay | Combining VCO, VCA and VCF designs to work effectively together, analysis of noise performance, hopefully prototype design PCBs have arrived | 31st July |
| Improving stability                                 | Adding improved power supply and temperature compensation for more advanced features later                                                   | mid Aug   |
| "Nice-to-haves"                                     | Noise generator, LFO, sample and hold, frequency modulated / sync VCOs                                                                       | Sep       |


The next stage is MIDI control integration. I'm less sure I'll meet this deadline, as university project work looks to hotten up even further over the next couple of weeks.

---
# A disappointing accident - digital buffers always need power!
Earlier this week, I connected up one of my TOGs (Top Octave Generators) to the output of my VCO through a CD40106 buffer. For some reason, I decided that the CD40106 didn't need power or ground at all, and connected the floating output, with no reference from the power supply rails, to my TOG.

Around 5 seconds after connecting up the TOG, there was a large spark and audible pop as one of my NPN transistors upstream of the digital buffer blew up.

This caused a short circuit between power and ground. The power supply entered overcurrent protection mode but it was too late - the huge current had already fried one of my four delicate TOGs and made it unusable. At almost 2000JPY each, they weren't cheap, and with them being vintage replicas I was a little disappointed at this loss. So how did this happen?

I realised I completely misunderstood what kind of chip the CD40106 actually is. It's not a typical inverter/buffer per se - it's a Schmitt trigger inverter. Schmitt triggers inverters are devices used for converting noisy or erratic analog voltages into cleaner digital ones. They work by latching the output voltage at either +Vcc or GND depending on the input voltage and reversing the polarity of the input. By chaining two inverters together, we can create a digital buffer.

Schmitt triggers require a +Vcc and GND connection so that the output voltage can latch correctly. If there is no power supply rail, the output voltage becomes floating and behaves erratically, potentially drawing huge amounts of current (which I think is what happened in my case).

This damage would have been prevented by a quick look at the datasheet, and it's absurd this happened as I'm usually quite careful with this sort of thing. Perhaps I relied on the schematic a little too much, as it doesn't explicitly show the power supply rails. TLDR: Always read the datasheet before mindlessly wiring up an unfamiliar IC. (I hoped I wouldn't need to give myself this advice, but here we are.)

---
# Change of mind on oscillator design

At the end of our last update, I was positive we'd go with the oscillator from the RS-505 Paraphonic, as it seemed to be a better design. Whilst this is probably true, it brings a number of complications I hadn't considered earlier.



The RS-505 Paraphonic master VCO uses a varactor diode to control the oscillator's pitch. A varactor diode is a reverse-biased PN diode which functions as a voltage-controlled capacitance. Remember - the RS-505 is centred around a second-order LC resonator, so in order to vary the VCO's frequency we either need to change the resonator's effective capacitance or inductance.

Whilst standard diodes and BJTs can be used as rudimentary varactor diodes by reverse biasing them due to their voltage-dependent parasitic capacitance, this variation in capacitance is far too small (on the order of picofarads) to give us the tuning range we require. Therefore custom varactor diodes must be used, which have a much larger capacitance range (on the order of hundreds of nanofarads).

Finding through-hole varactor diodes is very difficult. Whilst the RS-505 Paraphonic design is a cool one, I think the additional complexity it brings is just not worth the additional circuitry or the unusual components. The ES-50 VCO achieves very similar (possibly even better) stability using fewer components, and is also a RC oscillator, meaning oscillation frequency can be controlled by a variable resistor, which is much more common and easier to work with.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/20260605-es50.png" alt="The Korg Lambda ES-50 DCO">

The ES-50 tuning can be controlled coarsely by varying the 10k potentiometer VR4. In the ES-50 schematic, a negative supply voltage is used for tuning, which is then inverted to give a positive voltage at the output of the op-amp. As my home power supply doesn't have a negative supply rail, in my design I'll replacing this with a positive supply and a potential divider set to have identical output ranges.

---
# Oscillator control - the history

Let's now discuss a little bit about oscillator control. This refers to how we can use an external device to play notes on our synthesizer, and is also where faithfulness of our design to the 1970s/80s begins to fall apart a little.

The most obvious way to control our synthesizer (from a musical perspective) is to use a piano keyboard. For the oscillator to make sound, we need a controller to tell it the frequency corresponding to the key played, and the time the piano key is depressed.

In simple analog synthesis, the controller outputs two waveforms for each note - TRIG and CV. 

TRIG (short for trigger), consists of a short pulse that goes high when any note is pressed. Many analog synthesizers contain a circuit called a trigger-to-gate converter, which extends the pulse to the desired duration of the note and shapes the pulse using capacitors, resulting in a desired amplitude profile of a note known as an envelope.

The confusingly named CV (short for control voltage) consists of a DC voltage that represents the pitch of the note. Typically, higher DC voltages represent higher frequencies. Many synthesizers conform to the "1 volt per octave" rule, which states that as the frequency of the note doubles, the DC voltage increases by 1 volt. Incidentally, there are many other types of CV used to control amplifiers and filters for example, hence the confusing name.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/20260605-trigcv.jpg" alt="Typical waveforms of trigger and CV">

The concept of CV and TRIG made a lot of sense for synth manufacturers who designed their circuitry, keyboards and control interfaces from the ground up. However, if like me you want to control the synthesizer from an existing, fairly modern keyboard, we'll need a different approach.

---
# MIDI

The vast, vast majority of keyboard instruments are now fully digital, and don't have CV or TRIG as they don't use this method of control. Instead, many modern keyboards have a MIDI port on the rear.

MIDI (Musical Instrument Digital Interface) is a communication protocol very similar to UART developed specifically for musical instruments. It was invented in the early 1980s by mutual agreement between several American and Japanese synthesizer manufacturers wishing to make product development cheaper and more standardised. It has remained the standard for interfacing with musical instruments to the present day.

MIDI is perfect for my use case, as it supports polyphony off the bat (amongst many other things), and all of my keyboards already have MIDI ports on the back. The intention is to use a keyboard purely as a MIDI transmitter for my synthesizer, which is ironic considering the keyboard is able to synthesise sound in its own right.

I'm not going to explain the MIDI protocol here in fear of doing it injustice, but a detailed specification and summary of all the possible MIDI messages can be found at the two links below.

> <cite><a href="https://midi.org/midi-1-0-detailed-specification">https://midi.org/midi-1-0-detailed-specification</a></cite>

> <cite><a href="https://midi.org/midi-1-0-detailed-specification">https://midi.org/summary-of-midi-1-0-messages</a></cite>

The most important MIDI messages for now are the "Note ON" and "Note OFF" commands, which contain data equivalent of trigger/CV in a two-byte message. I currently have a MIDI to CV converter kit under construction. As I don't think we'll be using CV, I'll jumper across the conversion circuitry and pass the MIDI data directly into a microcontroller / FPGA to decode our messages for us.

---
# Controller interface design

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/20260605-controllerdesign.jpg" alt="">

This is my current design for a controller interface that takes our MIDI signal input and converts it into a polyphonic pitch output.

* The note encoder takes our input MIDI message and identifies Note On / Note Off events, and outputs 6 pitches (eg. C, D, E, F) and their corresponding octaves (eg. 2, 3, 4, 5) that are played.

* The pitch selector receives the 12 outputs of the TOG and the 6 inputs from the note encoder, and buffers 6 waveforms from the TOG as decided by the note encoder.

* The octave divider takes each of the 6 waveforms sent from the pitch selector and divides their frequency by the required power of 2^n to reach the desired octave.

This is the least hardware-heavy method of implementing polyphony that came to mind. An important design decision was placing the octave dividers after the pitch selector rather than before it, which reduces the number of frequency dividers required from over 70 on a 6-octave keyboard to just six.

The next stage of this project will be to implement some code to achieve this on either a microcontroller or an FPGA.









