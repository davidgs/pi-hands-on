---
title:  ""
type: "home"
reading_time: 2 minutes
---

## Bienvenido al taller

### Descripción general

Construiremos una Raspberry Pi (modelo 4B) desde cero y luego usaremos una [Zymkey](https://zymbit.com/zymkey) para protegerla cifrando el sistema de archivos raíz `rootfs`. Usaremos [Bootware](https://zymbit.com/bootware) para crear un nuevo mapa de particiones.

También habilitaremos la partición A/B para que las actualizaciones sean resistentes, confiables y recuperables en caso de una mala actualización.

### Cargando este tutorial

Se recomienda cargar este tutorial en su computadora portátil para facilitar las cosas (como la capacidad de copiar/pegar comandos) yendo a [https://davidgs.com/zymbit-workshop](https://davidgs.com/zymbit-workshop)

### Orden del día

- Una breve descripción general de lo que cubriremos en este taller.
- Algunos antecedentes sobre algunos de los términos y soluciones que utilizaremos
- Herrajes para construcción
- Instalación de software
- Configuración de software
- Probando nuestra configuración
- Envía algunos mensajes seguros

### Uso de la línea de comandos

Casi todo este taller implicará interactuar con su Raspberry Pi a través de la interfaz de línea de comandos (CLI). Si no está familiarizado con el uso de la CLI, esta será una oportunidad para aprender sobre esta poderosa herramienta.

> [!TIP]
> **Usuarios de Windows**
>
> La mejor solución para Windows es asegurarse de tener WSL instalado y activo. Esto le dará acceso a una línea de comandos adecuada que puede usar para `ssh`, etc. Para instalar WSL, consulte las instrucciones proporcionadas [aquí](https://davidgs.com/zymbit-workshop/index.html)

> [!TIP]
> **Usuarios de Mac**
>
> Ya tienes una aplicación de terminal adecuada disponible. Simplemente ve a la carpeta Aplicaciones, luego a la carpeta Utilidades y busca Terminal.app. También puedes instalar otros programas de terminal si lo prefieres.

> [!TIP]
> **Usuarios de Linux**
>
> Tienes terminales nativas disponibles con tu distribución Linux. Busca aplicaciones 'Terminal' o 'XTerm'.

### Tareas del hogar

A medida que avancemos en este taller, verás que se mencionan algunas cosas de diversas maneras. Son cosas a las que quiero que prestes especial atención.

> [!CAUTION]
> Informa sobre los riesgos o resultados negativos de determinadas acciones.

> [!IMPORTANT]
> Información clave que los usuarios necesitan saber para lograr su objetivo.

> [!NOTE]
> Información útil que los usuarios deben saber, incluso cuando hojean el contenido.

> [!TIP]
> Consejos útiles para hacer las cosas mejor o más fácilmente.

> [!WARNING]
> Información urgente que necesita la atención inmediata del usuario para evitar problemas.

## ¡Empecemos!

{{< pagenav next="chapter1/" >}}
