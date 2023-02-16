# There is no TODO

We make scheduling easier for those demanding
**maximum predictability** of real life.

## Getting Started

Have you ever

- dreamed for a todo CLI that syncs schedule with your phone and Polybar?
- been assigned to a faraway meeting at 14:15,
  having trouble to calculate when exactly you should stop playing games?
- wanted to take the exam optimally at 13:45, but no later than 14:15?
- planned to do something when you pass by place A, but later forgot?

You can try TiNT!

TiNT is a multi-platform TODO tool with more expressive TODO syntax.
You can easily associate event with place,
and dissolve uncertainty in real life with periodic representation of time.

## Design Philosophy

Reduce brain usage for real-life things as much as possible.

## Blueprint

There will be a lot of components, but they can viewed as:

- Mapping between CLI and json
- Mapping between Polybar program and json
- Mapping between Flutter interface and json
- Mapping between json and persistent data

Typical components are:

### Command Line Tool

Receives command with high customizability,
and upload to user-defined server.

For example, `todo dinner 17:15`
uploads a `dinner` schedule that begins at `17:15`.

### Polybar Program

Retrieves schedule from server,
and displays next schedule through Polybar.

### Flutter Interface

Retrieves schedule and status from server,
and emits notification with generated voice.
It should also contain a wrapper for CLI.

### Server

Contains API to upload and download schedule JSON,
and computes status of schedules.

## Prototype Specifics

In the prototype, we plan to make data structures clearer:
There will be two and only two datatypes, `Map` and `Plan`.

### Map

`Map` is essentially a weighed graph.

- Edge: `(Int, Int)` = minimum and maximum time from one place to another
- Vertex: `Null` = place
- AlarmMode: `Vibrate | Loud` = method for alarm that is suitable and can 100% be heard.

### Plan

`Plan` typically associates with a vertex and a specific time, but it's polymorphic.

- Time: `(Time | Null, Time | Null)` = Minimum and maximum start time
- Duration: `(Time | Null, Time)` = Minimum and maximum duration that you're forced to not leave
  - Silence mode will be automatically handled by Flutter Interface when you're in duration
  - Conflicts in plans should be detected
- Place: `[Int] | Null` = Specific places or all places

`Plan` can have very dynamic behaviour.
In the following examples, the event defaults at place B,
and you're at place A. When going to C, you'll first go through B.
AB takes 10 ~ 20 minutes, while BC takes 1 ~ 2 minutes.

- If you have a meeting at 14:00, then the alarm will start at 13:40
- If there's an exam, you want to get there before 13:45, but no later than 14:15,
  then the alarm will start at 13:25 if the phone is not silenced, and forcefully start at 13:55
- If there will be a package at B after 16:00, taking the package takes 2 ~ 5 minutes,
  and you have a meeting at C at 17:00, then alarm will rise at
  - 16:33 (Start from A to B, because 20 + 5 + 2 = 27)
  - 16:43 (Get the package, because from A to B takes at least 10 minutes)
  - 16:58 (Start from B to C, because from B to C takes at most 2 minutes)

## Contribution

You can create or fix issues in each repo,
or refer to [Roadmap](https://github.com/orgs/there-is-no-todo/projects/1/views/1).

### Stack

For the prototype, the polybar program and CLI tool would be in python,
and the server will use FastAPI and MongoDB.
Of course, Flutter will be included.

For the real app, I would like to use Haskell or Idris for extra modularity and expandability.
However, there will likely be no real app for this...

## Fun Facts

- Inspired by _There is No Game_
- **T**here **i**s **N**o **T**ODO -> TiNT
- Optimally one only need to upload key information, and computer can generate a schedule!
- Looking from MBTI, TiNT is a tool that facilitates Ni-Te cycle (INTJ).
