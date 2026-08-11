### The Project: **The Zephyr Project**

[The Zephyr Project](https://github.com/zephyrproject-rtos) is arguably one of
the most industry adopted open-source
[RTOS](https://en.wikipedia.org/wiki/Real-time_operating_system) projects out
there, and something I learned recently is that it is actually a Linux
Foundation project. It is also backed by companies such as Intel.

### RFC: Rust Support in Zephyr

https://github.com/zephyrproject-rtos/zephyr/issues/65837

To me, this is the piece of evidence that this is a project worth investing time
and effort into. Tool adoption (in this case that tool is a rust embedded stack)
is like scientific innovation: at first, very slow, when there are few immediate
benefits (or even at an incurred cost), then all at once, once it reaches a
critical mass with great long and short term benefits.

### 12-Week Goal

It is an ambitious goal, but I would like to have contributed to/launched a
project that readily translates zephyr RTOS device trees into a
[Rust BSP](https://esp32.implrust.com/abstraction-layers.html). One of the main
factors in lack of Rust industry adoption is really just the immaturity of the
ecosystem, and this is a great area to work on to improve that aspect.

This project is not my idea, but I do really believe it is a project that _just
makes sense ™_

I will need 3-4 weeks of studying, researching the environment and the rtos it
self, analyzing existing similar tools such as the
[svd2rust](https://github.com/rust-embedded/svd2rust), and have a couple of
useful contributions to zephyr and its rust-adjacents.

Then, I will start working on the actual project, which would require advancing
the existing tools and combining their utilities.

### Risks

The main risk is that I am new to this area, as I am still learning embedded
programming basics, and while I found through research that zephyr is a great
place to contribute to, I am still yet to play around with it and understand the
project deeply. This is mitigated by the fact that this is a very active
community, so I will probably always have someone to reach out to (via a discord
server or a matrix channel etc.).

Scope creep is another real risk here, and it might just be that I bit off more
than I can chew, I will mitigate that by keeping up the small but useful
contributions to the project, so that they act as a safety net in case the main
project is too much.

### Why Me For This Project

I've been working with C/C++ for a good 3 years or so, and I've been learning
Rust recently, a project that is essentially an ecosystem/toolchain migration
seems like it needs someone who understands both ends. One area that I am
definitely lacking in is embedded programming, but I am very eager to learn it
and adopt it as a hobby if not as a career.

### Mentorship

I will mainly need mentorship in the hardware/embedded side of things, I am
currently going through a roadmap for embedded engineering, and it would be nice
to have someone to keep me on track and help me face reality and not waste my
time.
