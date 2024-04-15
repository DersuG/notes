# Disks

## Types
- HDD
    - IDE/ATA
        - cheaper
        - lower performance
    - SCSI
        - high performance
        - expensive
- floppy disk
- optical disk (DVD, CD, etc.)
- SSD

## HDD
- block size affects % of disk transfer bandwith you get
    - 1 KB = 0.5%
    - 2 MB = 90%

### Unaligned I/O
- **read-modify-write procedure:** unaligned writes (e.g. 1.5 sectors), must read all sectors first, modify, then write them back

## Disk scheduling
- first come first serve FCFS
- shortest seek time first (SSTF)
- elevator (SCAN)
- circular SCAN (C-SCAN)
- add up the differences between the points on the seek graph

### FCFS
- first come first serve
- pros:
    - fairness among requests
- cons:
    * long seeks
    * wild swings can happen

### SSTF
- pick the one closest to the current position
- pros:
    - minimizes seek time
- cons:
    - starvation

### SCAN
- elevator
- take the closest request in the direction of travel
- change direction at the end
- pros:
    - bounded time for each request
- cons:
    - request at other end will take a while
    - favors requests in the center
- **LOOK algorithm:**
    - don't go to the end
    - service the last request, then change direction

### C-SCAN
- like SCAN but wrap around (only move in 1 direction except when wrapping around)
- pros:
    - uniform service time
- cons:
    - nothing gets done on the return
- **C-LOOK algorithm:**
    - don't go to end
    - service the last request, then wrap around
- **skip wrap-around time on the graphs**