# Fast File System

## SimpleFS problems
- doesn't take into account that random disk access is much slower than sequential disk access
    - inodes should be close to the data
    - fragmented data is slow
- file-systems should be "disk aware"

## FFS basic idea
- locality is important
    - in many subsystems, not just file systems
- blocks are organized into block groups
    - each group contains some consecutive blocks
    - accessing blocks from the same group is fast
    - each block group is its own SimpleFS
- why?
    - inodes and corresponding data blocks are usually accessed together
    - files in the same directory are often accessed together

## FFS allocation policies
- for new directories:
    - find a group with a low number of directories and a high number of free inodes
    - files in the same directory can be put into the same group
- for new files:
    - place data blocks and inode of the file in the same block group
    - place all files in the same directory in the same block group with the directory's inode

## Exceptions for large files
- large files could fill the entire block group
- prevents subsequent related files from being in the same group
- solution: place a certain number of the file's blocks in the first block group, then place a large chunk of the file in another block group, and repeat
- scattering data hurts performance, but if chunk size is large enough overhead isn't high
    - seek and rotation time becomes low relative to transfer time

## Optimizations for small files
- many files are 2 KB or smaller
- if using 4 KB blocks, internal fragmentation occurs
- solution: sub-blocks (512 bytes)
    - multiple small files may occupy sub-blocks in the same block

## Other optimizations
- support for long file names
- support for symbolic links
- atomic renames