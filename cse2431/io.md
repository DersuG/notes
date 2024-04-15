# I/O

## Device controller
- has registers for data, status, control
- CPU and device controller talk via I/O instructions and registers

## Programmed I/O
- every I/O operation is programmed and controlled
- e.g. printing means data is sent to kernel, then OS enters a tight loop outputting the characters one at a time
- polling

```c
void copy_from_user(char *buffer, int count)
{
    for (i = 0; i < count; i++)
    {
        while (*printer_status_register != READY) {}
        *printer_data_register = buffer[i];
    }
}
```

## Interrupt-driven I/O
- device sends interrupt to interrupt controller
- interrupt controller issues interrupt to CPU
- CPU has an interrupt report line that's checked after every executed instruction
- interrupt handler

## I/O via DMA
- DMA controller is a special processor
    - slower than CPU
- host writes a DMA command block
    - pointer to source
    - pointer to destination
    - number of bytes
- CPU writes address of command block to DMA controller
- DMA controller operates memory bus directly

## DMA issues
- handshaking between DMA controller and device controller
- **cycle stealing:** DMA controller takes away CPU cycles when it uses CPU memory bus, blocking CPU from accessing memory

## Memory-mapped I/O and Port I/O
- 3 ways:
    - port I/O:
        - separate I/O and memory address space)
    - memory-mapped I/O:
        - (I/O and memory share one address space)
    - hybrid

## Memory-mapped I/O
- pros:
    - don't need assembly to access I/O
    - no special protection mechanisms needed
    - every instruction can access I/O
- cons:
    - need to selectively disable caching
    - both memory modules and all I/O devices need to check on memory bus
        - harder with multiple buses
- 