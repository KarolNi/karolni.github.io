---
date: 2019-12-30
author: KarolNi
tags: proxmox btrfs virtio
---

Today I tested basic safety of virtualizing my BTRFS NAS using proxmox.
Initial plan was to pass through integrated HBA of my Proliant ML310e Gen8 to VM, but it was not possible due to HP Reserved Memory Region Reporting (RMRR) forbidding passing through basically any integrated pci device.

I already have some data in my btrfs pool (backed up, but restoring would not be fun). I had to test whether my data is safe when passing drive to virtualized NAS.

I passed two pendrives to test VM in proxmox with virtio:

```
qm set 100 -virtio0 /dev/disk/by-id/usb-drive1  # where 100 is ID of test VM and drive1 is physical ID of passed drive
qm set 100 -virtio1 /dev/disk/by-id/usb-drive2  # second drive (for RAID1 test)
```

I tested two scenarios:

- btrfs created in VM
```
mkfs.btrfs -d raid1 -m raid1 /dev/vda /dev/vdb  # vdX is drive device of virtio
```
Than I populated it with test data and then mounted it on bare metal system and it was all OK.

- btrfs created in bare metal
Fresh volume was created on bare metal Linux, then drives was mounted in VM. All data were OK, scrub in VM was successful.


### Versions

Guest: Debian 10
Bare metal test: Debian 9
Hypervisor: Proxmox 5.4-13
