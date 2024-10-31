---
title:  „Vorbereitung auf Bootware™“
date: 2024-10-25T12:49:09-04:00
weight: 2
reading_time: 3 minutes
---

## Endlich Sicherheit

Nachdem wir nun ein geeignetes Sicherheitsgerät installiert, getestet und einsatzbereit haben, sichern wir es. Stellen wir gleichzeitig sicher, dass wir das Gerät bei Bedarf sicher aktualisieren können und dass es so konstruiert ist, dass es im Falle eines fehlgeschlagenen Updates wiederherstellbar ist.

Normalerweise wäre das eine Menge Arbeit, aber wir vereinfachen alles und erledigen praktisch alles auf einmal.

### Ein Ort zum Ablegen des Sicherungsimages

Da wir Bootware(r) verwenden, um unser Gerät zu sichern, benötigen wir einen Ort, an den das System die gesamte SD-Karte kopieren kann, während es sie verschlüsselt. Hierfür verwenden wir ein USB-Laufwerk.

Wir müssen sicherstellen, dass wir unser USB-Laufwerk richtig verwenden können. Ich verwende es oft für andere Aufgaben wieder, also beginne ich gerne wie folgt. Nachdem ich das USB-Laufwerk angeschlossen habe, stelle ich sicher, dass ich das Laufwerk „auf Null“ setze und dann eine brandneue Partitionszuordnung und ein neues Dateisystem darauf erstelle.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=512 count=1 conv=notrunc
```
```bash
1+0 records in
1+0 records out
512 bytes copied, 0.0197125 s, 26.0 kB/s
```

Dadurch wird das vorherige Dateisystem (sofern vorhanden) gelöscht.

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

Die wichtigen Teile sind: Nachdem Sie `sudo fdisk -W always /dev/sda` eingegeben haben, geben Sie `n` ein, um eine neue Partitionszuordnung zu erstellen. Dann `p`, um sie zu einer primären Partition zu machen, und schließlich `w`, um die Partitionszuordnung auf die Festplatte zu schreiben. Für alles andere akzeptiere ich einfach die angezeigten Standardeinstellungen.

Da wir nun ein partitioniertes USB-Laufwerk haben, müssen wir darauf schließlich ein richtiges Dateisystem erstellen.

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

Sie sollten jetzt über ein ordnungsgemäßes Dateisystem auf dem USB-Laufwerk verfügen, das wir als Nächstes als Speicherplatz für die Erstellung unseres sicheren Startimages verwenden werden.

> [!WARNING]
> **Windows-Benutzer**
>
> Wir haben auf dem USB-Laufwerk ein ext4-Dateisystem erstellt. Windows kann ext4-Dateisysteme weder mounten noch lesen, daher können Sie das USB-Laufwerk nicht von einem Windows-System aus verwenden, ohne das Laufwerk neu zu formatieren (und alle darauf gespeicherten Daten zu löschen).
>
{{< pagenav prev="../page1" next="../page3/" >}}
