htop-mod
========

patch(es) for htop (open source process viewer for Linux) 

htop-1.0.2-pss.patch
--------------------

This patch adds a PSS (proportional set size) field to htop.

PSS is a newish per-process memory stat in Linux (added in version 2.6.25 of the
kernel) that estimates how much memory a process really uses, after taking into
account how many other processes share memory with it.

There's a downside to calculating PSS in htop, and that's that it causes htop to
use a lot more CPU. This is because the Linux kernel doesn't provide PSS for
each process in a single place but instead requires scanning the entire
/proc/<pid>/smaps file for each process. On my laptop with an Intel Core
i3-2350M processor, htop running with the PSS patch consumes about 20% of one
CPU. That's with an htop refresh rate (delay) of 1.5 seconds and about 60
processes running. More processes means more CPU use.

