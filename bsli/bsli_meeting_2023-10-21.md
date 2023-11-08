# BSLI Meeting 2023-10-21

## Context
- gotta use a mutex to control access to the `i2c1` peripheral
    - so we don't get true parallelization between sensors using i2c
        - could call all sensor driver functions (initialize/process) sequentially in one task, while still using interrupts
            - no need for mutex
            - could have separate tasks for each peripheral, instead of each sensor
    - this complicates the code quite a lot
        - i am certain it won't work first try
        - i am terrified that i've missed something
        - debugging will be painful
    - would it be possible to use blocking functions this year as a stepping stone towards next year?

## What we did
- new branch for SD card stuff
- removed use of mutex

## Barometer
- barometer needs to be moved farther up the rocket (above payload bay) to prevent pressure changes by the airbrakes
- i2c and spi can probably work over that distance
- if not, we can use converter chips (i2c -> can -> i2c) on one or both ends
    - be careful, the barometer's i2c interface is strange and not quite standard

## Conclusion
- each peripheral will have a task instead of each sensor having one
    - among the sensor drivers for each peripheral, there is no need for mutex because init and process functions are called sequentially
    - simpler and cleaner
- instead of tracking a peripheral's "owner", drivers will temporarily override the interrupt handler when using a peripheral
