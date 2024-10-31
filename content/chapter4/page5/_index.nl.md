---
title:  'Beveiligd opstarten inschakelen'
date: 2024-10-25T13:18:46-04:00
weight: 5
---

## Veilig opstarten instellen

Voordat we de partities kunnen instellen, moeten we ervoor zorgen dat we de sleutels hebben om de bootimage te decoderen die we in de vorige stap hebben gemaakt. Die sleutels zijn opgeslagen op de USB-stick, dus kopieer de openbare sleutel naar de lokale directory:

```bash
sudo mount /dev/sda1 /mnt
cp /mnt/z-image-1_pub_key.pem .
```

Nu we de sleutel bij de hand hebben, is het tijd om Bootware de beveiligde partities voor ons te laten maken.

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

Wanneer u gevraagd wordt om opnieuw op te starten, zegt u ja en wacht u vervolgens.

Nadat uw Pi opnieuw is opgestart, logt u in en controleert u of alles correct is.

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

Merk op dat we nu twee `cryptfs`-apparaten hebben. Dit zijn volledig ondertekende en gecodeerde bestandssystemen.

Wat als de update mislukt was? Dit is het mooie van A/B-partitionering met Bootware: als het systeem niet opstart (het bereikt een `systemd init`-doel 3 keer achter elkaar niet), zal Bootware teruggaan naar de bekende goede partitie, waardoor uw apparaat weer online komt.

### De wijzigingen verifiëren

We hebben A/B-partities gemaakt en geverifieerd dat ze er allebei zijn en dat we uit de actieve (A) partitie zijn geboot. Als onderdeel van de Bootware-initialisatie die we zojuist hebben uitgevoerd, is dezelfde image ook op de B-partitie geïnstalleerd. We kunnen dit verifiëren door

```bash
zbcli rollback-swap
```

Dit zal een herstart van je Pi veroorzaken, maar wanneer je weer inlogt, voer je het volgende uit:

```bash
lsblk
```

Controleer nogmaals welke bestandssystemen nu beschikbaar zijn.


### Gefeliciteerd

U beschikt nu over een Raspberry Pi met een veilig mechanisme voor het ondertekenen van gegevens en voor het maken en installeren van veilige opstartimages!

{{< pagenav prev="../page4" next="../../chapter5/" >}}
