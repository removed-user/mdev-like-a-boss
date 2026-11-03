# Mdev Utils

> Forked from: mdev-like-a-boss, mdev like a boss

A notes, scripts and configs
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

`settle-nics` creates a file 
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
