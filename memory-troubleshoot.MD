# Memory management in Linux

Linux memory management is a critical component of the operating system that ensures efficient utilization of physical(RAM) and virtual memory(SWAP).

### What is swap memory?

Swap memory is a portion of disk storage that Linux uses as an extension of physical RAM when the system runs out of available memory. Recently mobiles are coming up with this kind virtual memory.

### When swap is used?
* When RAM is full, inactive files are moved from RAM to disk(swap) to free up space for active processes.
* This prevents applications from crashing due to Out of Memory (OOM).
* Swap is slower than RAM because it’s on disk (SSD/HDD), but better than no memory.

### How It Works (Simplified)
* RAM fills up due to running processes.
* The Linux kernel identifies least-used memory pages.
* These pages are moved to the swap space on disk.
* When those pages are needed again, they're swapped back into RAM.

### When Swap Is a Problem
* High swap usage may mean your system lacks enough RAM.
* Can cause slow performance due to disk I/O bottlenecks.

We can use below command to check memory
```shell
free -h
```

```shell
               total        used        free      shared  buff/cache   available
Mem:           709Mi       258Mi       413Mi       2.0Mi       133Mi       451Mi
Swap:          2.0Gi          0B       2.0Gi
```


