---
layout: page
title: cuetips
description:
img: assets/img/cuetips2.png
importance: 4
category: fun
---

## Overview

[cuetips](https://course.ece.cmu.edu/~ece500/projects/s24-teamc0/) is my Electrical & Computer Engineering capstone project. The idea is real-life Gamepigeon 8-ball pool. When a user picks up their cue stick and aims at various cue balls, there will be a projector shining down instantaneous trajectory predictions.

#### [Demo](https://drive.google.com/file/d/1w6YWEdShutXH6WKRZk79FGGyhbYN67lt/view?usp=sharing)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cuetips1.jpeg" title="cuetips1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cuetips2.png" title="cuetips2" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cuetips3.png" title="cuetips3" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br />

## Software

- CV System\
Composed of submodules for detecting different objects on the pool table: cue stick, pockets, walls, cue ball, solids & stripes, etc. We packaged this into a small library which our local server running on the laptop calls.

- Physics Engine\
The physics engine takes in detections per-frame from the CV system in order to make predictions. The output is then a series of trajectories/lines starting from the cue ball which can then be display on the projector.

- Server\
I wrote our backend server in Flask; this is responsible for taking in frames live from the iPhone camera, running it through the CV system, piping the CV output into the physics engine, then displaying the predictions after it runs. These predictions are output onto the laptop screen which we then mirror onto the projector. While this runs continually, we expose an API endpoint /video for the frontend of our web application to query.

- Web application\
The web application serves as a sort of dashboard for configuring the CV system and also recording past shots taken. It's written with React and Javascript, pinging our Flask (Python) backend.

## Physical Components

- Metal frame\
We needed a metal frame to fix both our camera and projector in place above the pool table. We ended up buying a pre-built metal shelf and heavily modifying it to meet our needs. We reached out to a team who worked on a similar project in the past; they spent 2-3 weeks just building their mount from scratch - time that could've been spent writing code. We decided to spent a bit more money to avoid this issue altogether. This was one of the most important decisions we made since it saved so much time.

- Camera\
iPhones have a bluetooth camera function which we utilized in order to take in frames to process. The camera quality of modern phones is more than enough for the purposes of our project. It also helps that we didn't have to spend any additional money from our budget for an external camera. We would mount one of our iPhones to the top of the metal frame pointing downward to capture live video of users playing pool. Sending video to the laptop was also extremely convenient due to Apple's native integrations between iPhone and Macbook.

- Projector\
The projector was used to display trajectory predictions on the pool table below. We bought a small projector for about ~$60-70 and mounted it to the top of the metal frame, next to our camera/iPhone mount.

- Laptop\
We decided to use our own laptops to do the video processing and projection output. We were strongly considering an NVIDIA Jetson Nano or Raspberry Pi earlier on, but decided not two for two reasons:
1. Getting the iPhone to send video frames to a RasPi or Jetson Nano would be a huge time sink. We knew there would be all sorts of compatibility issues, and it's notoriously difficult to develop for iOS. Instead, we exploited the already-built integrations between Apple devices.
2. The processing power of a Macbook would be more than enough to handle whichever CV algorithms we would need for the project. Additionaly, one of the technical feats we wanted to accomplish was real-time predictions, which meant that the end-to-end time for a single frame processed must be less than the average human reaction time (~200 ms). Having this additional computational power would help with this.

#### Pool table
We bought a [mini pool table](https://www.amazon.com/REAHOISY-Portable-Adults-Wooden-Triangle/dp/B0DDCH6P6N/ref=sr_1_30?crid=2ACW4LLPYVRER&dib=eyJ2IjoiMSJ9.Tf3EknzmgiXIFl2Yr6Y5Tk06o6DsnHoTiJBlKqKiRRVIqfRNXC0WReVi5PRpMvpl1oTL8GOUMK09hqVDcZiOusb30ycNoB-9Qt2sBD-RS0hQRIq4fFgzECBjk6ntcwLM12W67IlKukv2lf3NuC2tiUb6bDm6zFuUFssUND4TyxXYGX6Lw21f5lyuiUoj8s2kzoxOsIZb0Chj6l9HURHuPU8HYdD_XBdzYvTPkUzfkmXuWTM19ltEbtrZU4jpigU0dSo4Vi0O43h6n5ZfAoodDOCZKsAKWJ2ALkWPh-4f3myHguh9Z_QWtqXdp3r1IkwwUwq1v4P_K64KxbFGBqO4bGsJtM1lFAEnY_jcmpFMjBee9xX6lkFVv_k9geU0uaRcAHQxFY5El4Pc2I5lBshFKfGuii25IbVyV0-U26z2gXrjvtJ-CyYkJlzyIjs6C7qX.WkafE3RgSxL6BFns6U7X4DPckgk6imCHYr4ouwyTbQY&dib_tag=se&keywords=mini%2Bpool%2Btable&qid=1734554988&sprefix=mini%2Bpool%2Bta%2Caps%2C285&sr=8-30&th=1); a full size one was far too expensive and we wouldn't be able to bring a full size pool table to the demo hall.


## Timeline
Week 1: Ideation\
I made a team with two friends (Tjun Jet, Debrina) I knew coming into the class. We had a number of ideas like a custom bluetooth communication protocol, creating a custom point-of-sale reader (like [this one](https://squareup.com/shop/hardware/us/en/products/chip-credit-card-reader-with-nfc?device=c&gad_source=1&gclid=CjwKCAiAgoq7BhBxEiwAVcW0LE8JLQ18H4BVKt75dtPEyqvYD_Mpi3ot9eLfoBK3CogIxXlbVWm7zxoC6bMQAvD_BwE&gclsrc=aw.ds&kw=PRODUCT_GROUP&kwid=p54463326261&matchtype=&pcrid=48630319297&pdv=c&pkw=&pmt=&pub=GOOGLE)) for college meal swipes, etc. We ultimately decided on making the [iOS Gamepigeon 8-ball pool app](https://apps.apple.com/us/app/gamepigeon/id1124197642) in real life.

<div class="row justify-content-center">
    <div class="col-sm-auto mt-1 mt-sm-0">
        {% include figure.html path="assets/img/gamepigeon-8ball.jpg" title="gamepigeon-8ball" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Week 2: Parts list\
We look at the work from past teams or similar projects to see what parts they used and also what challenges they faced. We took into account these factors when designing our system and compiled together a [list of parts](https://docs.google.com/spreadsheets/d/1m8ZDLl5TgMk4pPMn1DCL9xopwh1EYQadmjA_myjwBM4/edit?usp=sharing). The total came out to around ~$400.

Week 3: Starting development\
Debrina worked on entity detection (pool table walls, cue balls, etc), Tjun Jet worked on the physics engine, and I was responsible for the cue stick detection/tracking and web application. We all worked on the physical mount and setting up the system together. Initially we planned for the scope of the project to be much larger than it turned out to be. For the physics engine + cue stick detection, we wanted to due collision detection with ball spin (hence the IMU and ESP32 module in the parts list).

Week 4-7: Progress and tradeoffs\
The physical system and mount setup was finished in about a week, and we spent the rest of the time writing code for object detection and the physics engine. In order to ensure that our system processed each frame in under ~250 ms, we decided to *use zero machine learning* and solely relied on traditional techniques like masking, color filtering, threshold & contour detection, etc. Additionally, we were planning to use [AprilTags](https://en.wikipedia.org/wiki/ARTag) to detect the cue stick but decided that it would result in a poor user experience. In these weeks, we finished:
- Pocket and wall detection (unstable)
- Partial physics engine (can only compute ball-wall collisions)
- Cue stick detection (unstable) + server-side web application

Week 8: Design Review\
At the midpoint of the capstone class, we were required to write a [design review](https://drive.google.com/file/d/1ZKvjEFzM0V9P12CyuFw8NCZ0gdvfrAfQ/view?usp=sharing) to summarize our progress and the work remaining. The hardest part of the project by far was trying to do the CV part without machine learning. We tested with tools like [YOLO](https://en.wikipedia.org/wiki/You_Only_Look_Once) and found the output predictions to be extremely choppy.

Week 9-12: Rehauling CV\
We tested under a number of different conditions (lighting, different shots, pool stick orientation) and realized our system was nowhere near robust enough to be usable. We went for a near-full rewrite of our CV system and tested it rigorously under a number of different conditions. Luckily, we pulled through with a crazy amalgamation of a bunch of CV algorithms stringed together basically through trial-and-error.\

In hindsight, the most important change we implemented was how we did our color masking based on a new configuration phase before the actual detection begins. I also had to do a complete rewrite of how the cue stick detection worked. We realized at this point that we would run out of time before we could also add spin detection into the project. Implementing the physics for spin was difficult, and detecting spin was basically impossible. Originally we had planned to use the IMU to detect the [cue stick orientation](https://simple.wikipedia.org/wiki/Pitch,_yaw,_and_roll). However, we realized too late that cheap IMUs are notoriously unreliable, and readings tend to drift after ~5-10 mins of continuous use.

<div class="row justify-content-center">
    <div class="col-sm-auto mt-1 mt-sm-0">
        {% include figure.html path="assets/img/dog-house-burning.jpg" title="rehauling-cv" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Week 13-14: Cleanup & Final Demo\
We basically did nothing but test 24/7 in week 13 under various different conditions. We also had to write our final report and also print out a poster for our demo. We were surprised at how many people came to our table, and it was extremely rewarding to see how many people had fun playing pool with our system. Successful demo!
