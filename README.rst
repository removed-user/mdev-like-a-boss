================
mdev like a boss
================

This repo is a stash for notes, scripts and configs for the system running with mdev as a udev replacement.

Quickstart
==========
Gentoo-based system as example.

Manual install
--------------

... dont. It hasnt been updated since 2012. 
It's not ready yet. 

Apparently I patched the HELL out of my version of mdev. 

So, on trying to move to a reproducible setup, I found out all over again how much I hate udev...

AKA: This repo has a... somewhat long way to go before its ready.

But - is intended to contain the reusable scripts and logic for using mdev on groups/collections of device types. 
Ie:
hotplugging functions and general utility. 

Naturally there will be a bit of overlap between that and coldplug scripts 

coldplug scripts will likely be in a seperate repo, possibly copied here as a submodule, and I may rename the project - but keep mdev-like-a-boss in the description to make it easier for anyone else who liked using an mdev setup and needs help setting it up.

all storage items under /dev/disk/by-*
all usb device nodes


- Clone repo as root to /opt/mdev::

        git clone https://github.com/slashbeast/mdev-like-a-boss.git /opt/mdev

- Copy ``/opt/mdev/mdev.init`` to ``/etc/init.d/mdev``, replace the original one if exist, add exec bit and add to sysinit runlevel::
        
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
One of udev's features is config keymaps when it detect specified vendor.

That is not the role of a device manager...

Mdev does not do so, mdev is for /dev, not for all-other-things.

We have a tool to manage keymaps... and its called kbd. 

You feed it a keymap, and it loads that to the kernel. It should never change unless you buy a new keyboard. 
The default (kernel provided) keymap... works 99% of the time with 98% accuracy... 

That said... that 2% where you lose your macro keys is real annoying - so - thats a seperate project.

With seperate tooling and... this tool and all other mdev tools its a part will 


Knowing what keymap item to add is an odd 
along with recognizing devices... 


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

ip
from:
    - original iproute2 
or 
    - busybox's iproute2 
`settle-nics` creates `/etc/mactab` file of
detected
- eth* 
- wlan*, ath*, wifi*, ra* and usb* interfaces. Any other is ignored, you need to edit this file by hand if you wish add other names.

Random notes
============
- Keycodes under Xorg may be different than with evdev, for example mute keycode is no longer 121 but 160. Install 'xev' and check your keycodes if you remap or bind them with xmodmap.
- Mdev does not support udev's udisks and so on, Full blown desktop environments may not really like the change, you will lost your DE's automount stuff etc. But there is ``pmount`` and you can always config automount script in ``/etc/mdev.conf``
- Unmerging udev may not be good idea, as for example chromium need libudev to compile. Better append ``sys-fs/udev -*`` to package.use and put ``sys-fs/udev-init-scripts-10`` into ``/etc/portage/profile/package.provided``. Then you can rebuild udev with all USE flags disabled and remove udev-init-scripts.
- `lsusb` seems to run `/etc/udev/hwids.bin` to identify devices. One may need to add `udev` USE flag to the hwids package if wishes to see the device names.
