---
title:  'Instalación de Bootware™'
date: 2024-10-25T12:53:24-04:00
weight: 3
---

### Un lugar para colocar la imagen de respaldo

Dado que utilizaremos Bootware™ para proteger nuestro dispositivo, necesitaremos un lugar donde el sistema pueda copiar toda la tarjeta SD mientras la encripta. Para ello, utilizaremos una unidad USB.

Necesitamos asegurarnos de que podemos usar nuestra unidad USB correctamente. A menudo las reutilizo para otras tareas, así que así es como me gusta empezar. Después de conectar la unidad USB, me aseguro de "poner a cero" la unidad y luego creo un nuevo mapa de partición y sistema de archivos en ella.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=512 count=1 conv=notrunc
```
```bash
1+0 records in
1+0 records out
512 bytes copied, 0.0197125 s, 26.0 kB/s
```

Esto borra el sistema de archivos anterior, si lo hubiera.

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

Las partes importantes son las siguientes: una vez que haya ingresado `sudo fdisk -W always /dev/sda`, ingresará `n` para crear un nuevo mapa de particiones. Luego, `p` para convertirlo en una partición primaria y, finalmente, `w` para escribir el mapa de particiones en el disco. Para todo lo demás, simplemente acepto los valores predeterminados tal como se presentan.

Finalmente, ahora que tenemos una unidad USB particionada, tenemos que crear un sistema de archivos adecuado en ella.

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
> Si, como yo, te cansas de escribir `sudo` todo el tiempo, puedes ejecutar `sudo -i` una vez y obtener un símbolo del sistema desde el que ejecutar todos tus comandos. Pero recuerda, ¡un gran poder conlleva una gran responsabilidad!

### Instalación de Bootware™

Bootware es la herramienta de Zymbit para proteger y actualizar su Raspberry Pi. Es una herramienta poderosa que le permite actualizar una o varias Pi de su empresa. Además, le permite hacerlo de manera segura y recuperable si algo sale mal.

Primero, tenemos que ejecutar el instalador.

```bash
curl -sSf https://raw.githubusercontent.com/zymbit-applications/zb-bin/main/install.sh | sudo bash
```

Este instalador le hará un par de preguntas sencillas, así que veamos las respuestas. La primera es si desea o no incluir Firma de hardware. Si tiene un producto basado en HSM6 o SCM, puede responder que sí a esta pregunta. Si tiene una Zymkey o HSM4, no se admite la Firma de hardware, por lo que no necesita instalarla. Incluso con la firma de software, sus particiones cifradas LUKS finales estarán protegidas por las claves HSM de Zymbit.

A continuación, te preguntará qué versión de Bootware quieres instalar. Elige la versión más reciente.

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

Ahora que el instalador está listo, es hora de instalar Bootware™:

```bash
sudo zbcli install
```

El instalador le preguntará si está listo para reiniciar cuando haya terminado:

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

Después de reiniciar, el software de arranque debería instalarse correctamente. Ahora podemos continuar con la creación de particiones seguras y la preparación para actualizaciones seguras.

{{< pagenav prev="../page2" next="../page4/" >}}
