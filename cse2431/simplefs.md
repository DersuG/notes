# SimpleFS
- super simple file system proposed by Ken Thompson

## File system goal
- implements the file interface upon the disk interface

## Implementation overview
- divide disk into blocks (usually 4 KB, same as page size)
- create an inode for each file
- free space management
- directory management
- support for restart/crash recovery
- optimization for disk characteristics

## Popular file systems
- Unix/Linux:
    - ext series (ext2, ext3, ext4)
    - ZFS by SUN
    - etc.
- DOS/Windows: FAT, FAT32, NTFS, etc.
- Mac: HFS, APFS
- Networked File System: NFS, AFS, Google File System, etc.

## Implementation
- disk is divided into blocks
- statically partition disk into data blocks and metadata
    - e.g. first 8 blocks for metadata, remaining 56 blocks for data
- reserve space for inodes in metadata
    - number of blocks affects maximum number of inodes
    - e.g. last 5 blocks of metadata
- bitmaps in metadata to record which blocks are free/used
    - one for managing data region
    - one for managing inodes space
    - 1=used, 0=free
    - e.g. 2nd and 3rd blocks of metadata
- superblock in metadata to track info about the filesystem itself (e.g. file system type number of inodes and data blocks, location of inode tables, etc.)
    - e.g. first block of metadata
![[simplefs.png]]

### Inodes
- each inode has an inode number (ino aka inode id)
- OS finds inodes based on the inode id
- suppose each inode is 256 bytes and the inode table starts at 12 KB and has 5 blocks:
    - how to find an inode based on its id?
        - `inode_address = inode_table_address + inode_id * inode_size`
        - e.g. `12 KB + i * 256`
    - how to find its contents?
        - `inode_contents = inode_address / block_size`
![[simplefs_inode_table.png]]

## Opening files
- `open("/foo/bar", O_RDONLY)`
- find the file:
    - traverse from root
        - find and read inode of `/` (ino is fixed, 2 in UNIX)
    - read children of root
    - find ino of `foo`
- open the file:
    - check permissions (contained in inode)
    - create a file descriptor
        - in per-process open-file table
        - within each process, file descriptors have unique IDs

## Closing files
- release file descriptor

## Writing to files
- overwrite
    - simple
- appending
    - might need to allocate new blocks:
    - find empty block in block bitmap
    - mark as used
    - write block bitmap back to disk
    - update inode to include new block
    - write data

### Updating inodes with new blocks
- inode has direct, indirect, double indirect, and triple indirect pointers
- an additional block must be allocated for each level of indirection
    - +1 for indirect
    - +2 for double indirect
    - +3 for triple indirect

## Creating files
- allocate inode
- initialize inode, update disk block containing inode
- update parent directory (involves opening and writing)

## Optimizations
- each operation needs multiple IO operations
- cache inodes and data blocks

### Caching and buffering
- dynamic partitioning: modern OS uniformly manages file system caches and virtual memory
    - if process requests more pages, OS reduces file system cache size
    - if there's enough memory, OS can increase file system cache size

### Buffering writes
- 2 possible strategies:
    - write-through: don't buffer writes. writes go to disk immediately
    - write-back: first buffer writes in memory and write them to disk later
- write-back is default strategy
- write-back pros:
    - some writes can be merged (e.g. updating an inode multiple times)
    - more opportunity for disk IO scheduling (e.g. elevator's algorithm)
    - can reduce unnecessary writes (e.g. write to file then delete it)
- write-back cons:
    - increase probability of data loss
    - `fsync` forces file system to write data to disk
    - some applications may write to disk directly, skipping the filesystem layer