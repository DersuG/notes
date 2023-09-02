# BSLI Meeting 2023-08-30

## Context
- second Avionics meeting of the year
- got some new members
- we have a big team, like 26 (?) people total!
    - there are a few people (2-3?) willing to work on each part of the system (boards, flight computer, airbrakes, etc.)
    - limited experience (haven't taken Systems 1 or 2, don't know C super well)
- we are very likely going to use FreeRTOS multitasking for the flight computer
    - we need to decide how to split the problem into tasks
    - we need to decide how data will be sent between tasks
    - we need to decide what data will be sent between tasks

## Stuff we talked about
- flight computer architecture
- reading lots of documentation
- STM32 features (configuring through IDE, peripherals, DMA)
- communicating with STM32 (polling vs. interrupt vs. DMA)
- STM32CubeIDE
- 
## Conclusion
- proposed architecture:
    - data is mostly passed via FreeRTOS queue primitives
        - this duplicates data, but the convenience is probably worth it
        - this avoids having to debug complex concurrency problems
    - **sensor control task:** reads from sensors, sends data to other tasks
    - **telemetry control task:** sends data to radio, also does arming/disarming
    - **recovery control task:** does recovery stuff
        - internally keeps track of flight plan/stage
    - **airbrake control task:** does airbrake stuff
        - internally keeps track of flight plan/stage (only a subset of how the recovery control task does it)
- next meeting:
    - have an interactive problem for members to solve, can be a simplified version of a real problem
        - reading from UART?
        - packet format?
        - 
