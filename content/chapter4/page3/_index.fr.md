---
title:  « Installation de Bootware™ »
date: 2024-10-25T12:53:24-04:00
weight: 3
---

### Un endroit où placer l'image de sauvegarde

Comme nous allons utiliser Bootware™ pour sécuriser notre appareil, nous aurons besoin d'un emplacement pour que le système copie l'intégralité de la carte SD pendant qu'il la crypte. Pour cela, nous allons utiliser une clé USB.

Nous devons nous assurer que nous pouvons utiliser correctement notre clé USB. Je les réutilise souvent pour d'autres tâches, alors voici comment j'aime commencer. Après avoir branché la clé USB, je m'assure de la « mettre à zéro », puis de créer une toute nouvelle carte de partition et un nouveau système de fichiers dessus.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=512 count=1 conv=notrunc
```
```bash
1+0 records in
1+0 records out
512 bytes copied, 0.0197125 s, 26.0 kB/s
```

Cela efface le système de fichiers précédent, le cas échéant.

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

Les parties importantes sont les suivantes : une fois que vous avez entré la commande « sudo fdisk -W always /dev/sda », vous devez entrer « n » pour créer une nouvelle table de partition. Ensuite, « p » pour en faire une partition primaire et enfin « w » pour écrire la table de partition sur le disque. Pour tout le reste, j'accepte simplement les valeurs par défaut telles qu'elles sont présentées.

Enfin, maintenant que nous avons une clé USB partitionnée, nous devons créer un système de fichiers approprié dessus.

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
> Si, comme moi, vous en avez assez de taper « sudo » tout le temps, vous pouvez exécuter « sudo -i » une fois et obtenir une invite root à partir de laquelle exécuter toutes vos commandes. Mais n'oubliez pas qu'un grand pouvoir implique de grandes responsabilités !

### Installation de Bootware™

Bootware est l'outil Zymbit pour sécuriser et mettre à jour votre Raspberry Pi. C'est un outil puissant qui vous permet de mettre à jour un ou plusieurs Pis dans toute votre entreprise. Et il vous permet de le faire en toute sécurité, de manière sécurisée et de manière récupérable en cas de problème.

Tout d’abord, nous devons exécuter le programme d’installation

```bash
curl -sSf https://raw.githubusercontent.com/zymbit-applications/zb-bin/main/install.sh | sudo bash
```

Ce programme d'installation vous posera quelques questions simples. Voyons donc les réponses. La première consiste à savoir si vous souhaitez ou non inclure la signature matérielle. Si vous possédez un produit basé sur HSM6 ou SCM, vous pouvez répondre oui à cette question. Si vous possédez une clé Zymkey ou HSM4, la signature matérielle n'est pas prise en charge, vous n'avez donc pas besoin de l'installer. Même avec la signature logicielle, vos partitions chiffrées LUKS finales seront protégées par les clés HSM Zymbit.

Ensuite, il vous sera demandé quelle version de Bootware installer. Choisissez la version la plus récente.

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

Maintenant que le programme d'installation est prêt, il est temps d'installer Bootware™ lui-même :

```bash
sudo zbcli install
```

Une fois l'installation terminée, le programme d'installation vous demandera si vous êtes prêt à redémarrer :

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

Après le redémarrage, Bootware devrait être correctement installé. Nous pouvons maintenant passer à la création de partitions sécurisées et à la préparation des mises à jour sécurisées.

{{< pagenav prev="../page2" next="../page4/" >}}
