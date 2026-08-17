---
toc: true
toc_sticky: true
classes: wide
---

As predicted, none of the exams I've had this week went particularly well. But maybe I'll surprise myself like last year when results come out.

---

# This week

Not much progress this week due to exams. As mentioned in last week's update, I hope to start building from around mid-May.

When I haven't been in Laplace transform jail, this week I've been thinking about project timelines and also about possible designs for my VCO. I'll start with discussing project timelines first.

| Project stage                                       | Things to do                                                                                                                                 | Deadline  |
| :-------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------: | :-------: |
| VCO and oscillator core                             | Decide on oscillator type, design wave generator circuits, prototype on breadboards, design PCBs, test initial prototype                     | 31st May  |
| MIDI control integration                            | Linking the VCO to a MIDI source, implement polyphony                                                                                        | 14th June |
| Design of envelope generator, VCA and VCF           | Decide on VCA/VCF types, decide between analog / digital implementations, prototype on breadboards, design PCBs, test initial prototype      | 11th July |
| Buffer period for integration testing / parts delay | Combining VCO, VCA and VCF designs to work effectively together, analysis of noise performance, hopefully prototype design PCBs have arrived | 31st July |
| Improving stability                                 | Adding improved power supply and temperature compensation for more advanced features later                                                   | mid Aug   |
| "Nice-to-haves"                                     | Noise generator, LFO, sample and hold, frequency modulated / sync VCOs                                                                       | Sep       |

If I can get to having a good VCO, VCF and VCA by the end of July, I'll be happy, as this will give me a solid two months to add interesting improvements. I have an assessed university project from May-June, and an internship during July, both of which should come first, so I've given extra time to some of the earlier deadlines.

This plan almost definitely will change during the course of the project. This is my first build at this scale, and my first time making a synthesizer, so I'm prepared to anticipate and embrace delays.

# In the post

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/20260501-cv12.jpg" alt="The CV-12 ORAC chip from Midimuso (£17.49 as part of a kit containing a PCB and other components). Probably based around a Microchip PIC">

The CV-12 is a remarkable chip, which is capable of converting digital MIDI signals into a CV (control voltage) and a trigger (a short pulse that tells an envelope generator / amplifier when a MIDI Note On event is detected).

Many MIDI to CV/Trig converters are very expensive, with good ones costing around £100 each. These are intended more for the Eurorack market rather than for ground-up electronics builds, so don't really fit my requirements or budget.

What makes the CV-12 special is that it is capable of polyphonic MIDI to CV conversion, meaning it is capable of outputting many control voltages (corresponding to many different notes) at once. This means it becomes possible to play chords from the get-go. Being a programmable logic device, it's also fully configurable with various mode changes. Polyphony can be sacrificed for additional MIDI parameters like note pressure and pitch bend, and even VCF parameters like cutoff / resonance. Considering what this thing can do, £17 is an absolute bargain.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/20260501-practicekit.jpg" alt="SMD soldering practice kit, £0.75 from AliExpress">

I also bought an SMD soldering practice kit, if not for learning how to solder SMD components for this project then just for a bit of fun. This consists of a small, and a variety of SMD resistors, capacitors and chips in different sizes to practice with.

I don't think I quite realised just how small SMD components really are - whilst the 1206 and 0805 size look manageable to solder with my iron, the smallest 0402 size ones are absolutely tiny.

