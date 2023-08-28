# MavLink

MavLink is a protocol. It includes a packet format.

- Using the full protocol may be too hard
- We could just use the packet format + serialization/deserialization libs
- `minimal.xml` contains the "bare minimum"
    - enums for endpoint types, statuses, etc.
    - heartbeat and protocol version packets
- We should:
    - Include `minimal.xml`
    - Add packet types for our own sensors
    - Not worry about the rest of the protocol
        - We can skip heartbeat packets, version negotiation, etc.
