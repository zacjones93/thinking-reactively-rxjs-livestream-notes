<h1 align="center">Thinking Reactively with RxJS</h1>
<h2 align="center">Rares Matei</h2>

<p align="center"><img src="https://d2eip9sf3oo6c2.cloudfront.net/tags/images/000/000/375/thumb/rxlogo.png" width="200"></p>


## Overview

RxJS is really good at certain problems involving asynchony, especially when multiple 'events' are being called and reference. The consitent tool kit that you have at your disposal makes it easy to read and extensible - harder to mess up.

RxJS needs upfront thought on the design of the code to make it become as extensible and beautiful as possible. In this live stream, Rares' broke a requirement from a pretend manager down into digestible problems that could be solved immediately. 

A key aspect of this problem solving style is identifying the unkowns upfront so you know how to design your solution in a self-contained, isolated fashion. In this case, separating how the UI is represented as well as how background tasks start/end. He assumes that there will be tasks coming in and ending themselves somehow.

Once the problems are broken down into digestible chunks, Rares builds chains of RxJS streams that will end up reading much like the plain english requirements they were broken down into.

---

The notes in this repository are broken down into 3 sections:   
- [Requirements and problem solving](https://github.com/zacjones93/thinking-reactively-rxjs-livestream-notes/blob/master/break-requirements-down.md)
- [Notes taken during the livestream](https://github.com/zacjones93/thinking-reactively-rxjs-livestream-notes/blob/master/notes.md)
- [Viewer questions](https://github.com/zacjones93/thinking-reactively-rxjs-livestream-notes/blob/master/questions.md)

Some question(s) are outlined in the essential questions section of the README for you to think about as you learn this content. There is no 'definitive' answer to these questions but as you learn, you'll develop a more contextualized answer.

Right below is the intended outcomes of the material, these are the skills and knowledge you will learn that you can take to any application to use.

## Essential Questions
    What problems are best solved reactively?
    How can I best extract this problem into a reactive solution?
    How can I connect imperative code with the streams I’m building?
    How should abstractions be named?

## Outcomes:

- Identify a problem
- See why rxjs would be a good solution
- Think about how to solve with RxJS
- Neatly wrap our code so it can be used anywhere
- Learn some operators along the way.
  - combineLatest, timer, delay, scan, takeUntil, skipWhile, distinctUntilChanged, shareReplay, etc.

## Resources
[Code](https://gitlab.com/rarmatei/egghead-reactive-solutions/tree/master/src/lesson-code)

[Example](https://vigilant-visvesvaraya-945613.netlify.com/)

### Pre-requisites

[<p class=""><img src="https://d2eip9sf3oo6c2.cloudfront.net/series/square_covers/000/000/020/square_480/EGH_IntrotoReactive.png" width="100"></p>
Intro to Reactive Programming
](https://egghead.io/courses/introduction-to-reactive-programming)

[<p ><img src="https://d2eip9sf3oo6c2.cloudfront.net/series/square_covers/000/000/034/square_480/EGH_IntroReactive_sq.png" width="100"></p>
RxJS Operators In Depth](https://egghead.io/courses/rxjs-beyond-the-basics-operators-in-depth)


---
### More

During Rares’ live stream about thinking reactively, some members of the chat shared some resources for learning RxJS:

[RxJS Marbles](https://rxmarbles.com/)

[Rx Visualizer](https://rxviz.com/)

[Operator Decision Tree](https://rxjs.dev/operator-decision-tree)


## Contribute
These are community notes that I hope everyone who studies benefits from. If you notice areas that could be improved please feel free to open a PR!