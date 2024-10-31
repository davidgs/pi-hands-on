---
title:  'Bootware™ installieren'
date: 2024-10-25T12:53:24-04:00
weight: 3
---

### Ein Ort zum Ablegen des Sicherungsimages

Da wir Bootware™ verwenden, um unser Gerät zu sichern, benötigen wir einen Ort, an den das System die gesamte SD-Karte kopieren kann, während es sie verschlüsselt. Hierfür verwenden wir ein USB-Laufwerk.

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

> [!NOTE]
> Wenn Sie es wie ich leid sind, ständig `sudo` einzugeben, können Sie `sudo -i` einmal ausführen und erhalten eine Root-Eingabeaufforderung, von der aus Sie alle Ihre Befehle ausführen können. Aber denken Sie daran: Mit großer Macht geht große Verantwortung einher!

### Bootware™ installieren

Bootware ist das Zymbit-Tool zum Sichern und Aktualisieren Ihres Raspberry Pi. Es ist ein leistungsstarkes Tool, mit dem Sie einen oder eine ganze Pi-Flotte in Ihrem Unternehmen aktualisieren können. Und Sie können dies sicher und zuverlässig tun und auf eine Weise, die wiederherstellbar ist, wenn etwas schief geht.

Zuerst müssen wir das Installationsprogramm ausführen

```bash
curl -sSf https://raw.githubusercontent.com/zymbit-applications/zb-bin/main/install.sh | sudo bash
```

Dieses Installationsprogramm wird Ihnen ein paar einfache Fragen stellen, also gehen wir die Antworten durch. Die erste ist, ob Sie Hardware Signing einschließen möchten oder nicht. Wenn Sie ein HSM6- oder SCM-basiertes Produkt haben, können Sie diese Frage mit „Ja“ beantworten. Wenn Sie einen Zymkey oder HSM4 haben, wird Hardware Signing nicht unterstützt, Sie müssen es also nicht installieren. Selbst mit Software Signing werden Ihre endgültigen LUKS-verschlüsselten Partitionen durch die Zymbit-HSM-Schlüssel geschützt.

Als nächstes werden Sie gefragt, welche Bootware-Version installiert werden soll. Wählen Sie die aktuellste Version.

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

Nachdem das Installationsprogramm nun bereit ist, ist es Zeit, Bootware™ selbst zu installieren:

```bash
sudo zbcli install
```

Wenn das Installationsprogramm abgeschlossen ist, werden Sie gefragt, ob Sie zum Neustart bereit sind:

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

Nach dem Neustart sollte die Bootware erfolgreich installiert sein. Jetzt können wir mit der Erstellung sicherer Partitionen und der Vorbereitung sicherer Updates fortfahren.

{{< pagenav prev="../page2" next="../page4/" >}}
