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




## ORIGINAL INSTALL GUIDE DONT FOLLOW


- Clone repo as root to /opt/mdev::

        git clone https://github.com/removed-user/mdev-like-a-boss.git /opt/mdev

- Copy `/opt/mdev/mdev.init` to `/etc/init.d/mdev`, replace the original one if exist, add exec bit and add to sysinit runlevel::
        
        cp /opt/mdev/mdev.init /etc/init.d/mdev
        chmod 700 /etc/init.d/mdev
        rc-update add mdev sysinit

- Remove udev from sysinit runlevel (it can be udev or udevd, depends if Gentoo or Funtoo)::

        rc-update del udev sysinit

- Copy or symlink ``/opt/mdev/mdev.conf`` to ``/etc/mdev.conf``::

        ln -sf /opt/mdev/mdev.conf /etc/mdev.conf

- Now you can test mdev.





Xorg
====
The input configuration is not the same as with evdev, we will no longer use 'InputClass' but 'InputDevice' sections. You propably do not need ``xorg.conf`` at all. Create ``/etc/X11/xorg.conf.d/`` and copy content of the ``/opt/mdev/xorg.conf.d/`` there. Then adjust the config files as you wish.

Keymaps
=======

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


You can generate proper setkeycodes commands from udev's sources with ``get-keys.awk`` script. like::

        awk -f keymaps/get-keys.awk /usr/include/linux/input.h /PATH/TO/UDEV/SOURCE/DIR/src/keymap/keymaps/NAME

To make life easier, ``keymap`` dir contain already generated scripts from udev-182's keymaps files. All what you need is to put ``/opt/mdev/keymaps/NAME.sh`` into one of your startup scripts.

Preserve interfaces names
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
`sesettle-nics` creates `/etc/mactab` file of
detected
- eth* 
- wlan*, ath*, wifi*, ra* and usb* interfaces. Any other is ignored, you need to edit this file by hand if you wish add other names.

