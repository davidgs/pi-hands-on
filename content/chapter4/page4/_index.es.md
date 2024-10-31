---
title:  Arranque seguro
date: 2024-10-25T13:12:15-04:00
weight: 4
---

### Configuración del software de arranque

¡Aquí es donde comienza la verdadera diversión! Si alguna vez ha utilizado LUKS para cifrar un sistema de archivos de Pi, sabrá que, si bien es un gran paso para proteger su Pi, aún debe almacenar esa clave de cifrado en algún lugar al que se pueda acceder en el momento del arranque.

Con Bootware y un HSM Zymbit, la clave de cifrado LUKS está bloqueada por el HSM Zymbit, lo que la hace mucho más segura. Bootware espera que la imagen de arranque esté en un formato específico y cifrado llamado z-image. La herramienta CLI de Bootware le ayuda a crear y administrar estas imágenes para implementarlas en toda su empresa.

Así que vamos a crear nuestra primera imagen z y utilizaremos el sistema actual como base para ella.

Primero, necesitamos montar la unidad USB para que tengamos un lugar donde colocar nuestra imagen z:

```bash
sudo mount /dev/sda1 /mnt
```

A continuación, ejecutaremos la herramienta de imágenes para crear una imagen z encriptada de nuestro sistema actual:

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

Tenga en cuenta que utilicé el punto de montaje de la unidad USB como directorio de salida. Luego, elegí un nombre y un número de versión para la imagen y opté por utilizar una clave de software, ya que estoy utilizando una Zymkey.

No te sorprendas si este paso tarda un poco. Lo que hace es realizar una copia completa de los archivos del disco en ejecución y firmarla con la clave de hardware que ha generado.


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

El par de claves pública/privada se guarda en la unidad USB y lo necesitaremos más adelante.

Si recuerdas desde el [principio](/capítulo1/página2), crearemos particiones A/B

El software de arranque cifra las particiones A, B y DATA. Las particiones A y B están bloqueadas con claves LUKS únicas, lo que significa que no se puede acceder a la partición de respaldo desde la partición activa. Se puede acceder a la partición DATA cifrada desde la partición A o B.

Configurar este esquema de particionamiento A/B suele ser bastante complicado y difícil de implementar. El software de arranque de Zymbit ha tomado ese proceso y lo ha simplificado de manera que es un proceso relativamente fácil. Así que repasemos ese proceso ahora y hagamos que su Pi sea seguro y resistente.

### Crear particiones A/B

Como no teníamos una partición B de respaldo anteriormente, crearemos una y colocaremos la imagen actual (que sabemos que es buena, ya que la estamos ejecutando actualmente) en esa partición. Para ello, actualizaremos la configuración (en realidad, la crearemos) con la herramienta `zbcli`.

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

Este proceso le hará algunas preguntas para determinar cómo distribuir sus particiones. La primera es qué distribución de partición de dispositivo desea utilizar. Elija la opción recomendada:
```bash
? Select device partition layout after an update ›
❯   [RECOMMENDED] A/B: This will take the remaining disk space available after the boot partition and create two encrypted partitions, each taking up half of the remaining space. Most useful for rollback and reco
       Using partition layout (A/B)
        Info the root file system will be re-partitioned with your chosen configuration.
```
A continuación, seleccionará la política de actualización. Nuevamente, elija la recomendada.

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

A continuación, puede seleccionar el tamaño de la partición de datos. El valor predeterminado es 512 MB, pero le sugiero que lo aumente a 1024 MB.

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

Ahora tenemos un sistema que está configurado para tener particiones A/B y para aplicar actualizaciones a la partición de respaldo cuando estén disponibles.

> [!TIP]
> **Registros**
>
> Los mensajes de Bootware se registran en `/boot/firmware/zboot.log`, por lo que si está interesado en ver qué está haciendo Bootware, puede iniciar sesión allí.
>

Para completar el proceso, aplicaremos la actualización (que en realidad es solo una copia del sistema que se está ejecutando actualmente). Esto activará la nueva partición y el reinicio.

{{< pagenav prev="../page3" next="../page5/" >}}
