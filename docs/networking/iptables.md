!!! note "Work in progress"
    This document is a work in progress.
    
    
```
                                                                   XXXXXXXXXXXXXXXXXX
                                                                 XXX     Network    XXX
                                                                   XXXXXXXXXXXXXXXXXX
                                                                           +
                                                                           |
                                                                           v
                                     +-------------+              +------------------+
                                     |table: filter| <---+        | table: nat       |
                                     |chain: INPUT |     |        | chain: PREROUTING|
                                     +-----+-------+     |        +--------+---------+
                                           |             |                 |
                                           v             |                 v
                                     [local process]     |           ****************          +--------------+
                                           |             +---------+ Routing decision +------> |table: filter |
                                           v                         ****************          |chain: FORWARD|                               
                                    ****************                                           +------+-------+
                                    Routing decision                                                  |
                                    ****************                                                  |
                                           |                                                          |
                                           v                        ****************                  |
                                    +-------------+       +------>  Routing decision  <---------------+
                                    |table: nat   |       |         ****************
                                    |chain: OUTPUT|       |               +
                                    +-----+-------+       |               |
                                          |               |               v
                                          v               |      +-------------------+
                                    +--------------+      |      | table: nat        |
                                    |table: filter | +----+      | chain: POSTROUTING|
                                    |chain: OUTPUT |             +--------+----------+
                                    +--------------+                      |
                                                                          v
                                                                   XXXXXXXXXXXXXXXXXX
                                                                 XXX    Network     XXX
                                                                   XXXXXXXXXXXXXXXXXX

```
