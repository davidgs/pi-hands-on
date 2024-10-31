---
title:  'Creando una imagen'
date: 2024-10-24T14:48:01-04:00
weight: 2
reading_time: 2 minutes
---

## Iniciando Pi Imager

Cuando inicies Pi Imager por primera vez, verás que debes tomar algunas decisiones:

![Pantalla inicial de Pi Imager](images/pi-imager.png)

Primero, deberás elegir el modelo de Pi que tienes. Usaremos Pi 4

![Opciones de hardware de Pi](images/choose-hardware.png)

A continuación, deberás elegir el sistema operativo. Nosotros vamos a utilizar la versión más reciente (Bookworm, 64 bits), pero no necesitaremos el entorno de escritorio completo, así que elige la versión "Lite".

![Software Elija otro sistema operativo](images/choose-os-2.png)

![Software elegido Raspberry Pi Lite 64 bit Bookworm](images/choose-os-1.png)

A continuación, deberá identificar la tarjeta Micro SD en la que desea escribir. Si aún no lo ha hecho, inserte la tarjeta Micro SD en el dispositivo de escritura de tarjetas SD y conéctelo a su computadora.

![Seleccione el medio de la tarjeta SD](images/choose-media.png)

El último paso antes de escribir el sistema operativo en el disco es configurar cualquier configuración adicional que desees para el Pi. Te recomiendo que configures al menos un nombre de host y un nombre de usuario/contraseña y, si deseas utilizar tu red WiFi local, las credenciales de la red WiFi.



![Editar configuraciones adicionales](images/edit-settings.png)

Para este ejercicio, establezca lo siguiente:
- `hostname`: ¡Elige algo único! Todos estaremos en la misma LAN, así que elige algo único para esta LAN
- `username`: uso `zymbit` para el nombre de usuario, pero puedes elegir el que quieras
- `contraseña`: para facilitar su uso, también utilizo `zymbit` aquí, pero claramente no es seguro, así que elija cualquier contraseña que pueda recordar con seguridad.
- `SSID`: Utilizaremos un punto de acceso WiFi local, así que ingrese `zymbit-lab` aquí.
- `contraseña`: El WiFi `zymbit-lab` usa la contraseña `zymbit-lab-wifi` así que ingrésala aquí.
![Configurar parámetros adicionales como WiFi y nombre de usuario](images/customize.png)

Necesitaremos iniciar sesión a través de ssh, así que active ssh y permita inicios de sesión con contraseña.

![permitir inicios de sesión ssh](images/enable-ssh.png)

Una vez que hayas realizado todos los ajustes correctamente, es momento de escribirlos en la tarjeta. Ten en cuenta que esto borrará por completo todos los datos existentes en la tarjeta SD, así que ten cuidado.

![escribir datos en la tarjeta](images/Pi-warning.png)

Después de eso, puedes sentarte y disfrutar de una taza de café mientras se escribe el sistema operativo en la tarjeta. Una vez que esté listo, podemos pasar a configurar el hardware.

{{< pagenav prev="../page1" next="../page3/" >}}
