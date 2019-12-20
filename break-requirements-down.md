show a loading spinner when async work is being done in the background

when you write in rxjs — you end up with a declaritive stream of meaty rxjs operators.

Sometimes it can flow like an english sentance

We have our requirement in english - then lets try to problem solve by speaking in these sentances

---
## Initial requirement - have some background tasks and show a loader

What we want at a high level: 

when the loader needs to show
 - show the loader
    - until it's time to hide

—

lets break that down:
- when does the loader to show?
  - when the count of bgr tasks goes from 0 to 1

-

- when does the loader need to hide?
  - when the count goest to 0

—

how do we count?
- start from zero
- when a tasks starts, inc. the count by 1
- when a task ends, dec. the count by 1

We're now at a lower plane of requirements. We're trying to simplify so we can start on the problem right away.

—

unkowns:
- show the loader
- when a task starts - don't know what will tell us
- when a task ends

---

## New Requirement - Add feature: don't flash spinner 

we have quick tasks that only last for 300ms. Users don't like the spinner showing up and hiding so fast. Don't show the spinner unless tasks work for 2 seconds or more

Initial requirement: have some background tasks and show a loader

---
What we want at a high level: when the loader needs to show
 - show the loader
    - until it's time to hide

—

**NEW FLOOR**

when does the loader to show?
- when the loader needs to show immediately
  wait 2 secons
  - or cancel if we need to hide it

—

lets break that down:

- when does the loader to show immediately?
  - when the count of bgr tasks goes from 0 to 1

—

- when does the loader need to hide?
  - when the count goest to 0

—

how do we count?
 - start from zero
 - when a tasks starts, inc. the count by 1
 - when a task ends, dec. the count by 1

We're now at a lower plane of requirements. We're trying to simplify so we can start on the problem right away.

—

unkowns:
 - show the loader
 - when a task starts - don't know what will tell us
 - when a task ends

 ---

## New New Requirement - 'almost' quick task
what happens with an 'almost' quick task? we're back to square one with the loader flashing on the screen?


Initial requirement: have some background tasks and show a loader

---
What we want at a high level: when the loader needs to show
 - show the loader
    - until it's time to hide

—

**NEW FLOOR**

when does the loader to show?
- when the loader needs to show immediately
  wait 2 secons
  - or cancel if we need to hide it

—
**NEW NEW FLOOR**

when does the loader need to hide?
- when 2 things happen:
  - 2 seconds have passed since showing it
  - and the spinner needs hide immediately

—

lets break that down:

- when does the loader to show immediately?
  - when the count of bgr tasks goes from 0 to 1

—

- when does the loader need to hide?
  - when the count goest to 0

—

how do we count?
 - start from zero
 - when a tasks starts, inc. the count by 1
 - when a task ends, dec. the count by 1

We're now at a lower plane of requirements. We're trying to simplify so we can start on the problem right away.

—

unkowns:
 - show the loader
 - when a task starts - don't know what will tell us
 - when a task ends

 ---

 ## New Request - key combo to not show spinner at all
 —

When the combo starts
 - keep listening for combo keys
    - while the keys follow the combo
    - or until the combo is finished
    - or until the timer is expired

—