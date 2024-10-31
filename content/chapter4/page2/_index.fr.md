---
title:  « Préparation pour Bootware™ »
date: 2024-10-25T12:49:09-04:00
weight: 2
reading_time: 3 minutes
---

## Enfin, le sécuriser

Maintenant que nous avons installé, testé et préparé un dispositif de sécurité adéquat, sécurisons-le. En même temps, assurons-nous que nous pouvons mettre à jour le dispositif en toute sécurité le moment venu et qu'il est conçu pour être récupérable en cas d'échec d'une mise à jour.

Normalement, cela représenterait énormément de travail, mais nous allons tout simplifier et tout faire presque en même temps.

### Un endroit où placer l'image de sauvegarde

Comme nous allons utiliser Bootware(r) pour sécuriser notre appareil, nous aurons besoin d'un emplacement pour que le système copie l'intégralité de la carte SD pendant qu'il la crypte. Pour cela, nous allons utiliser une clé USB.

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

Vous devriez maintenant avoir un système de fichiers approprié sur la clé USB, que nous utiliserons comme espace pour créer notre image de démarrage sécurisé ensuite.

> [!WARNING]
> **Utilisateurs Windows**
>
> Nous avons créé un système de fichiers ext4 sur la clé USB. Windows ne peut pas monter ou lire les systèmes de fichiers ext4, vous ne pourrez donc pas utiliser la clé USB à partir d'un système Windows sans reformater le lecteur (et effacer toutes les données qu'il contient).
>
{{< pagenav prev="../page1" next="../page3/" >}}
