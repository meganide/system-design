# System Design Detailed
How do you glue an entire system together?

## Computer Architecture
Computers function through a layered system, each optimized for a specific task.

At the core computers only understand binary, 0s and 1s, these are represented as bits. 

- **Bit:** The smallest piece of data, either 0 or 1.
- **Byte:** 8 bits, used to represent a single character or number (Like A or 1).
- **KB:** 1024 bytes
- **MB:** 1024 KBs
- **GB:** 1024 MBs
- **TB:** 1024 GBs

### Disk Storage
To store this data we have computer disk storage, which holds the primary data, it can be either HDD or SDD type.
It is non-volatile, which means that it maintains data without power. If you turn it off, the data will still be there.

- Size: Hundreds of GBs to multiple TBs
- HDD read speed: 80 MB/s to 160 MB/s
- SDD read speed: 500 MB/s to 3500 MB/s

### RAM (Random Access Memory)
RAM serves as the primary active data holder and holds data structures, variables, and application data that are currently in use or being processed.   

When a program runs its variables, intermediate computations, and more are stored in RAM because it allows for quick read and write access. This is a volatile memory which means that it requires power to retain is contents, if you restart your computer the data might not be persisted.

- Size: A few GBs to hundreds of GBs in high end servers.
- Read/Write speed often >5000 MB/s

### Cache
But sometimes, even the RAM speed isn't enough which brings us to the cache.
The cache is smaller than the RAM, typically measured in MBs.

The access times for cache memory are even faster than RAM, just a few nano seconds.

The CPU first checks the L1 cache for the data, if it is not found it then checks the L2 and the L3. 
Finally, it checks the RAM.

The purpose of the cache is to reduce the average time to access data, that's why we store frequently accessed data here to optimize CPU performance. 

### CPU
CPU the brain of the computer, it fetches, decodes and executes instructions.
When you run your code it is the CPU that processes the operations in that program.

Before it can run our code which is often written in high level languages like Python, JS, Java etc. it first needs to be compiled to machine code (the loewst level representation of a computer program, 0s and 1s).

A compiler performs these translations and then the CPU can execute it.  

The CPU can read and write from our RAM, disk and cache data.

### Motherboard
The component that connects everything, provides the pathways that allow the data to flow between these components. 



