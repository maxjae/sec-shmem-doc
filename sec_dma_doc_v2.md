
# Documentation

This file gives an overview of the removal of vfio-user from the project.

## Existing implementation

This file gives an overview of the multiproc-qemu project, which aims to seperate the VM CPU emulation from the device emulation. There is [existing work](https://elixir.bootlin.com/qemu/v10.0.2/source/docs/devel/multi-process.rst) on the subject of remote device emulation in QEMU.

- Emulated device process
  - In order for a remote device to be emulated by QEMU it needs a machine, which offers parent busses, memory regions etc.. [This](https://elixir.bootlin.com/qemu/v10.0.2/source/hw/remote/machine.c) machine type, implemented for those needs, offers that. libvfio-user also uses this machine type for the remote device emulation.
  - [This](https://elixir.bootlin.com/qemu/v10.0.2/source/hw/pci-host/remote.c) pci host device allows the remote pci device to work in this special environment.
- VM
  - Because the device itself is not emulated in the same process as the VM there needs to be a replacement ("fake device") which delegates the real interactions to the emulated device.  
  [This](https://github.com/harshanavkis/multiproc-qemu/blob/60f1fba8bef657e533f2ee70faee6bce108bc45b/hw/vfio/user-pci.c#L429) is the device used by vfio-user and can be used as a reference.

## New Structure

The new structure would be similiar to vfio-user with the difference of no communication via a UNIX socket, but only over the shared memory. This would then also allow to place proxy and VM on different hosts.

![new structure](res/disagg_doc-restructuring.drawio.svg)

As the communication via MMIO and DMA is already implemented over shared memory, the focus now lies on accesses to config space.  
In order to get completely rid of vfio-user there is also the need for a fake device, which is exposed by QEMU to the VM as the real device.
