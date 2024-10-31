---
title: 'Installing Bootware™'
date: 2024-10-25T12:53:24-04:00
weight: 3
---

### A place to put the backup image

Since we will be using Bootware™ to secure our device, we will need a place for the system to copy the entire SD Card as it encrypts it. For this, we're going to use a USB Drive.

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

> [!NOTE]
> If, like me, you get tired of typing `sudo` all the time, you can run `sudo -i` once and get a root prompt from which to run all your commands. But remember, with great power comes great responsibility!

### Installing Bootware™

Bootware is the Zymbit tool for securing and updating your Raspberry Pi. It is a powerful tool that allows you to update one, or an entire fleet, or Pis across your enterprise. And it allows you to do it safely, securely, and in a way that is recoverable if something goes wrong.

First, we have to run the installer

```bash
curl -sSf https://raw.githubusercontent.com/zymbit-applications/zb-bin/main/install.sh | sudo bash
```

This installer will ask you a couple of simple questions, so let’s go through the answers. The first is whether or not you’d like to include Hardware Signing. If you have an HSM6 or SCM-based product, you can answer yes to this question. If you’ve got a Zymkey or HSM4, Hardware Signing is not supported, so you don’t need to install it. Even with software signing, your final LUKS encrypted partitions will be protected by the Zymbit HSM keys.

Next it will ask you which version of Bootware to install. Choose the most recent version.

```bash
zb-install.sh: bootstrapping the zbcli installer
	---------
	Pi Module:         Raspberry Pi 4/Compute Module 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	---------

✔ 'zbcli' comes with software signing by default. Include hardware key signing? (Requires SCM or HSM6) · No
✔ Select version · zbcli-1.2.0-rc.26
Installing zbcli
Installed zbcli. Run 'zbcli install' to install Bootware onto your system or 'zbcli --help' for more options.
zb-install.sh: cleaning up
```

Now that the installer is ready, it's time to install Bootware™ itself:

```bash
sudo zbcli install
```

The installer will ask you if you're ready to reboot when it's done:

```bash
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	------reading_time: 5 minutes
---
       Found kernel '/boot/firmware/kernel8.img'
     Created '/etc/zymbit/zboot/mnt'
     Created '/etc/zymbit/zboot/scripts'
     Created '/etc/zymbit/zboot/zboot_backup'
     Created '/boot/firmware/zboot_bkup'
   Installed 'u-boot-tools'
     Created '/etc/fw_env.config'
     Created '/usr/bin/zbconfig'
       Found OpenSSL 3
     Created '/boot/firmware/zb_config.enc'
    Modified zbconfig 'kernel_filename'
   Installed zboot
    Modified '/etc/rc.local'
     Created '/lib/cryptsetup/scripts/zk_get_shared_key'
    Modified '/boot/firmware/config.txt'
     Created '/etc/update-motd.d/01-zymbit-fallback-message'
    Modified /etc/update-motd.d/01-zymbit-fallback-message
✔ A reboot into zboot is required. Reboot now? · yes
    Finished in 29.1s
```

After the reboot, Bootware should be successfully installed. Now we can move on to creating secure partitions, and preparing for secure updates.

{{< pagenav prev="../page2" next="../page4/" >}}