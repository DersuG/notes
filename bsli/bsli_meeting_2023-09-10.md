# BSLI Meeting 2023-09-10

## Context
- todo:
    - figure out api for FreeRTOS queues
    - should we use queues during interrupts when reading from sensors?
    - how will we structure our data and interfaces?
    - what does each task do in its process loop

## FreeRTOS queues
- receive uart using interrupts, send to queues

## Timeline
- [by 2023-09-10] figure out tasks + queues
- [on 2023-09-13] make project file + repo
    - blocked by: Peter needs to figure out what chip we're using
- [on 2023-09-13] make the tasks + queues
    - do this as a git demo, save some for Akshay to do 2023-09-17
- [on 2023-09-13] figure out apogee detection / main chute deployment algorithms
- [on 2023-09-17] implement apogee detection / main chute deployment algorithms
- [by 2023-10-01] read data from barometer
- [by 2023-10-01] read data from accelerometer
- read data from gps
- sensor control task done
- determine mavlink packets, make the xml definitions
- saving data to sd card

## Conclusion
- ?
