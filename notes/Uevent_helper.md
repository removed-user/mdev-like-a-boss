# Uevent helper
> A Uevent helper listens on a netlink (kernel) socket... 

>Then runs a set/specified command for each time the kernel notifies it

> Which happens on hotplug events. 

In our case we want to run mdev when notified. 

This is drastically better than udev... instead of a constantly running daemon waiting for events - we trigger an event when asked to, by the only part of the system that knows when we need to. 


- nldev
    - is a known uevent helper
- busybox
    - The original mdev-like-a-boss repo pointed to `busybox` as something with a uevent helper also

- CONFIG_UEVENT_HELPER
    - Thirdly there is the kernel config Item


I'm not entirely sure
A: which is which - 
or
B: whether the busybox/nldev helpers are necessary 
or 
C: If the CONFIG_UEVENT_HELPER was added before/after or - 
makes the other tools unnecessary 
