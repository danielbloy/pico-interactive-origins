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

For the full details of the Halloween project 2023 and beyond, pop over to the full
project at https://github.com/danielbloy/pico-interactive-halloween.

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
