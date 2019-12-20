# Questions
`highlighted is learner questions`

**Bold is on stream answer**

> quoted is good observations or feedback

## Initial requirement
### have some background tasks and show a loader

I appreciate the way it is being laid out, because it helps you to understand the thought process that will ultimately go into the code

From Itay Roisman to All panelists: (01:14 PM)  
this a great approach to break down the process with comments

From Itay Roisman to All panelists: (01:21 PM) 

`Are there any more operators like scan which keeps the state of the stream?`

From Ian Jones to All panelists and attendees: (01:25 PM) 

`Is this because a new observable is created when someone subscribes?`

From Edwin Meijne to All panelists and attendees: (01:25 PM)  No the same observable is returned 
https://rxmarbles.com/  Good resource to keep next to this session

**the way observables work is that they are lazy. by default, they start up every time someone subscribes to them.**

**shareReplay makes it so that if 100 people subscribe to an interval, they wouldn't each get their own interval but get the same interval from the start**

From Ian Jones to All panelists and attendees: (01:28 PM)  I bet this is good for further reading :P https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339

From Adam Barrett to All panelists and attendees: (01:28 PM)  https://rxviz.com/ is nice too

`Why shareReplay(1) instead of the share() operator?`

**with the share() operator. if the observable emits a #2 and then someone else subscribes - the new subscriber would have to wait until the next emition to get the state.**
**shareReplay will imediately share the value that it is storing and then also share future values.**

From Edwin Meijne to All panelists and attendees: (01:30 PM)   https://www.learnrxjs.io/operators/multicasting/sharereplay.html


From Guillermo Timón to All panelists and attendees: (01:36 PM)  couldn’t just use some sort of combinatio sprry combination with the distinct change and a map too*

> From Edwin Meijne to All panelists and attendees: (01:37 PM)  I often use this one to help me decide: https://rxjs.dev/operator-decision-tree

build your own adventure with operators

> From Itay Roisman to All panelists: (01:38 PM)  pairwise is a good example for another operator which share previous state

From Me to All panelists and attendees: (01:48 PM)  That’s a question I have, `you know what you’re doing with building these observables up from scratch. But if I were to solve a problem with this today, how would I do iteratively before ‘hooking it up to the real thing’?`

**worry about the unkowns first and make sure they are real. handle the connection first and work backwards from there. go up that tree backwards**

From Rajesh Vishwakarma to All panelists and attendees: (01:49 PM)  `what about unsubscribe()?`


From Guillermo Timón to All panelists and attendees: (01:50 PM) `how can you call .then twice and have different results? you create the promise`

From Rajesh Vishwakarma to All panelists and attendees: (01:52 PM)  observable must be unsubscribe if subscribe before. it might be will leave to memory leak if we don't unsubscribe

---
## New Requirement - Add feature: don't flash spinner 

- No questions

## New New Requirement - 'almost' quick task

From Artem Kalikin to All panelists and attendees: (02:04 PM)  `So you don't really need showDelayed here, hideDelayed should satisfy requirements`

From Ian Jones to All panelists and attendees: (02:05 PM)  This is wild

From Artem Kalikin to All panelists and attendees: (02:06 PM)
 `It shouldn't look glitchy anymore for a quick task that  finishes immediatly, becuase it'll wait 2 secs after completion? Or maybe I'm misunderstanding`

**if you use showSpinner it will show for 2 seconds where with the current implementation just hides the whole thing for 2 seconds.**

From Guillermo Timón to All panelists and attendees: (02:20 PM) `why skip is in second position?`
From Guillaume Sallaber to All panelists and attendees: (02:20 PM)
 `is the order important here? I mean the skip and the take before the takeUntil?`

From Rajesh Vishwakarma to All panelists and attendees: (02:20 PM) ` how we can make React props as observable stream?` don't wanna use recompose library.

From Itay Roisman to All panelists: (02:20 PM)  you assume that the order of key press is in the same order of the array i might start with “f” first and you’ll dispose the observable

From Guillermo Timón to All panelists and attendees: (02:21 PM) `could you go through why you do length - 2`, it is because you get one out in the begining and other to just get the last one beginning*

From Ian Jones to All panelists and attendees: (02:22 PM) How does takeUntil(timer(5000)) `fire if the opteration above it will end the observable if it gets called?`

From Guillermo Timón to All panelists and attendees: (02:24 PM) Thanks, now makes sense

From Viktor Soroka to All panelists: (02:25 PM)  Please one more time about exhaustMap thing.

**There are many ways you can merge streams. exhaustMap is a less used operator. We don't want to trigger this observable eevery time one of the special keys is pressed. But want to silence the triggering while we are inside the observable. It makes it so you only have one process being triggered at a time.**

From Itay Roisman to All panelists: (02:29 PM) ` is it possible to restart a stream after take until returned false?`

**yes. There's an operator that called repeat so anytime the parent observer completes, it calls the parent observable**