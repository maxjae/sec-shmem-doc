
# Documentation

This file contains information about things that do not fit into another category.

## BAR setup / config space accesses

- seabios:
  - Populates PCI device list (<https://github.com/coreboot/seabios/blob/a6c8e8bbdf24ece39e4f55c8ab464d28e5e44745/src/hw/pcidevice.c#L21>)
  - Reads requested BAR size by writing all ones, and then reading the size (<https://github.com/coreboot/seabios/blob/a6c8e8bbdf24ece39e4f55c8ab464d28e5e44745/src/fw/pciinit.c#L697>)
  - Writes address of region, which is of sufficient size, into the device's BAR register (<https://github.com/coreboot/seabios/blob/a6c8e8bbdf24ece39e4f55c8ab464d28e5e44745/src/fw/pciinit.c#L93>)
- kernel:
  - reads the address again from BAR register and populates its own pci list with the memory regions (<https://elixir.bootlin.com/linux/v6.15.1/source/drivers/pci/probe.c#L183>)

## Qemu context

vfu_realize_ctx (libvfio) sets up config space and allocates it if not present. Also sets type of bar regions

<https://github.com/maxjae/multiproc-qemu/blob/3711d37e40720ef794e08a743372cec8041fbe17/hw/vfio/user-pci.c#L307>: copies config space on qemu vm side

<https://github.com/maxjae/multiproc-qemu/blob/61db152ff5875cf603ef728f79ad03fd220c33f5/hw/vfio/user.c#L1711>: used to communicate over socket

<https://github.com/maxjae/multiproc-qemu/blob/61db152ff5875cf603ef728f79ad03fd220c33f5/hw/vfio/user.c#L1711>: used for read/writes to config space
