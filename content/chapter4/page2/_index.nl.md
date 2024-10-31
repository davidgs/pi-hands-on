---
title:  'Voorbereiden op Bootware™'
date: 2024-10-25T12:49:09-04:00
weight: 2
reading_time: 3 minutes
---

## Ten slotte, het veilig maken

Nu we een goed beveiligingsapparaat hebben geïnstalleerd, getest en gereed, gaan we dit ding beveiligen. Laten we er tegelijkertijd voor zorgen dat we het apparaat veilig kunnen updaten als de tijd daar is, en dat het is gebouwd om te herstellen als een update mislukt.

Normaal gesproken zou dit een hoop werk zijn, maar we gaan alles vereenvoudigen en het in één keer doen.

### Een plek om de back-upimage te plaatsen

Omdat we Bootware(r) gaan gebruiken om ons apparaat te beveiligen, hebben we een plek nodig waar het systeem de hele SD-kaart kan kopiëren terwijl het deze versleutelt. Hiervoor gaan we een USB-stick gebruiken.

We moeten ervoor zorgen dat we onze USB-stick goed kunnen gebruiken. Ik hergebruik ze vaak voor andere taken, dus dit is hoe ik graag begin. Nadat ik de USB-stick heb aangesloten, zorg ik ervoor dat ik de drive "op nul zet" en maak ik er een gloednieuwe partitiemap en bestandssysteem op.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=512 count=1 conv=notrunc
```
```bash
1+0 records in
1+0 records out
512 bytes copied, 0.0197125 s, 26.0 kB/s
```

Hiermee wordt het vorige bestandssysteem gewist, indien aanwezig.

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

De belangrijke onderdelen zijn dat als je eenmaal `sudo fdisk -W always /dev/sda` hebt ingevoerd, je `n` invoert om een nieuwe partitiemap te maken. Dan `p` om er een primaire partitie van te maken en ten slotte `w` om de partitiemap naar de schijf te schrijven. Voor al het andere accepteer ik gewoon de standaardinstellingen zoals gepresenteerd.

Nu we een gepartitioneerde USB-stick hebben, moeten we er een passend bestandssysteem op aanmaken.

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

U zou nu een geschikt bestandssysteem op de USB-stick moeten hebben. Deze gaan we vervolgens gebruiken als opslagruimte voor het maken van een veilige opstartimage.

> [!WARNING]
> **Windows-gebruikers**
>
> We hebben een ext4-bestandssysteem op de USB-drive gemaakt. Windows kan ext4-bestandssystemen niet mounten of lezen, dus u kunt de USB-drive niet gebruiken vanaf een Windows-systeem zonder de drive opnieuw te formatteren (en alle gegevens erop te wissen).
>
{{< pagenav prev="../page1" next="../page3/" >}}
