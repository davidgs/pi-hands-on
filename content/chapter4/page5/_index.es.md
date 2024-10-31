---
title:  'Habilitar arranque seguro'
date: 2024-10-25T13:18:46-04:00
weight: 5
---

## Configuración del arranque seguro

Antes de poder configurar las particiones, debemos asegurarnos de que tenemos las claves para descifrar la imagen de arranque que creamos en el paso anterior. Esas claves se almacenan en la unidad USB, así que copia la clave pública en el directorio local:

```bash
sudo mount /dev/sda1 /mnt
cp /mnt/z-image-1_pub_key.pem .
```

Ahora que tenemos la clave a mano, es hora de que Bootware cree las particiones seguras para nosotros.

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

Cuando le pida reiniciar, diga que sí y luego espere.

Una vez que tu Pi se haya reiniciado, inicia sesión y verifica que todo esté correcto.

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

Observe que ahora tenemos dos dispositivos `cryptfs`. Estos son sistemas de archivos completamente firmados y cifrados.

¿Qué sucede si la actualización falla? Esta es la ventaja de la partición A/B con Bootware: si el sistema no arranca (no logra alcanzar un objetivo `systemd init` tres veces seguidas), Bootware volverá a la partición que funciona correctamente y hará que su dispositivo vuelva a estar en línea.

### Verificando los cambios

Hemos creado particiones A/B y hemos verificado que ambas están allí y que hemos arrancado desde la partición activa (A). Como parte de la inicialización del software de arranque que acabamos de realizar, la misma imagen también se instaló en la partición B. Podemos verificar esto ejecutando

```bash
zbcli rollback-swap
```

Esto provocará un reinicio de su Pi, pero cuando vuelva a iniciar sesión, ejecute

```bash
lsblk
```

Nuevamente mira qué sistemas de archivos tienes disponibles ahora.


### Felicitaciones

¡Ahora tienes una Raspberry Pi que tiene un mecanismo seguro para firmar datos y para crear e instalar imágenes de arranque seguras!

{{< pagenav prev="../page4" next="../../chapter5/" >}}
