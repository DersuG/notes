# BSLI Meeting 2023-09-06

## Context
- we have a good plan for the flight computer architecture
- still need to decide:
    - how arming/disarming will work
    - how saving sensor data to an SD card will work
    - how the camera will work
    - how to do unit testing

## Conventions

### Code conventions
- prefixing things with `fc_`
- try to keep code outside of generated files
- every function should be documented
- NASA's Power of 10
    - use `assert()`
    - keep it simple, this isn't code golf
    - avoid macros/preprocessor
    - hard upper limit on loops
- 

### Git/GitHub workflow
- feature branches
- pull requests and code reviews
- branch protections
- many small commits > one big commit
- extended commit messages
- we might want to try moving to a fork-and-pr workflow instead of a branch-and-pr workflow
    - then we can set the default repo permissions to "read" instead of "write"

### STM32CubeIDE use
- don't change project settings without documenting what you do
    - check `git status` to make sure you didn't accidentally change something
