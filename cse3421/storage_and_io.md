# Storage and Other I/O Stuff

## Disk
- controlled by OS via **disk controller**
- DRAM interacts with disk with help from OS
- partitioned into 3 regions:
    - user file system areas
    - OS swapping space
    - system administration files (for OS kernels)

## Page faults trigger disk accesses

## Disk electronics
- 6 chips in HDD:
    - R/W channel
    - processor
    - power
    - on-board DRAM
    - control ASIC
    - motor/spindle control

## Disk latency
- mostly from mechanical operations
- seek (57%)
- rotation (27%)
- data fetch (7%)
- other overhead (6%)
- data transfer via SCSI bus is only 3%

## Disk structure
- disks contain a **cylinder**, which contains **platters**, each with 2 **surfaces**
    - multiple platters per cylinder enables parallel data access
- each surface is organized into concentric rings called **tracks**
- each track contains **sectors** separated by gaps
![[hdd_sectors.png]]

## Disk access
- cylinder must be rotated so that the target sector passes under the head
- `access_time = avg_seek_time + avg_rotation_time + avg_transfer_time`
    - `avg_seek_time`: time to position head in target track (3-5 ms)
    - `avg_rotation_time`: time for target sector to pass under head (best case current sector, worst case whole rotation, average case 1/2 rotation)
        - `avg_rotation_time = 1/2 * 1/rpm * 60sec/1min`
    - `avg_transfer_time`: time to read bits in target sector (scan entire sector)
        - `avg_transfer_time = whole_track_rotation_time / avg_num_sectors_per_track`

## OS-disk interaction
- OS is memory-address centric
- disk has its own address space
    - basic unit of data is a sector (512 bytes)
- **block device:** allows OS and disk to communicate to each other by "blocks"
- disk is not directly managed by OS
- disk controller manages disk data
    - OS interacts with disk controller for I/O
    - disk controller takes logical block numbers from OS to find or write them
    - logical blocks managed by OS are read from disk and physically stored there by disk controller
    - logical block addresses (LBAs) are statically mapped to physical block addresses (PBAs)

## Block devices
- disk storage can be represented as an array of blocks
- logical block size is determined at boot by OS
    - OS virtual memory system: 4-8 KB, buffer cache: 1 KB
    - database block size: 8 KB
- HDD sector size is 512 bytes
    - a 4 KB logical block would be allocated in 8 contiguous sectors
- **block device:** a storage device that reads/writes a block of fixed size at a time
- disk interface standards:
    - **SCSI:** small computer system interface
    - **IDE:** integrated drive electronics
    - **ATA:** AT bus attachment

## Memory resident page table
- logical block numbers in OS (virtual page numbers) are mapped to disk locations for pages that aren't resident in main memory

## Disk capacity
- **capacity:** # bits that can be stored
- determined by:
    - **recording density:** (bits/in) # bits in a 1 inch linear segment of a track
    - **track density:** (tracks/in) # tracks in a 1 inch radial segment
    - **areal density:** (bits/in^2) recording density times track density
- `capacity = bytes_per_sector * avg_num_sectors_per_track * num_tracks_per_surface * num_surfaces_per_platter * num_platters_per_disk`

## Reading from disk
1. CPU starts a disk read by writing a `READ` command, with the logical block number, number of blocks, and destination address to a port (address) associated with the disk controller
2. disk controller reads sectors and performs a direct memory access (DMA) transfer into main memory
3. when DMA transfer completes, the disk controller notifies the CPU with an interrupt

## Physical memory pages vs. disk sectors
- id of memory pages: memory address + process id
- id of disk sectors: starting LBA + file id
- memory pages are mapped to disk sectors by a table in the disk controller

## File system
- organizes files at the logical block level
- interacts with disk controller to retrieve and store:
    - the mapping between logical files and physical storage
    - the files themselves

## Contiguous allocation
- logical blocks are contiguously allocated sector by sector under a file name, user id, starting sector number, and length
- memory address is irrelevant to disk storage allocation
- pros:
    - good for sequential read accesses (e.g. database range queries)
- cons:
    - bad for random access
    - bad space efficiency (e.g. frequently updated files generate multiple versions in time stamps, which may not be contiguously allocated)
    - hard to grow files
        - **extent:** reserved contiguous space in disk, and a file may consist of one or more extents

![[file_system_contiguous_allocation.png]]

## Disk allocation in file system can be grouped
- one pointer for all related sectors
- additional support needed in disk controller
- pros:
    - high space utilization (easy to grow/shrink files, directories are simple)
- cons:
    - index location can be bottleneck
        - hard to support files with many blocks
        - multiple level index locations are used
        - management of index location is complex
![[file_system_grouped_allocation.png]]

## Disk allocation in file system can be linked
- linked-list is formed for all related sectors
- additional support needed in disk controller
- see slide 65
- pros:
    - high space utilization (easy to grow/shrink files, directories are simple)
- cons:
    - block access time is random
        - seek times cause high latency
        - any file access must start from the link head
- File Allocation Table (FAT) implementation creates a table for linked allocation
![[file_system_linked_allocation.png]]

## File allocation table (FAT)

![[file_system_fat.png]]

## Disk allocation in file system is in inodes
- **index node (inode):** aka information node collects all related information for a data entity (multiple files) based on ownership, access rights, etc.
- inodes are stored in a special region on the disk and loaded (cached) into memory when used
![[file_system_inode.png]]

## Buffer cache
- special reserved memory region
- caches copies of disk blocks (e.g. files, inodes)
- serves as write-back buffer (for modified data)
- managed by OS
    - linux has 1KB kernel block size
- replacement is used all the time (e.g. LRU, LIRS, Clock, Clock-pro)
- **prefetching:** OS predicts which blocks of a file to cache after reading part of the file
![[buffer_cache.png]]

## Demand paging vs. CPU caching vs. buffer caching
- demand paging:
    - OS/MMU creates page tables for each process, TLB for MRU page table entries
    - lifecycle: program execution
- cpu caching:
    - during execution, MRU pages are stored in on-chip caches, or in memory, and LRU pages are evicted
    - lifecycle: program execution
- buffer caching:
    - a memory cache for data copies between filesystem/DB and disks
    - lifecycle: buffer cache space dependent
