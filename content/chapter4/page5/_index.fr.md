---
title:  « Activation du démarrage sécurisé »
date: 2024-10-25T13:18:46-04:00
weight: 5
---

## Configuration du démarrage sécurisé

Avant de pouvoir configurer les partitions, nous devons nous assurer que nous disposons des clés pour décrypter l'image de démarrage que nous avons créée à l'étape précédente. Ces clés sont stockées sur la clé USB, copiez donc la clé publique dans le répertoire local :

```bash
sudo mount /dev/sda1 /mnt
cp /mnt/z-image-1_pub_key.pem .
```

Maintenant que nous avons la clé à portée de main, il est temps de demander à Bootware de créer les partitions sécurisées pour nous.

```bash
sudo zbcli update
```
```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	---------
     Cleaned '/etc/zymbit/zboot/update_artifacts/tmp'
       Found update configs
? Proceed with current configs? These can be modified through 'zbcli update-config'
	---------
 	Update endpoint   /dev/sda1
 	Update name       z-image-1
 	Endpoint type     LOCAL
 	Partition layout  A/B
 	Update policy     UPDATE_BOTH
 	------reading_time: 3 minutes
---
     Created temporary directory (/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c)
✔ Enter public key file (Pem format) · ./z-image-1_pub_key.pem
     Mounted '/dev/sda1' to '/etc/zymbit/zboot/update_artifacts/tmp/.tmpyKYgR3'
       Found image tarball (/etc/zymbit/zboot/update_artifacts/tmp/.tmpyKYgR3/z-image-1.zi)
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/update_artifact.tar'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/signatures'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/header.txt'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/file_manifest'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/file_deletions'
     Decoded header signature
     Decoded image signature
     Decoded manifest signature
     Decoded deletions signature
       Found header data
       Found image data
       Found manifest data
       Found file deletions data
    Verified header signature
    Verified image signature
    Verified manifest signature
    Verified file deletions signature
    Modified zbconfig 'public_key'
    Modified zbconfig 'new_update_needed'
    Modified zbconfig 'root_a'
    Modified zbconfig 'root_b'
    Modified zbconfig 'root_dev'
      Copied file (/boot/firmware/usr-kernel.enc) to (/boot/firmware/zboot_bkup/usr-kernel-A.enc)
      Copied file (/boot/firmware/kernel8.img) to (/boot/firmware/zboot_bkup/kernel8.img)
    Modified zbconfig 'update_with_new_image'
    Modified zbconfig 'kernel_filename'
? Scheduled update for the next reboot. Reboot now? (y/n) › yes
```

Lorsqu'il vous demande de redémarrer, répondez oui, puis attendez.

Une fois votre Pi redémarré, connectez-vous et vérifiez que tout est correct.

```bash
lsblk
NAME              MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINTS
sda                 8:0    1 59.8G  0 disk
└─sda1              8:1    1 59.8G  0 part
mmcblk0           179:0    0 29.7G  0 disk
├─mmcblk0p1       179:1    0  512M  0 part  /boot/firmware
├─mmcblk0p2       179:2    0 14.1G  0 part
│ └─cryptrfs_A    254:0    0 14.1G  0 crypt /
├─mmcblk0p3       179:3    0 14.1G  0 part
└─mmcblk0p4       179:4    0    1G  0 part
  └─cryptrfs_DATA 254:1    0 1008M  0 crypt
```

Notez que nous avons maintenant deux périphériques « cryptfs ». Il s'agit de systèmes de fichiers entièrement signés et chiffrés.

Que se passerait-il si la mise à jour avait échoué ? Voici la beauté du partitionnement A/B avec Bootware : si le système ne parvient pas à démarrer (il ne parvient pas à atteindre une cible « systemd init » 3 fois de suite), Bootware reviendra à la partition connue comme étant correcte, remettant votre appareil en ligne.

### Vérification des modifications

Nous avons créé des partitions A/B et avons vérifié qu'elles sont toutes les deux présentes et que nous avons démarré à partir de la partition active (A). Dans le cadre de l'initialisation du Bootware que nous venons d'effectuer, la même image a également été installée sur la partition B. Nous pouvons vérifier cela en exécutant

```bash
zbcli rollback-swap
```

Cela déclenchera un redémarrage de votre Pi, mais lorsque vous vous reconnecterez, exécutez

```bash
lsblk
```

Encore une fois, voyez quels systèmes de fichiers sont disponibles actuellement.


### Félicitations

Vous disposez désormais d’un Raspberry Pi doté d’un mécanisme sécurisé pour signer des données et pour créer et installer des images de démarrage sécurisées !

{{< pagenav prev="../page4" next="../../chapter5/" >}}
