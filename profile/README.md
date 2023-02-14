# There is no TODO

We make scheduling easier for those demanding
**maximum predictability** of real life.

## Philosophy

- Your memory is randomly cleared.
- You can forget to schedule.
- Always avoid remembering pending objectives.

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

## Contribution

You can create or fix issues in each repo,
or refer to [Roadmap](https://github.com/orgs/there-is-no-todo/projects/1/views/1).

### Language

We would use Rust for everything except Flutter Interface,
because it's enum and pattern matching feature makes life a lot easier.

### How Schedules are Stored

Schedules are stored as json.
They should be immutable, user can only create or delete them.

Different schedule correspond to different choice of Enum in Rust.
As there will be more and more types of schedule,
we'll have to change the definition of schedule Enum,
so it violates the Open-Closed Principle.
If you have better solution, sharing is welcomed.

## Fun Facts

- Inspired by _There is No Game_
- **T**here **i**s **N**o **T**ODO -> TiNT
- Optimally one only need to upload key information, and computer can generate a schedule!
- Looking from MBTI, TiNT is a tool that facilitates Ni-Te cycle (INTJ).
