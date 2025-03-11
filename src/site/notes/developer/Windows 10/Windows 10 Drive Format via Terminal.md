---
{"dg-publish":true,"tags":["windows","microsoft"],"permalink":"/developer/windows-10/windows-10-drive-format-via-terminal/","dgPassFrontmatter":true}
---

```
diskpart
```

- Next, type the command below and hit Enter:

```
list disk
```

The command will list all the hard drives on your computer.

- Next, type the command below and hit Enter:

```
select disk 3
```

Here, disk 3 is the flash drive plugged into the system. Be sure to select the number that corresponds to the flash drive inserted in your system.

- Next, type the command below and hit Enter:

```
 list partition
```

The command will list all the partitions on the USB flash drive.

- Next, type the command below and hit Enter:

```
 select partition 1
```

In this case, we assume partition 1 is the partition we want to delete. Make sure to select the number that corresponds to the partition on your flash drive.

- Finally, type the command below and hit Enter:

```
delete partition
```

---
## Credit
- https://www.thewindowsclub.com/delete-volume-option-is-greyed-out-for-usb-flash-drive