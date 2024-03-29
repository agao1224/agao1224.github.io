---
layout: page
title: ScottyChord
description: 
img: assets/img/scottychord1.png
importance: 1
category: fun
related_publications:
---

ScottyChord is a remote-controlled piano built during CMU's annual [Build18 hardware hackathon](https://www.build18.org/). How it works is: you press keys on a laptop to "record" a song, then ScottyChord plays it back to you.

<div class="row">
    <div class="col-sm mt-1 mt-sm-0">
        {% include figure.html path="assets/img/scottychord1.png" title="ScottyChord" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Final project at demo day
</div>

## Software 

The software consists of both the display for interacting with the piano and the microcontroller code that processes the user's input and plays it back on the piano. I built the user interface using `pygame`, and data was streamed from laptop to microcontroller through USB via `pyserial`. The user input was encoded as a specially-formatted string with notes and in-between delays which implements the concept of rhythm. Basically, I take the time between two notes, divide by some small time delta (e.g. 100ms), and encode this into the string as an 'X'. So an example of an encoded song in this format would be:

```
EDCDEEEXXXXDDDXXXXEEEXXXXEDCDEEEEDDEDC (Mary Had a Little Lamb)
```

This was the simplest and most efficient way to encode songs and transfer them over to the microcontroller to process.


The microcontroller then receives a stream of bytes and decodes them into a string of the format above. It goes char-by-char and either calls `delay()` when encountering an `X`, or sends digital signals to various digital pins. These pins are each hooked up to a relay that connects our 9V power supply to one of the motors. When on, the motor spins the hammer and hits the string, producing sound.

### Hardware

There are two components to the hardware: mechanical and electrical. My project partner was responsible for all of the mechanical components (instrument body, strings, structure) while I was responsible for all electrical components (motors, wiring, microcontroller, relay, power).


##### Mechanical

My partner CAD'ed three layers for the instrument body. The strings would be strung across the top layer, set in place by metal pegs; the second layer would be where the playing mechanism would be; and the third layer is where all our electronics would lie. We ordered a huge, 4" x 8" piece of plywood and laser cut all the layers out for the body. Also, my partner plays all sorts of instruments and picked Lyre strings. This was a really important consideration since Lyre strings require much less force to produce sound. Many Build18 groups that chose to build some sort of instrument couldn't get theirs to work because they picked strings that were too heavy to pluck/strum/hit (e.g. guitar strings). Attached to each motor is a small wooden hammer that's used to hit its corresponding string. We also put felt at the bottom of the motor so that the hammers wouldn't clack too loudly when they fell.

##### Electrical

The electrical part consisted of a microcontroller (Arduino Uno) with digital pins connected to various relays. Each relay was connected to a small DC motor, digital pin output of the microcontroller, and a shared 9V battery power supply. When the relay is on, the battery powers the motor, and the hammer hits the string. We originally thought of using one 9V battery per motor (overkill), but realized that a single 9V battery was enough to power the whole system.

## Setbacks

1. <b> Playing mechanism: </b> Originally, we came up with a much more complicated mechanism to pluck the strings, much like that of a Harpsichord. We realized the night before demo day that our motors were too weak and didn't generate enough force to pluck the strings. We experimented with a series of other mechanisms and pivoted last-minute to the motor and wooden hammer mechanism (more like a piano).

2. <b> Warping: </b> While tuning the Lyre strings, we realized that the instrument kept getting out of tune within seconds. After messing around with the strings we found the wood was warping. When we tuned the strings, the wood warped significantly which would quickly slacken the recently-tuned strings and make the instrument out-of-tune again. We fixed this by hackily wrapping tape pulling the top layer and bottom layer together. This actually worked pretty well.
