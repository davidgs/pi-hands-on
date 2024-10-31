---
title:  'Configuración de Pi'
date: 2024-10-25T08:53:52-04:00
weight: 3
reading_time: 2 minutes
---

¡Ahora es el momento de conectar el Pi a la fuente de alimentación, esperar a que arranque y comenzar a configurar nuestra seguridad!

Primero, iniciemos sesión en su Pi. ¿Recuerda el nombre de host que le proporcionó mientras [Configuraba su tarjeta SD](capítulo 3/página 2/)? Lo necesitará, y el nombre de usuario y la contraseña que estableció. También necesitará una aplicación de terminal.

> [!IMPORTANT]
> En macOS, puedes ir a Aplicaciones –> Utilidades –> Terminal.app
> En Linux, la aplicación XTerm o Terminal funcionará
> En Windows, necesitará [Putty](https://www.putty.org) o una aplicación de Terminal similar

> [!TIP]
> **Usuarios de Windows**
>
> La mejor solución para Windows es asegurarse de tener WSL instalado y activo. Esto le dará acceso a una línea de comandos adecuada que puede usar para `ssh`, etc. Para instalar WSL, consulte las instrucciones proporcionadas [aquí](https://davidgs.com/zymbit-workshop/index.html)


```bash
ssh <username>@<hostname.local>
```
Luego, ingrese la contraseña que estableció anteriormente. Como llamé a mi Pi `zymbit-pi` y establecí el nombre de usuario y la contraseña en `zymbit`, usaría

```bash
ssh zymbit@zymbit-pi.local
zymbit@zymbit-pi.local's password: <zymbit>
...
zymbit-pi:~  $
```
¡Y ahora estás conectado a tu Raspberry Pi!

Antes de poder configurar el Zymkey, debemos asegurarnos de que el Pi pueda comunicarse con él. El software del Zymkey se comunica con el dispositivo a través de I2C, por lo que debemos asegurarnos de que la interfaz I2C del Pi esté habilitada.

```bash
$ sudo raspi-config
```
Le lleva a la utilidad de configuración.

![Pantalla inicial de Raspi-config](images/interface-options.png)

Luego seleccionarás "Opciones de interfaz" y luego "I2C".

![Habilitar la interfaz I2C](images/enable-i2c.png)

Luego puedes salir y guardar raspi-config

![guardar cambios en la interfaz I2C](images/ic2-enabled1.png)

Ya no es necesario reiniciar tu Pi después de esto.

Una última cosa antes de continuar. Probablemente sea una buena idea asegurarse de que el sistema operativo esté completamente actualizado. Puede ejecutar los siguientes dos comandos:

```bash
sudo apt update
sudo apt upgrade
```

> [!TIP]
> Si te cansas de agregar `sudo` a todos estos comandos de administración, puedes ingresar
> ```golpe
> sudo -yo
> ```
> Esto te permitirá obtener un shell "root". Pero recuerda: ¡un gran poder conlleva una gran responsabilidad!

Luego querrás reiniciar tu Pi para aplicar todos los cambios.

```bash
sudo reboot
```

Y espera a que se complete el reinicio.

{{< pagenav prev="../page2" next="../../chapter4/" >}}
