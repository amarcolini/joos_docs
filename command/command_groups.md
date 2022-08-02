---
order: 89
icon: container
---

# Command Groups

Command groups join multiple commands into one single command. Depending on how they join commands, command groups can also require their commands to have different requirements, like, for example, when they are run in parallel and cannot use the same components at the same time.

All the current types of command groups are outlined below:

Commmand Group | Description
:---: | :---
`SequentialCommand` | Runs commands in sequence, one after the other.
`ParallelCommand` | Runs commands in parallel until they all finish.
`RaceCommand` | Runs commands in parallel until only one of them finishes.
