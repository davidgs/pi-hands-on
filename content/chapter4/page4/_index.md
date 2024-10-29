+++
title = 'Secure Booting'
date = 2024-10-25T13:12:15-04:00
weight = 4
+++

### Configuring Bootware

This is where the real fun begins! If you’ve ever used LUKS to encrypt a Pi filesystem before, you know that, while it’s a great step in securing your Pi, you still have to store that encryption key somewhere that is accessible at boot time.

With Bootware and a Zymbit HSM, the LUKS encryption key is locked by the Zymbit HSM, making it much more secure. Bootware expects the boot image to be in a specific, encrypted format called a z-image. The Bootware CLI tool helps you create and manage these images for deployment across your enterprise.

So let’s create our first z-image, and we’ll use the current system as the basis for it.

First, we need to mount the USB Drive so that we have someplace to put our z-image:

```bash
sudo mount /dev/sda1 /mnt
```

Next, we'll run the imaging tool to create an encrypted z-image of our current system:

```bash
sudo zbcli imager
```
```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	---------
     Created '/etc/zymbit/zboot/update_artifacts/tmp'
✔ Enter output directory · /mnt
✔ Enter image name · z-image-1
✔ Select image type · Full image of live system
✔ (Optional) enter image version · 1.0
✔ Select key · Create new software key
```

Notice that I used the mount point for the USB Drive as our output directory. I then chose a name and version number for the image and chose to use a software key, since I’m using a Zymkey.

Don’t be surprised if this step takes a while. What it’s doing is making a complete copy of the files on the running disk, and signing it with the hardware key that it has generated


```bash
     Created signing key
     Created '/etc/zymbit/zboot/update_artifacts/file_manifest'
     Created '/etc/zymbit/zboot/update_artifacts/file_deletions'
    Verified path unmounted '/etc/zymbit/zboot/mnt'
     Cleaned '/etc/zymbit/zboot/mnt'
     Deleted '/etc/crypttab'
    Verified disk size (required: 2.33 GiB, free: 26.39 GiB)
     Created initramfs
     Created snapshot of boot (/etc/zymbit/zboot/update_artifacts/tmp/.tmpBgEBJk/z-image-1_boot.tar)
     Created snapshot of root (/etc/zymbit/zboot/update_artifacts/tmp/.tmpBgEBJk/z-image-1_rfs.tar)
     Created '/mnt/tmp'
     Cleaned '/mnt/tmp'
     Created staging directory (/mnt/tmp/.tmpEhjNN7)
     Created '/mnt/tmp/.tmpEhjNN7/header.txt'
     Created tarball (/mnt/tmp/.tmpEhjNN7/update_artifact.tar)
     Created header signature
     Created update artifact signature
     Created file manifest signature
     Created file deletions signature
     Created '/mnt/tmp/.tmpEhjNN7/signatures'
     Created signatures (/mnt/tmp/.tmpEhjNN7/signatures)
      Copied file (/etc/zymbit/zboot/update_artifacts/file_manifest) to (/mnt/tmp/.tmpEhjNN7/file_manifest)
      Copied file (/etc/zymbit/zboot/update_artifacts/file_deletions) to (/mnt/tmp/.tmpEhjNN7/file_deletions)
     Created tarball (/mnt/z-image-1.zi)
     Created '/mnt/z-image-1_private_key.pem'
       Saved private key '/mnt/z-image-1_private_key.pem'
     Created '/mnt/z-image-1_pub_key.pem'
       Saved public key '/mnt/z-image-1_pub_key.pem'
     Cleaned '/mnt/tmp'
       Saved image '/mnt/z-image-1.zi' (2.33 GiB)
    Finished in 384.8s
```

The public/private keypair is saved on the USB Drive, and we will need it later.

If you remember from the [beginning](/chapter1/page2), we will be creating A/B partitions

Bootware encrypts the A, B, and DATA partitions. The A and B partition are locked with unique LUKS keys, meaning you cannot access the Backup partition from the Active partition. The encrypted DATA partition is accessible from either the A or B partition.

Setting up this A/B partitioning scheme is usually quite cumbersome and difficult to implement. Zymbit’s Bootware has taken that process and simplified it such that it’s a relatively easy process. So let’s go through that process now and make your Pi both secure and resilient.

### Create A/B partitions

Since we've not previously had a backup B partition, we will create one, and we will place the current image (which we know is good, since we're currently running it) into that partition. To do that, we will update the configuration (really create it) with the `zbcli` tool.

```bash
$ sudo zbcli update-config
```
```
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	---------
        Info the root file system will be re-partitioned with your chosen configuration.
```

This process will ask you some questions to determine how to lay out your partitions. The first is what device partition layout you would like to use. Choose the recommended option:
```bash
? Select device partition layout after an update ›
❯   [RECOMMENDED] A/B: This will take the remaining disk space available after the boot partition and create two encrypted partitions, each taking up half of the remaining space. Most useful for rollback and reco
       Using partition layout (A/B)
        Info the root file system will be re-partitioned with your chosen configuration.
```
Next you will select the update policy. Again, just choose the recommended one.

```bash
? Select update policy ›
❯   [RECOMMENDED] BACKUP: Applies new updates to current backup filesystem and swap to booting the new updated backup partition as the active partition now. If the new update is bad, it will rollback into the pre
     Running [========================================] 2/1 (00:00:17):                                                                                                                                             WARNING! Detected active partition (28.71GB) is larger than 14.86GB needed for two filesystems.
 Active partition won't be saved!!!
 Changing update mode to UPDATE_BOTH!!!
       Using update mode (UPDATE_BOTH)
        Data partition size currently set to: 512 MB
        Info bootware will create a shared data partition after A/B in size MB specified
```

Next, you can select the size of the data partition. It defaults to 512MB, but I suggest increasing that to 1024MB.

```bash
✔ Enter size of data partition in MB · 1024
       Using Data Partition Size 1024MB
  Defaulting to configured endpoint '/dev/sda1'
        Info update endpoints can be either an HTTPS URL or an external mass storage device like a USB stick.
       Found update name 'z-image-1'
       Saved update name 'z-image-1'
       Using update endpoint '/dev/sda1'
Configuration settings saved
    Finished in 42.1s
```

We've now got a system that is configured to have A/B partitioning, and to apply updates to the backup partition when they are available.

To complete the process, we will actually apply the update (which is really just a copy of the currently running system). This will trigger the re-partitioning and a reboot.

{{< pagenav prev="../page3" next="../page5/" >}}