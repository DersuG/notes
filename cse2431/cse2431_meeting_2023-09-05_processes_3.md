# Meeting 2023-09-05

## IPC models
- **shared memory:** multiple processes share the same memory
    - 
- **message passing:** processes send messages between them
    - can be blocking or nonblocking

## Shell pipes
- e.g. `ls -l | grep "hi"`
    - `ls` spawns a process
        - has 3 file descriptors open at the start:
            - `stdin`
            - `stdout`
            - `stderr`
    - `| grep` spawns another process
        - has 3 file descriptors open at the start
            - `ls`'s `stdout` is redirected to `grep`'s `stdin` through the pipe
