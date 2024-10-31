---
title:  « Démarrage sécurisé »
date: 2024-10-25T13:12:15-04:00
weight: 4
---

### Configuration du logiciel de démarrage

C'est ici que le vrai plaisir commence ! Si vous avez déjà utilisé LUKS pour crypter un système de fichiers Pi, vous savez que, même si c'est une excellente étape pour sécuriser votre Pi, vous devez toujours stocker cette clé de cryptage dans un endroit accessible au démarrage.

Avec Bootware et un HSM Zymbit, la clé de chiffrement LUKS est verrouillée par le HSM Zymbit, ce qui la rend beaucoup plus sécurisée. Bootware s'attend à ce que l'image de démarrage soit dans un format chiffré spécifique appelé image z. L'outil CLI Bootware vous aide à créer et à gérer ces images pour les déployer dans toute votre entreprise.

Créons donc notre première image z et nous utiliserons le système actuel comme base.

Tout d’abord, nous devons monter la clé USB afin d’avoir un endroit où placer notre image z :

```bash
sudo mount /dev/sda1 /mnt
```

Ensuite, nous allons exécuter l’outil d’imagerie pour créer une image z chiffrée de notre système actuel :

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

Notez que j'ai utilisé le point de montage de la clé USB comme répertoire de sortie. J'ai ensuite choisi un nom et un numéro de version pour l'image et j'ai choisi d'utiliser une clé logicielle, puisque j'utilise une clé Zymkey.

Ne soyez pas surpris si cette étape prend un certain temps. Elle consiste à effectuer une copie complète des fichiers sur le disque en cours d'exécution et à la signer avec la clé matérielle qu'elle a générée.


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

La paire de clés publique/privée est enregistrée sur la clé USB et nous en aurons besoin plus tard.

Si vous vous souvenez du [début](/chapitre1/page2), nous allons créer des partitions A/B

Bootware chiffre les partitions A, B et DATA. Les partitions A et B sont verrouillées avec des clés LUKS uniques, ce qui signifie que vous ne pouvez pas accéder à la partition de sauvegarde à partir de la partition active. La partition DATA chiffrée est accessible à partir de la partition A ou B.

La configuration de ce schéma de partitionnement A/B est généralement assez lourde et difficile à mettre en œuvre. Bootware de Zymbit a repris ce processus et l'a simplifié de telle sorte qu'il soit relativement simple. Examinons donc ce processus maintenant et rendons votre Pi à la fois sûr et résilient.

### Créer des partitions A/B

Comme nous n'avons pas encore de partition B de sauvegarde, nous allons en créer une et y placer l'image actuelle (dont nous savons qu'elle est correcte, puisque nous l'exécutons actuellement). Pour ce faire, nous mettrons à jour la configuration (en fait, nous la créerons) avec l'outil `zbcli`.

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
	------reading_time: 5 minutes
---
        Info the root file system will be re-partitioned with your chosen configuration.
```

Ce processus vous posera quelques questions pour déterminer comment organiser vos partitions. La première concerne la disposition des partitions de périphériques que vous souhaitez utiliser. Choisissez l'option recommandée :
```bash
? Select device partition layout after an update ›
❯   [RECOMMENDED] A/B: This will take the remaining disk space available after the boot partition and create two encrypted partitions, each taking up half of the remaining space. Most useful for rollback and reco
       Using partition layout (A/B)
        Info the root file system will be re-partitioned with your chosen configuration.
```
Ensuite, vous devez sélectionner la politique de mise à jour. Là encore, choisissez simplement celle qui est recommandée.

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

Ensuite, vous pouvez sélectionner la taille de la partition de données. La valeur par défaut est de 512 Mo, mais je suggère de l'augmenter à 1 024 Mo.

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

Nous avons maintenant un système configuré pour avoir un partitionnement A/B et pour appliquer les mises à jour à la partition de sauvegarde lorsqu'elles sont disponibles.

> [!TIP]
> **Journaux**
>
> Les messages Bootware sont enregistrés dans `/boot/firmware/zboot.log` donc si vous souhaitez voir ce que fait Bootware, vous pouvez y accéder.
>

Pour terminer le processus, nous allons appliquer la mise à jour (qui n'est en fait qu'une copie du système en cours d'exécution). Cela déclenchera le repartitionnement et un redémarrage.

{{< pagenav prev="../page3" next="../page5/" >}}
