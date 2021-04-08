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

### Tables
iptables contains five tables:

- `raw` is used only for configuring packets so that they are exempt from connection tracking.
- `filter` is the default table, and is where all the actions typically associated with a firewall take place.
- `nat` is used for network address translation (e.g. port forwarding).
- `mangle` is used for specialized packet alterations.
- `security` is used for Mandatory Access Control networking rules.

