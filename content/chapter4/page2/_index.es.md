---
title:  'Preparación para Bootware™'
date: 2024-10-25T12:49:09-04:00
weight: 2
reading_time: 3 minutes
---

## Finalmente, haciéndolo seguro

Ahora que tenemos un dispositivo de seguridad adecuado instalado, probado y listo, vamos a protegerlo. Al mismo tiempo, asegurémonos de que podamos actualizar el dispositivo de forma segura cuando llegue el momento y de que esté diseñado para poder recuperarse en caso de que falle una actualización.

Normalmente esto sería muchísimo trabajo, pero vamos a simplificar todo y hacerlo prácticamente todo de una vez.

### Un lugar para colocar la imagen de respaldo

Dado que utilizaremos Bootware(r) para proteger nuestro dispositivo, necesitaremos un lugar donde el sistema pueda copiar toda la tarjeta SD mientras la encripta. Para ello, utilizaremos una unidad USB.

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

Ahora debería tener un sistema de archivos adecuado en la unidad USB, que utilizaremos como espacio para crear nuestra imagen de arranque segura a continuación.

> [!WARNING]
> **Usuarios de Windows**
>
> Hemos creado un sistema de archivos ext4 en la unidad USB. Windows no puede montar ni leer sistemas de archivos ext4, por lo que no podrá utilizar la unidad USB desde un sistema Windows sin formatear la unidad (y borrar todos los datos que contiene).
>
{{< pagenav prev="../page1" next="../page3/" >}}
