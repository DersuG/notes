# BSLI Meeting 2023-10-11

## Context
- we've created the project files and have started working on the drivers for the sensors
    - we've created a collaborative research doc on the onedrive
- we're starting with the ADXL375 accelerometer because [this video](https://www.youtube.com/watch?v=_JQAve05o_0) walks through writing a driver for a similar sensor
- lack of C experience (and lack of autonomy) has been a roadblock
    - teaching them is an ongoing process
    - created https://github.com/osu-bsli/learn-c and had them work through exercises
        - threw them in the deep end to teach them autonomy
- need to start distributing tasks
- need to figure out a better way to manage tasks
- there's like 3-4 active people on the flight computer sub-subteam (working group? task force? idk)
    - not super dedicated but not not dedicated either
    - need to get them into it more

## Task management
- we need to use resilient and open formats like plaintext markdown, not closed, proprietary, and janky stuff like word docs in a onedrive or GitHub-specific tools
- need to assign tasks to specific people
- need sub-tasks
- we can probably just put it right in the git repo
- format: `- [ ] (assignee1+assignee2) task description`
    - e.g. `- [ ] (alice+bob) set up git repo and project files`
    - e.g. `- [x] (morgan) draft readme` (task completed)
    - e.g. `- [ ] fix gitignore` (unassigned)
- need deadlines
    - can put tasks as a sub-list beneath deadlines
    - can append `(by 2023-10-13)` or similar
        - can also do stuff like `(on 2023-10-13)` or `(after 2023-10-13)`
- what to do with completed tasks?
    - deleting them immediately obscures progress
    - keeping them around for too long is messy
    - solution: keep them around for a while, but regularly clean them up
        - `git commit -m 'clean todo.md'`

```md
# Todo List
- setup
  - [x] (alice+bob) (by 2023-10-13) figure out the names of your fictional teammates (see https://en.wikipedia.org/wiki/Alice_and_Bob)
  - [ ] (carol) (by 2023-10-14) outline documentation structure
  - [ ] (dan+eve) (by 2023-10-14) brainstorm architectures, be ready to discuss
- research
  - [x] (faythe) (by 2023-10-14) find and distribute mcu documentation to everyone
  - [ ] (all) (by 2023-10-17) read mcu datasheet
```

### Repo structure
- `./docs/` - documentation goes here
- `./docs/todo.md` - track tasks here
- `./readme.md`

## Conclusion
- need better task management
    - ask other team members for input, become collaborative instead of hierarchical
- barometer, altimeter, and gps models are the only ones set in stone
- altimeter driver just needs to be converted to interrupt logic
- barometer is weird but joey started working on the framework of the driver
- the others are researching on-board SD card storage
    - it'd be nice to have system logs there too, not just telemetry data (e.g. error interrupts, etc.)
