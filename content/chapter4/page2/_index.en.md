---
title: 'Preparing for Bootware™'
date: 2024-10-25T12:49:09-04:00
weight: 2
reading_time: 3 minutes
---

## Finally, making it secure

Now that we have a proper security device installed, tested, and ready let's secure this thing. At the very same time, let's make sure that we can securely update the device when the time comes, and that it is built to be recoverable in case an update fails.

Ordinarily this would be a ton of work, but we're going to simplify everything and do it pretty much all at once.

### A place to put the backup image

Since we will be using Bootware(r) to secure our device, we will need a place for the system to copy the entire SD Card as it encrypts it. For this, we're going to use a USB Drive.

We need to make sure that we can use our USB Drive properly. I often re-use them for other tasks, so here's how I like to start out. After plugging the USB Drive in, I make sure to "zero out" the drive, then create a brand new partition map and file system on it.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=512 count=1 conv=notrunc
```
```bash
1+0 records in
1+0 records out
512 bytes copied, 0.0197125 s, 26.0 kB/s
```

That clears the previous file system, if any.

```bash
sudo fdisk -W always /dev/sda
```
```bash
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS (MBR) disklabel with disk identifier 0x27b0681a.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-125313282, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-125313282, default 125313282):

Created a new partition 1 of type 'Linux' and of size 59.8 GiB.
Partition #1 contains a ext4 signature.

The signature will be removed by a write command.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

The important parts there are, once you've entered the `sudo fdisk -W always /dev/sda` you will enter `n` to create a new partition map. Then `p` to make it a Primary partition, and finally `w` to write the partition map to the disk. For everything else, I just accept the defaults as presented.

Finally, now that we have a partitioned USB Drive, we have to create a proper file system on it.

```bash
sudo mkfs.ext4 -j /dev/sda1 -F
```
```bash
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 15663904 4k blocks and 3916304 inodes
Filesystem UUID: 4a3af5d0-bac4-4903-965f-aa6caa8532cf
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424

Allocating group tables: done
Writing inode tables: done
Creating journal (65536 blocks): done
Writing superblocks and filesystem accounting information: done
```

You should now have a proper file system on the USB Drive, which we will use as our space for creating our secure boot image next.

> [!WARNING]
> **Windows users**
>
> We have created an ext4 filesystem on the USB Drive. Windows cannot mount or read ext4 filesystems, so you will be unable to use the USB Drive from a Windows system without reformatting the drive (and erasing all data on it).
>
{{< pagenav prev="../page1" next="../page3/" >}}