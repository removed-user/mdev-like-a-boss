You can generate proper setkeycodes commands from udev's sources with:
`get-keys.awk` 

ex:
`awk -f keymaps/get-keys.awk /usr/include/linux/input.h /PATH/TO/UDEV/SOURCE/DIR/src/keymap/keymaps/NAME`

To make life easier.
`keymap` dir contains pre-generated scripts from udev-182's keymaps files.
All what you need is to put 
`/opt/mdev/keymaps/NAME.sh` into one of your startup scripts.

I think for the Key maps configuration repo...
A tool that auto-generates these keymaps 
from the udev hwdb databases upstream sources... 
would be useful. 

Personally: I think for my previous device/laptop
I got the xxnn identifier for my keyboard and generated relevant keycodes manually...
For a useful tool that does that automatically... 

We can query PCI.ids USB.ids etc. databases directly and
get the vendor/device mapping


from there:
We can cross reference that against the devices actually connected to the pc/pci-bus
and put just those vendor Id values/ attributes to a database in /etc/hwdb/local-hardware.bin

possibly using systemds native tool for update-hwdb, but only after extracting/isolating it.

From there - we can use a script to output relevant additional keycodes for said 
"native devices". 
Before mapping those into kbd commands

Then write those into a database that references back to the hwdb.bin file

Maybe with all of these we could have a keycodes.bin file that uses a structure like 
hwdb.bin... but just contains the device Id and extra keycodes, so later others can download that archive
activate their keyboard and then remove it through debconf. 

such that it creates a keycodes file to use in scripts
