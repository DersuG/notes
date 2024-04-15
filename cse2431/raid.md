# RAID
- redundant arrays of inexpensive disks
- **hardware RAID:** RAID controller is special hardware
- **software RAID:** RAID controller is a special device driver

## RAID-0
- "striping"
- data is distributed across disks in round-robin fashion
- capacity: `N * disksize` (N is number of disks)
- reliability: tolerates 0 failures

![[Pasted image 20231207235320.png]]

## RAID-1
- "mirroring"
- copy data on another disk
- can be combined with RAID-0
    - RAID-10: mirror within groups, stripe across groups
    - RAID-01: stripe within groups, mirror across groups
- capacity: `N/2 * disksize`
- reliability: tolerates 1 failure, and up to N/2 failed disks

![[Pasted image 20231207235618.png]]

## RAID-4
- parity bits
- parity disk is bottleneck

![[Pasted image 20231207235952.png]]

## RAID-5
- distribute parity blocks across all disks
![[Pasted image 20231208000123.png]]

## Performance
![[Pasted image 20231208000248.png]]