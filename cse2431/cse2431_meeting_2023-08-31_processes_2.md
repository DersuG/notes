# CSE 2431: Processes 2 (2023-08-31)

## Process hierarchy
- processes have children
- `ps` command gets info about processes

## Fork
- clones the PCB of the process
- returns 0 if we are the child
- returns the PID of the child if we are the parent
- returns a negative on error
