# Code
Notes will be timestamped to reference questions asked during that period of time during the livestream. Each section will be marked with `(xx:xx)` to indicate the time.   

## Initial Requirement - have some background tasks and show a loader
`(01:13)`

we'll assume that they exist at the start

just starting them as empty observables. now that we have them - we can just assume that they exist so we can solve the actual problem for now.

```js
import { Ovservable, merge } from "rxjs";
import { mapTo } from "rxjs/operators";


let showTheLoadingSpinner = new Ovservable()
let loadingStarted = new Ovservable()
let loadingEnded = new Ovservable()

const loadUp = loadingStarted.pipe(mapTo(1));
const loadDown = loadingStarted.pipe(mapTo(-1));
const loadingVariations = merge(loadUp, loadDown);
```

we can just work with `loadingVariations` and not have to deal with any of the set up.
```js
const currentLoadCount = loadingVariations.pipe(
  startWith(0),
  scan((totalCurrentLoads, changeInLoads) => {
    const newLoadCount = totalCurrentLoads + changeInLoads;
  })
)
```

When ever you want to deal with state, you want to use the `scan` operator.

as we're getting 1s, the `newLoadCount` will increase and as tasks complete, we get -1s and `newLoadCount` will go down

> From Ian Jones to All panelists and attendees: (01:20 PM)
This is like reducing an observable stream? Thinking in regular js
scan is kind of like the reduce of an array. scan is more useful because it gives you intermediate values as things change

We can assume that we are now just working on `currentLoadCount` and that's the api that others will use.

we want this api to be as predicable as possible.

We should protect against the number going negative, we cant trust the inputs that we get so 

`return newLoadCount > 0 ? newLoadCount : 0;`

distinctUntilChanged() stops emissions of the same type and only fire the first one (if multiple 0s get emited)

people get scared of the scan operator because it's not shareable. 

If multiple subscribes subscribe to the ``currentLoadCount`, you'll get multiple instances.

Do we want different subscribers to get different counts of background tasks? NO

to protect against this, we'll use `shareReplay(1)` 

This makes the observable above it `hot`

`(01:21)`

```js
const currentLoadCount = loadingVariations.pipe(
	startWith(0),
	scan((totalCurrentLoads, changeInLoads) => {
		const newLoadCount = totalCurrentLoads + changeInLoads;
		return newLoadCount > 0 ? newLoadCount : 0	
	}),
	distinctUntilChanged(),
	shareReplay(1),
)
```

---
now we want to hide the spinner when it goes to 0

    const hideSpinner = currentLoadCount.pipe(filter(counte => count === 0));

when do we want to show it? when it goes from 0 to 1. the 0 to 1 is IMPORTANT. not every time the count is 1.

we need to know the previous value and the current value

could use `scan` for this.

`pairwise` gives you the previous and current only

```js
const showSpinner = currentLoadCount.pipe(
	pairwise(),
	filter(([previous, current]) => precious === 0 && current === 1)
)
```

pairwise signifies intent better. we're just interested in previous and current values and nothing more.

`(01:36)`

```js
showSpinner.pipe(
	switchMap(() => showTheLoadingSpinner.pipe(takeUntil(hideSpinner))))
	.subscribe();
)
```

Now is going to be a good time to test it but we still have the weird set up.

make this as easy as possible to use for anyone even if they aren't familiar with rxjs

create two functions

```js
export function taskStarted() {
	loadedingStart.next()
}

export function taskCompleted() {
	loadingEnded.next()
```
}

how do you go from functions to observables? Subjects!

Subjects are functions but also observables.

We can assume that the loading spinner is coming from a 3rd party api

```js
showSpinner.pipe(
	switchMap(() => showTheLoadingSpinner.pipe(takeUntil(hideSpinner))))
	.subscribe();
)

function showSpinnerOnScreen() {
	const spinnerPromis = initLoadingSpinner();
	spinnerPromise.then(spinner => spinner.show());
	spinnerPromise.then(spinner => spinner.hide());
}
```

`showSpinnerOnScreen` is hard to deal with but we still want to use it.. how do we obstract it away and wrap it in a neat observable.  You can just wrap it in an Observable!

```js
function showSpinnerOnScreen() {
	return new Observable(() => {
		const spinnerPromis = initLoadingSpinner();
		spinnerPromise.then(spinner => spinner.show());
		return () => {
			// disposal logic, observables know how to handle unmounting
			spinnerPromise.then(spinner => spinner.hide());
		}
	}	
}
```

`(01:48)`

## New Requirement
### Add feature: don't flash spinner 

—

**NEW FLOOR**

when does the loader to show?
- when the loader needs to show immediately
  wait 2 secons
  - or cancel if we need to hide it

—
```js
const showDelayed = showSpinner.pipe(
	switchMap(() => timer(2000).pipe(takeUntil(hideSpinner)))

);

showDelayed
.pipe(
	switchMap(() => showTheLoadingSpinner.pipe(takeUntil(hideSpinner))))
	.subscribe();
)
```

we don't need to touch our old implementation but add showDelayed to the beginning of the flow

## New New Requirement - 'almost' quick task
**Create another proxy!**

```js
const thresholdMs = 2000

const showDelayed = showSpinner.pipe(
	switchMap(() => timer(thresholdMs).pipe(takeUntil(hideSpinner)))

);

const hideDelayed = combineLatest(
	hideSpinner,
	timer(thresholdMs)

)
```

combineLatest waits for inner events to fire

```js
const hideDelayed = combineLatest(hideSpinner, timer(thresholdMs))

showDelayed
	.pipe(
		switchMap(() => showTheLoadingSpinner.pipe(takeUntil(hideSpinner))))
		.subscribe();
)
```

`(02:04)`

 ## New Request - key combo to not show spinner at all

create the factory function

```js
export function keyCombo(keyCombo) {
	const comboInitiator = keyCombo[0];
	return keyPressed(comboInitiator)
		.pipe(
			// once your combo has started, ignore anything coming from the pipe - use exhaustMap()
			exhaustMap(() => {
				return anyKeyPresses.pipe(
					// start with + 1 index because we already confirmed that the combo started
					///
					takeWhile((key, index) => keyCombo[index + 1] === key),
					// only take for as long as the key combo length is defined
					// only want to emit at the end, not any time during the process
					// this will skip the first letters and only let the last one go through
					skip(keyCombo.length - 2),
					take(1),
					// take until the time specified then cancel the observable
					takeUntil(timer(5000))
				)
			})
		)
}
```
    
```js
keyCombo(['a', 's', 'd'])
// this is a combo that emits only when the 
// keys are pressed in this order in the array

const stopSpinner = keyCombo(['a', 's', 'd'])

showDelayed
	.pipe(
		switchMap(() => showTheLoadingSpinner.pipe(takeUntil(hideSpinner))),
		takeUntil(stopSpinner())
		)
		.subscribe();
)
```

`(02:20)`