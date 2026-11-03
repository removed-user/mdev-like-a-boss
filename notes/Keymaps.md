## GOALS
- Strip out the keycodes and other values but only after...
    -  sourcing a flat-text safety/default keycode file
    - along with adding dependency on 
         - pci.ids, usb.ids, pnp.ids, oui.txt


Keymaps
=======
Thankfully - recently the newer `pci.ids` package ships PCI vendor data independently of systemd. 
While also the `pciutils` package provides an update-pciids script. 

Both of these can be used to check the repository at 
`https://pci-ids.ucw.cz/` - and update our local copy of 
`/use/share/misc/pci.ids`

Using that + the systemd script that creates a static map/.bin file - we can then work to completely decouple the systemd-hwdb program into a dedicated hwdb management utility... which can then be used to check against your hardware to see if you need any of the additional keycodes to be generated. 


A second repo will be created for keymap utils... that's not part of mdev, but is part of udev, and - as such will be fulfilled by other tools.

One of udev's features is config keymaps when it detects a specified vendor.

That is not the role of a device manager...

mdev is for /dev, not for device databases and device/keyboard configuration?

We have a tool to manage keymaps... its called kbd. 

We also have tools for "higher level" usage of kbd 

You feed it a keymap, and it loads that to the kernel. It should never change unless you buy a new keyboard. 

The default (kernel provided) keymap... works 99% of the time with 98% accuracy... 

So the fact that udev... a device manager, also... maintains a dedicated device hardware database, maps keyboard devices to keymaps, and loads keymaps is absolutely fucking insane

Not in a good way. 

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
