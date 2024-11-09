# pico-interactive-origins

Moved from https://github.com/danielbloy/pico-interactive/originals

Contains the code for the original MicroPython and CircuitPython projects
that form the inspiration for this project. See the section [Origins](#origins) below
for some background on those projects.

## Origins

So how did this project come about? Maybe some background will help to explain.
The images directory contains a small number of images and videos that
show some of the finished electronics. For the Halloween project, I uploaded a
[video to YouTube](https://youtu.be/a0I0U5x334Y), so you can get a feel for what
it was like. The video doesn't quite do it justice but its good enough to get an
idea.

### Halloween 2023

For the full details of the Halloween project 2024 and beyond, pop over to the full
project at https://github.com/danielbloy/pico-interactive-halloween.

Halloween 2023 [video on YouTube](https://youtu.be/a0I0U5x334Y)

My daughter loves Halloween and after Halloween 2022 (she was 8 years old at the
time) we discussed using some of the electronics that I use in my Coding Clubs to
make our house more interactive with sounds and lights. I agreed to this because
how hard could it possibly be?

In June of 2023 I started working on ideas and designs that we could put into the
garden. I wanted to minimise wiring so Wi-Fi communications between the devices
seemed the most sensible approach. I then built up a number of devices to perform
the actual interactive parts of the display. What we had was a standardised box
that had a speaker, could control NeoPixel strands and communicate to any other
device (via a coordinator node) in the network. There were 5 such devices:

* 1 x Pico box on each side of the path that had 6 glass skulls. The skulls had
  NeoPixels in them which I controlled to make them look like they were flickering.
  The devices turned on the skulls in pairs to coincide with church bells that were
  being played from another device.
* 1 x Pico box that controlled a bubbling cauldron (NeoPixels in green). This had
  witches voices playing.
* 2 x Pico boxes that had large LED buttons that when pressed made some actions happen.

Each of those 5 devices actually had two Picos in them. All the original code (in
particular the networking code) was written in MicroPython. This worked beautifully.
However, when it came to the sound processing (which I added last), I was short of
time so just had to get something that worked. In the end I resigned to use
CircuitPython because it had everything I needed already built in. Because time was
limited, rather that get the CircuitPython code working on MicroPython I took the
simpler approach of adding another Pico and physically connected the devices together
allowing the MicroPython board (which was the primary device) to control the
CircuitPython board, telling it which sound sample to play. Space in the box was
cramped when there was one Pico.

In addition there was another Pico box that contained a IR sensor and ultrasonic
sensor which kicked off the whole display when someone got close enough to the house
(it played a sound, made the LED buttons light up and pulse).

There was also a Pico in the house that was coordinating all the other devices.

We also had an [Adafruit ItsyBitsy M4 Express](
https://shop.pimoroni.com/products/adafruit-itsybitsy-m4-express-featuring-atsamd51?variant=12519303086163)
board that I built into a MakeCode Arcade compatible device. I modified (and fixed
lots of bugs) in some freely available code that generated an eye. The eye blinked
and could be controlled to look left and right. I output the display signal to two
LCD screens to make a pair and then we housed this in a box behind a mask which my
daughter designed. The effect was a very creepy face that looked like it was watching
you. We combined this with another Pico with larger storage (a
[Tiny 2040](https://shop.pimoroni.com/products/tiny-2040?variant=39560012267603))
from Pimoroni which played background music.

There was so much more that we started building (spiders with LED eyes that pulsed
got mostly done) but sadly time was against us. I literally spent 100's of hours
on this project (much of it with the electronics and building). The whole setup was
very cool but I learned a lot. The key takeaways were:

* Picos do not have a lot of RAM. The Pico device coordinating the other devices did
  not have enough RAM to concurrently talk to all devices. Therefore communications had
  to be batched up which complicated the coding a bit. The coordinator in future
  versions needs to be a proper computer like a Rasbperry Pi or a PC.
* The wireless communications contains delays. This is not normally a problem except
  where you need two devices to sync very accurately. This was evidenced by the two
  devices that controlled the path. Occasionally, the delay was a few tenths of a second
  and this resulted in a visible difference in the timing of the skulls lighting up. The
  solution is to either have one device control both sides or have a more accurate way
  to sync the two devices.
* The buttons mostly got missed. Even though they had lights and pulsed, most children
  didn't realise they could press them for something to happen. The sesnors worked much
  better because they were fully automated.
* Using a coordinator allowed for a lot of control but also introduced complexity. It
  also required to be calibrated to a typical walking speed but the actual walking speed
  varied significantly based on the age of the groups and whether they stopped to look
  at some of the display. A better solution would be to add more sensors (i.e. each
  device has one) to allow the devices to be more autonomous in how thyey are triggered.
* The devices need to be very robust. Because of time, I had a lot of prototype devices
  and these were on breadboards with push in components. This was problematic as they
  were not particularly robust and if knocked could be disconnected. Also, waterproofing
  was more problematic.
* Stick to cables that use only 3 wires. A lot of the cabling I used needed 4 wires
  as I was connecting NeoPixels. Unfortunately, the cable is more expensive and
  weatherproof connectors are more expensive and harder to get hold of. Sticking to
  3 wire cable means that cable is cheap, plentiful and connectors are easy to get with
  lots of options. All future devices will be designed with 3 wire cabling in mind.

### Coding Club Christmas Project 2023

Each Christmas, I like to do a physical computing project with my Coding Club. In the
past we've done projects like Santa Sleighs with BBC MicroBits, Musical pictures with
Picos. This year, I wanted to do a Santa House that had NeoPixels, music and LEDs using
a Pico. The Pico was a standard headered Pico and I made some simple circuits that
contained 3 x LEDs, an 8 NeoPixel ring, a buzzer, a push button and a power connector.
The circuit connected to the front half of the Pico. Because CircuitPython is easier
to use on the Library computers as it is plug and play, I ported a bunch of the
MicroPython code from the Halloween project to CircuitPython. This meant that I had
the same basic project structure and control as for the Halloween project but could
drive the entire project from a single CircuitPython device. The functionality was not
quite as broad as the Halloween project (but more than adequate) and naturally did not
include any networking code. However, it proved it would all work and was robust.

This project was developed using Blinka which made the whole development experience
must simple, faster and less frustrating as I could simply connect the device to
a PC and test the code out without having to keep deploying it. Blinka is slow but
very effective.

The key learnings points were:

* Even though CircuitPython only gives access to a single core, the asyncio library
  is very effective and the Pico is powerful enough to do everything I need with a
  single core.
* Porting code from MicroPython to CircuitPython is reasonably straightforward.
* Blinka is a super useful development and debugging aid, even if the performance of
  the Pico is much slower than running native code.

### Christmas Light Jars

This was a project that I did for Amelia. We used a [Pimoroni Plasma Stick](
https://shop.pimoroni.com/products/plasma-stick-2040-w?variant=40359072301139)
and connected it to the two sets of strands of 6 jars that we used in the Halloween
project. The jars were controlled to run a variety of different NeoPixel displays.
The jars needed to be controlled so that they only ran in the evening at the desired
time. This required implementing some networking code to get the current date and
time but was a useful experience in getting networking code working on CircuitPython
and with Blinka. The main difference between the light jars and the Halloween project
is that the Halloween project was outside for a single day but the light jars were
outside for a whole month. The waterproofing was not up to the job, neither was the
exception handling.

The key learning point was:

* If you are going to leave your device outside in Britain, have good waterproofing by
  designing for good waterproofing from the start.
* If you need your display to work for days and days, make sure you have excellent
  error handling and recovery so your display does not randomly stop working and you have
  to go outside and power cycle it (turn it off and then back on again). Alternatively,
  fudge it a bit by putting an automated reboot of the device every few hours or so!

## License

All materials provided in this project is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0
International License. To view a copy of this license, visit
<https://creativecommons.org/licenses/by-nc-sa/4.0/>.

In summary, this means that you are free to:

* **Share** — copy and redistribute the material in any medium or format.
* **Adapt** — remix, transform, and build upon the material.

Provided you follow these terms:

* **Attribution** — You must give appropriate credit , provide a link to the license, and indicate if changes were made.
  You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
* **NonCommercial** — You may not use the material for commercial purposes.
* **ShareAlike** — If you remix, transform, or build upon the material, you must distribute your contributions under the
  same license as the original.
