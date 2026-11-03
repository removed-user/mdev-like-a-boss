================
mdev like a boss
================

A stash for notes, scripts and configs
for systems running `mdev` as a `udev` replacement.

Manual install
--------------

... dont. It hasnt been updated since 2012. 
It's not ready yet. 

- Apparently I patched the HELL out of my owm version of mdev. 

So, on trying to move to a reproducible setup, I found out... all over again how much I hate udev...

AKA: This repo has a... somewhat long way to go before it's ready.

But - is intended to contain the reusable scripts and logic for using mdev on groups/collections of device types. 

Ie: hotplugging functions and general utility. 

Naturally there will be a lot of overlap between that and coldplug scripts 

coldplug scripts will likely be in a seperate repo, possibly 
and this repo will exist as a component of that one. 

If I rename the project - 
- I'll keep mdev-like-a-boss in the description
To make it easier for anyone else who liked using an mdev setup and needs help setting it up.

## GOALS
This repo should contain scripts to...

- reliably
- predictably
- idempotently
 and
- Properly

Set, Configure and bind "device nodes" into `/dev` based on the contents of the sysfs: `/sys`

Which fall outside of the domain of the more basic basic mdev functionality.

Meaning this repo has scripted mdev rules for 
- all storage items under /dev/disk/by-*
- all usb device nodes
- all types of network interfaces

With the goal being - "possibly"
to add configurations for "abnormal file systems" - if needed... 

mdev was so reliable - that I don't even remember 
but there may be a need/case for adding dedicated scripts for zvols or btrfs filesystem devices to be treated differently from a standard partitioned device. 

2nd





Keymaps
=======

https://pci-ids.ucw.cz/

A second repo will be created for keymap utils... that's not part of mdev, but is part of udev, and - as such will be fulfilled by other tools.

One of udev's features is config keymaps when it detects a specified vendor.

That is not the role of a device manager...

mdev is for /dev, not for device databases and device/keyboard configuration?

We have a tool to manage keymaps... its called kbd. 

We also have tools for "higher level" usage of kbd 

You feed it a keymap, and it loads that to the kernel. It should never change unless you buy a new keyboard. 

The default (kernel provided) keymap... works 99% of the time with 98% accuracy... 

So the fact that udev... a device manager, also... maintains a dedicated device hardware database, maps keyboard devices to keymaps and loads keymaps is absolutely fucking insane

and not in a good way. 

That said... that 2% where you lose your macro keys is real annoying - so - thats a seperate project.

Knowing what keymap item to add a missing macro is odd 

In order to make your special keys work you need to assign keycode to them.

keycodes depend on the byte sequence sent to the kernel when pressing a key

Which in turn - depends on the firmware of the keyboard... 

So different devices may have different keycodes for odd/unusual keys or keymaps. 

To figure out which keycodes have to be added to the kernels map? 
Practically - 
you need;
- A list of the keycodes sent by a device
    - filtered by all standard keys
- Associations between remaining keycodes and keys.

- Those can be auto generated using a hardware database, and we have a few sources for those, that being:
- from the kernel source directly
- from your keyboards firmware directly
- from systemd-udev
- from LVFS
- fwupd
- and... one source I don't remember off the top of my head? maybe add it later. 

Preserve/rename interfaces names
=========================
Mdev does not provide bulitin support for renaming NICs.

However there is `settle-nics` script which will do it using `nameif`
from:
    - net-tools
    - busybox

`ip`
from:
    - original iproute2 
or 
    - busybox's iproute2 

`sesettle-nics` creates a file 
`/etc/mactab`
consisting of detected interfaces
of the following types:
   - eth* 
   - wlan*
   - ath*
   - wifi*
   - ra*
   - usb*

Any other is ignored, you need to edit the `settle-nics` script if you wish add other names. 
mdev can trigger the nameif command on any of these devices.
