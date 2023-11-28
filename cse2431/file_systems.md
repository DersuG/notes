# File Systems
- provided by OS
- implements the abstraction of a file tree on top of the disk abstraction

## File
- **inode:** indexed node
    - stores all metadata about a file (e.g. owner, creation time, disk sectors, etc.)
    - **inode number:** unique number, each inode has one
- each file has an inode
- can have 1 or more human-readable names
    - OS maps these names to inodes
- nowadays, file extensions are just conventions (OS doesn't interpret file contents)

## Directory
- (in Linux)
- directories are also files
    - content specified by the file system itself
    - contains a map of `<name, inode_number>`
        - **dentry:** an entry in this map ("directory entry")
- also has a unique inode
- special directories:
    - `.` (current directory)
    - `..` (parent directory)
    - `/` (root directory)

## File system interface
- create
- read/write
- seek
- Fsync - flushes data, writes to disk
- rename
- get info
- remove
- make directory
- read directory
- delete directory
- hard and soft/symbolic links
- make and mount filesystem

### Creating files
- system call: `int open (char *name, int mode, int permissions)`
    - returns the file descriptor
    - e.g. `int fd = open ("foo", O_CREAT | O_WRONLY | O_TRUNC, S_IRUSR | S_IWUSR);`

### Reading/writing files
- system calls: `int read (int fd, char *data, int size)` and `int write (fd, char *data, int size)`
    - `fd` is file descriptor
- files are often big, so can't handle whole thing at once, must buffer
```c
fd = open (...)
char *buffer = malloc (size)
while (read (fd, buffer, size) != 0)
{
    /* handle buffer contents */
}
```

### Seek
- starting point depends on which mode the file was opened in (e.g. truncate mode vs. append mode)
- system call: `off_t lseek (int fd, off_t offset, int whence)`
- disk seek may be deferred until you actually read/write

### Fsync
- flushes system buffers and actually writes your data to disk

### Renaming files
- OS guarantees that renames are atomic
    - if system crashes, filename will either be old name or new name
    - useful for atomically updating files (e.g. when saving files, applications usually don't write to the original file directly, but rather write to a new file and rename it)
- system call: `rename (char *old, char *new)`

### Getting file info
- system calls: `stat` and `fstat`

### Removing files
- system call: `unlink`

### Making directories
- system call: `mkdir`
- contains the special directories `.` and `..`

### Reading directories
- system calls: `opendir`, `readdir`, and `closedir`
```c
int main (int argc, char **argv)
{
    DIR *dp = opendir (".");
    assert (dp != NULL);
    struct dirent *d;
    while ((d = readdir (dp)) != NULL)
    {
        printf ("%d\t%s\n", (int) d->d_ino, d->d_name);
    }
    closedir (dp);
    return 0;
}
```

### Deleting directories
- system call: `rmdir`

### Hard links
- physical files can have multiple path names
- when you create a file, it has one path name
- multiple dentries can map to the same inode
- OS maintains a counter for each inode to record the number of hard links
- can't link directories
- can't link to other disk partitions or file systems
    - if the counter drops to 0, the OS may remove the inode
- modifying one modifies all others (they are the same file with the same inode)
- command: `ln [FILE] [NAME]`
- system call: `link`

### Symbolic/soft links
- can link to directories
- can link to other disk partitions or file systems
- is a completely new file with a different inode
- modifying the file contents will modify all others
- deleting the link preserves the original
- deleting the original causes the link to become a dangling reference
- don't increase an inode's reference counter
- command: `ln -s [FILE] [NAME]`

### Making and mounting a file system
- commands: `mkfs.{fsname} [DEVICE]` and `mount -t [TYPE] [DEVICE] [LOCATION]`
    - e.g. `mkfs.ext4 /dev/sda1`
    - e.g. `mount -t ext3 /dev/sda1 /home/users`
    - `mkfs` can format a whole disk or a partition
    - list all mount points with `mount` (no args)
- `sda` means first SCSI or SATA disk
    - S means SCSI or SATA
    - D means disk
    - A means first
- each partition acts like a smaller disk
    - can partition with `fdisk` command

## Inode info
![[fs_inode_info.png]]

## Mapping virtual addresses to physical addresses
-  each file has a separate virtual space from 0 to MAX
- divide virtual space and physical space into virtual and physical blocks
- inodes store info to convert virtual block number to physical block number
- procedure: using VA, compute VBN and offset, map VBN to PBN, concatenate PBN with offset
- inode stores an array of block pointers for such mapping
    - **direct pointers:** these pointers
    - `blocks[i]` stores the physical block number of virtual block `i`
    - essentially the same as a single-level page table
    - number of direct pointers limits max file size
        - use multi-level indexes to bypass this limit
- **2-level index:** let some pointers (**indirect pointers**) point to data blocks of more pointers
- **3-level index:** some direct, indirect, and **double-indirect pointers**
- most files are small, so 12 direct pointers are usually enough
    - having all pointers be indirect would be slow

### Max file size with multi-level index
- 2-level index with 12 direct and 1 indirect pointer:
    - `num_pointers_per_block = block_size / pointer_size`
    - `max_filesize = block_size(12 + 1(num_pointers_per_block))`

### Index structure
- if block number is in `[0, 12)`, it's referenced by a direct pointer
- if block number is in `[12, 12+1024)`, it's referenced by the indirect pointer

### Other solutions
- contiguous allocation (extents)
    - simple to keep track of blocks
    - external fragmentation
- linked list
    - each data block points to the next
    - random access is slow

## Directory organization
- implemented as a file with specially-formatted content
![[fs_directory_dentries.png]]

## Free space management
- need to track which inodes/blocks are used/unused
- use a bitmap for each (one for inodes, one for blocks)
- to create a file, search the bitmap for an unused entry
- to delete a file, unset the bitmap entry

## ?
- how many disk I/Os to to read `/foo/bar`?
    - read inode of `/` (inode of root is fixed, usually 2)
    - read contents of `/`, search for `foo/` and get its ino
    - read inode of `foo/`
    - read contents of `foo/`, search for `bar/` and get its ino
    - read inode of `bar`
    - read contents of `bar`
-  caching this file means other reads of `/foo/bar` don't need 6 disk I/Os