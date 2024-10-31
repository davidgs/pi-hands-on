---
title:  'Bootware™ installeren'
date: 2024-10-25T12:53:24-04:00
weight: 3
---

### Een plek om de back-upimage te plaatsen

Omdat we Bootware™ gebruiken om ons apparaat te beveiligen, hebben we een plek nodig waar het systeem de hele SD-kaart kan kopiëren terwijl het deze versleutelt. Hiervoor gebruiken we een USB-stick.

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

> [!NOTE]
> Als je, net als ik, het zat bent om steeds `sudo` te typen, kun je `sudo -i` één keer uitvoeren en een root-prompt krijgen van waaruit je al je commando's kunt uitvoeren. Maar vergeet niet, met grote macht komt grote verantwoordelijkheid!

### Bootware™ installeren

Bootware is de Zymbit-tool voor het beveiligen en updaten van uw Raspberry Pi. Het is een krachtige tool waarmee u één of een hele vloot Pi's in uw hele onderneming kunt updaten. En het stelt u in staat om dit veilig, beveiligd en op een manier te doen die herstelbaar is als er iets misgaat.

Eerst moeten we het installatieprogramma uitvoeren

```bash
curl -sSf https://raw.githubusercontent.com/zymbit-applications/zb-bin/main/install.sh | sudo bash
```

Deze installer zal u een paar simpele vragen stellen, dus laten we de antwoorden doornemen. De eerste is of u Hardware Signing wilt opnemen. Als u een HSM6- of SCM-gebaseerd product hebt, kunt u ja antwoorden op deze vraag. Als u een Zymkey of HSM4 hebt, wordt Hardware Signing niet ondersteund, dus hoeft u het niet te installeren. Zelfs met software signing worden uw uiteindelijke LUKS-gecodeerde partities beschermd door de Zymbit HSM-sleutels.

Vervolgens wordt u gevraagd welke versie van Bootware u wilt installeren. Kies de meest recente versie.

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

Nu het installatieprogramma klaar is, is het tijd om Bootware™ zelf te installeren:

```bash
sudo zbcli install
```

Zodra het installatieprogramma klaar is, wordt u gevraagd of u klaar bent om opnieuw op te starten:

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

Na het opnieuw opstarten zou Bootware succesvol geïnstalleerd moeten zijn. Nu kunnen we doorgaan met het maken van beveiligde partities en het voorbereiden op beveiligde updates.

{{< pagenav prev="../page2" next="../page4/" >}}
