- Support GPT's PARTUUID and PARTLABEL in storage helper.
- Reduce proc-bus-usb (should be dev-bus-usb anyway) as we will have full path where to store device nodes anyway.
- [Uevent Helper](./notes/Uevent_helper.md)

- Strip out the keycodes and other values but only after...
    -  sourcing a flat-text safety/default keycode file
    - along with adding dependency on 
         - pci.ids, usb.ids, pnp.ids, oui.txt
