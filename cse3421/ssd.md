# Solid State Device Storage
- same interface as HDD
- way faster than HDD
- internal structure differs from HDD

## Giant magneto-resistance (GMR)
- discovered independently in 1988 by Peter Gruenberg and Albert Fert
- huge resistance changes in materials made of alternating very thin (nanometer) layers when exposed to magnetic fields
- foundation of increasing HDD density
- 1997: first GMR-based commercial HDD by IBM, with 16 GB capacity
- 2007: 1+ TB HDD become available on market
- next generation fast and high-density memory: magneto-resistive RAM

## HDD improvement has focused on density
- compact HDDs with huge capacity are available for cheap
- HDDs are still slow
- high capacity causes high access latency
- power hungry
- sensitive to shocks (don't drop them)

## Flash memory based solid state drive
- **solid state drive (SSD):** no mechanical components
- low latency (e.g. 75 us)
- high bandwidth (e.g. 250 MB/s)
- low power consumption (e.g. 0.07 W idle, 2.4 W active)
- shock resistance
- lifespan: 100 GB/day → ~5 years

## Floating-gate transistor
- data is stored permanently in the floating gate
- large voltage difference between source and drain maintains a level of electrons in the floating gate
- level of electrons represent the state of the cell
![[ssd_floating_gate_transistor.png]]
- **single level cell (SLC):** 2 voltage states (1 bit per cell)
    - low density, high access speed, high endurance, low power, high cost
- **multi-level cell (MLC):** 4 voltage states (2 bits per cell)
    - high density, lower access speed, lower endurance (10x), increased power, low cost
- **triple level cell (TLC):** 8 voltage states (3 bits per cell)
    - enhanced MLC

## Hard disk controller
![[Pasted image 20231116101018.png]]
- all inbound and outbound data is filtered by **error correction code (ECC)**
- buffer memory is used for locality
- **disk drive CPU:** finds disk locations of logic blocks and sends control signals
- **code memory & variable store:** memory space for disk cpu
- **servo control & demodulator:** gets signals from disk cpu to control disk motor

## SSD limitations
- expensive
    - Gordon HPC Cluster is a full-SSD supercomputer, costs ~$5,000,000/year with only a 3-4 year lifespan
- unpredictable performance
    - different data allocations among flash chips result in different latencies
- hard to use full potential performance
    - requires extensive effort
    - reliability
        - **program-erase cycles (P/E cycles):**
            - most commercial flash products are rated to withstand ~100,000 P/E cycles
            - in reality, the cycles decrease for low end SSD products to save on cost
            - **wear leveraging:** dynamically remapping blocks to spread write operations between sectors
        - **read disturb errors:** causes other nearby cells to change over time if they aren't rewritten
        - **limited capacity:** tradeoff between large capacity and reliability
## SSD architecture
![[ssd_architecture.png]]

## SSD write and erase
- data write:
    - data can't be overwritten (must erase before writing)
    - write in units of **pages** (4-8 K)
- data erase:
    - erase in units of **blocks** (64-128 pages)

## SSD garbage collection
- **stale pages:** no longer needed
    - SSD will copy valid pages to a free block, then erase the original (thereby erasing the stale pages)
- expensive
- can be done as a background process
![[ssd_gc_1.png]]
![[ssd_gc_2.png]]
![[ssd_gc_3.png]]

## Memory units
- *cache block size:* 4-32+ bytes
- *memory (DRAM) page size:* 4-8 KB
- memory and cache share the same memory address
    - points to:
        - a byte in a page in a segment in a memory bank
        - a block in a direct-mapped, set-associative, or fully-associative cache
        - a cache region, if the cache is partitioned by pages
- HDD location is independent of memory address
    - sector
    - track
- *HDD block size:* 512 bytes (a sector on a track)
- when reading/writing from HDD, a virtual or physical page is broken into multiple *512 byte **logical blocks (LB)***
- HDD disk controller uses a **directory** to map filenames and LBs to PBs (physical sectors)
- SSD location is independent of memory address
    - page
    - block
    - domain
- *SSD block size:* 64-128 pages (1 page = 2 KB)
- when reading/writing from SSD, a virtual or physical page is written to a free page in a block
- SSD controller (firmware, **flash translation layer (FTL)**) uses a **mapping table** for the file system to map filenames and page numbers to SSD locations
- cache block ≠ logical block ≠ SSD block

## Write amplification
- measurement
- `write_amplification = actual_data_written / requested_data_written`
- worst case:
    - writing a 4 K page to a 512 K block, where only 1 page is invalid
    - garbage collection writes 127 valid pages to a free block, then erases the entire block before writing the 4 K page to the new block
    - write amplification = 512 K / 4 K = 128

## Wear leveling
1. upon an update request, mark the page as invalid
2. fetch an empty block from the free pool
3. update the page in the empty block and give the block to the used pool
![[ssd_wear_leveling.png]]

## Overview of SSD writes
- over-provisioning provides a sufficiently large free pool
- iteratively write to blocks in free pool to achieve wear leveling
- minimize garbage-collection operations
![[ssd_write.png]]

## Over-provisioning
- free block pool
- data block pool
    - this is the user capacity
    - if data block pool = free block pool, no over-provisioning
    - the difference is the over-provisioning amount
- reduces the number of garbage collections
    - reduces write amplification

## SSD as block device
- block size
    - based on logical block size
    - VM: 4-8 KB, database: 8 KB, buffer cache: 1 KB
- different from SSD block
    - a flash memory block contains 64-128 flash pages
    - flash page is the basic SSD unit
    - flash pages in blocks are ordered by numbered
    - LBNs are written/read sequentially inside an SSD block
- HDD vs SSD
    - a logic block is stored in multiple 512-byte sectors in HDD
    - it will be in half, one, or multiple flash pages in SSD

## I/O interface for storage devices
- serial ATA (SATA)
- eSATA
- small computer system interface (SCSI)
- peripheral component interconnect express (PCIe)

## Hystor
- Intel prototype
- hybrid storage system combining SSD and HDD
- SSD stores:
    - semantically critical data (e.g. OS code)
    - performance critical data
- HDD stores:
    - low priority data

## SSD architecture overview
![[ssd_architecture_overview.png]]