---
title: systemd
taxonomy:
    category: docs
---

### Units

There are different unit files for different purposes. All of them have some common properties. For more information run the following command:

```
$ man systemd.unit
```

Some of the most important unit files are:

#### Service

For more information run:

```
$ man systemd.service
```

#### mount

For more information run:

```
$ man systemd.mount
```

#### automount

For more information run:

```
$ man systemd.automount
```

### Run a Systemd Unit When a USB Storage is Plugged-in

Basically we are going to extract some unique properties of a USB storage device like `idVendor` and `idProduct` using [udev](https://wiki.archlinux.org/title/Udev) rules that uniquely identify that device when plugged-in. Then we are adding a new udev rule in which a systemd service is called when that storage device is plugged in.  For the syntax of rule files you can read `RULES FILES` section by running:

```
$ man udev
```

#### Extract Device Storage Unique Properties

First we plugged the storage device. Then we use one of these commands to extract unique properties such as `idVendor` and `idProduct`. I assumed the device is `/dev/sda` and it has only one partition named `/dev/sda1`.

```
$ udevadm info --attribute-walk --name=/dev/sda1

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/ff500000.dwc3/xhci-hcd.0.auto/usb2/2-1/2-1.3/2-1.3:1.0/host0/target0:0:0/0:0:0:0/block/sda/sda1':
    KERNEL=="sda1"
    SUBSYSTEM=="block"

...

  looking at parent device '/devices/platform/ff500000.dwc3/xhci-hcd.0.auto/usb2/2-1/2-1.3':
    KERNELS=="2-1.3"
    SUBSYSTEMS=="usb"

...

    ATTRS{idProduct}=="foo_product"

...
    ATTRS{idVendor}=="foo_vendor"
```

udev doesn't guarantee to use `/dev/sda` for this device the next time we boot the system. So we need to use regex to handle this case. You can also ignore `KERNEL` and `SUBSYSTEM`. In this rule we start `mnt-backup.mount` which mount the device. Basically we mount and run backup service when the device is plugged in:

```
$ cat /etc/udev/rules.d/90-backup.rules

KERNEL=="sd[a-z]1", SUBSYSTEM=="block", SUBSYSTEMS=="usb", ATTRS{idVendor}=="foo_vendor", ATTRS{idProduct}=="foo_product", ACTION=="add", ENV{SYSTEMD_WANTS}+="mnt-backup.mount backup.service"
```

We can test our rule by running:

```
$ sudo udevadm test $(udevadm info --query=path --name=/dev/sda1) 2>&1
```

For more information read this Arch [wiki](https://wiki.archlinux.org/title/Udev).

### Finding the location of unit file on the disk

You can run `systemctl show` and look at the value of `FragmentPath`. For example:

```
$ systemctl show httpd.service  | grep FragmentPath
```

