---
title:  „Sicheren Start aktivieren“
date: 2024-10-25T13:18:46-04:00
weight: 5
---

## Einrichten des sicheren Bootens

Bevor wir die Partitionen einrichten können, müssen wir sicherstellen, dass wir die Schlüssel zum Entschlüsseln des Boot-Images haben, das wir im vorherigen Schritt erstellt haben. Diese Schlüssel sind auf dem USB-Laufwerk gespeichert. Kopieren Sie daher den öffentlichen Schlüssel in das lokale Verzeichnis:

```bash
sudo mount /dev/sda1 /mnt
cp /mnt/z-image-1_pub_key.pem .
```

Nachdem wir nun den Schlüssel zur Hand haben, ist es an der Zeit, dass Bootware die sicheren Partitionen für uns erstellt.

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

Wenn Sie zum Neustart aufgefordert werden, sagen Sie „Ja“ und warten Sie.

Sobald Ihr Pi neu gestartet ist, melden Sie sich an und überprüfen Sie, ob alles korrekt ist.

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

Beachten Sie, dass wir jetzt zwei `cryptfs`-Geräte haben. Dies sind vollständig signierte und verschlüsselte Dateisysteme.

Was wäre, wenn das Update fehlgeschlagen wäre? Das ist das Schöne an der A/B-Partitionierung mit Bootware: Wenn das System nicht bootet (es erreicht dreimal hintereinander kein „systemd init“-Ziel), kehrt Bootware zur bekanntermaßen funktionierenden Partition zurück und bringt Ihr Gerät wieder online.

### Überprüfen der Änderungen

Wir haben A/B-Partitionen erstellt und überprüft, dass beide vorhanden sind und dass wir aus der aktiven (A) Partition gebootet haben. Im Rahmen der Bootware-Initialisierung, die wir gerade durchgeführt haben, wurde dasselbe Image auch auf der B-Partition installiert. Wir können dies überprüfen, indem wir ausführen:

```bash
zbcli rollback-swap
```

Dies löst einen Neustart Ihres Pi aus. Wenn Sie sich jedoch wieder anmelden, führen Sie Folgendes aus:

```bash
lsblk
```

Sehen Sie noch einmal nach, welche Dateisysteme Ihnen jetzt zur Verfügung stehen.


### Glückwunsch

Sie haben jetzt einen Raspberry Pi, der über einen sicheren Mechanismus zum Signieren von Daten und zum Erstellen und Installieren sicherer Boot-Images verfügt!

{{< pagenav prev="../page4" next="../../chapter5/" >}}
