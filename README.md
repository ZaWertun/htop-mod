htop-mod
========

patch(es) for htop (open source process viewer for Linux) 

htop-2.0.2-pss.patch
--------------------

This patch adds a
  * PSS (proportional set size) field to htop.
  * SWAP
  * SwapPss

PSS is a newish per-process memory stat in Linux (added in version 2.6.25 of the
kernel) that estimates how much memory a process really uses, after taking into
account how many other processes share memory with it.

Description on wikipedia

https://en.wikipedia.org/wiki/Proportional_set_size

Article on LWN

https://lwn.net/Articles/230975/


For more info please read description of /proc/PID/smaps file.

https://www.kernel.org/doc/Documentation/filesystems/proc.txt

This problem resolved now.

~~There's a downside to calculating PSS in htop, and that's that it causes htop to
use a lot more CPU. This is because the Linux kernel doesn't provide PSS for
each process in a single place but instead requires scanning the entire
/proc/<pid>/smaps file for each process. On my laptop with an Intel Core
i3-2350M processor, htop running with the PSS patch consumes about 20% of one
CPU. That's with an htop refresh rate (delay) of 1.5 seconds and about 60
processes running. More processes means more CPU use.~~

Performance tests
-----------------
When PSS column is hidden, one cycle of LinuxProcessList_recurseProcTree took 20ms, on 181 total tasks (user/kernel threads are disabled).

When PSS column is visible, one cycle of LinuxProcessList_recurseProcTree took 200ms, on 181 total tasks,  10 times slower but not significally.

PPA
---
For ubuntu patched htop available here https://launchpad.net/~linvinus/+archive/ubuntu/linvinus

Screenshot
----------
![screenshot](screenshot.png?raw=true)

Other
-----
Other tools that show PSS

**smem** https://www.2daygeek.com/smem-linux-memory-usage-statistics-reporting-tool/#

**libpagemap** https://github.com/pholasek/libpagemap

there was a patch based on libpagemap for htop-0.9-3.fc16.src.rpm

https://github.com/dwks/pagemap
