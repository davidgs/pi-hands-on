---
title:  'Particionado para actualizaciones'
date: 2024-10-24T12:23:03-04:00
weight: 2
reading_time: 2 minutes
---

## Particionado A/B

Probablemente sea apropiado dar algunos antecedentes al respecto. La idea de la partición A/B es un concepto importante para la capacidad de recuperación. Si tiene una única partición de disco desde la que se inician sus dispositivos y actualiza elementos críticos en esa partición que de alguna manera están dañados, su dispositivo puede quedar en un estado en el que sea imposible iniciarlo o recuperarlo. Está bloqueado. La única forma de recuperar un dispositivo de este tipo es acceder físicamente al dispositivo y realizar cambios directos en la tarjeta SD. Esto no siempre es práctico o incluso posible.

Con la partición A/B, crea particiones de arranque duales y solo ejecuta desde una. Esa es la partición que se sabe que funciona bien o la partición primaria. Luego, tiene una partición secundaria donde puede aplicar actualizaciones. Una vez que se aplica una actualización a la partición secundaria, el dispositivo se reinicia desde esa partición recién actualizada. Si la actualización se realiza correctamente, su sistema vuelve a funcionar y esa partición se marca como la partición primaria y se reiniciará desde esa partición que se sabe que funciona bien a partir de ahora.

Si la actualización falla por algún motivo y el dispositivo no puede iniciarse correctamente desde la partición actualizada, el sistema se reinicia desde la partición primaria utilizada anteriormente y puede continuar ejecutándose hasta que se pueda implementar una actualización reparada.

![Particionado A/B](images/AB-part.png)

Con este esquema de partición implementado, es mucho menos probable que su Pi termine bloqueado ya que puede mantener una partición en buen estado en todo momento desde la cual arrancar.

El software de arranque cifra las particiones A, B y DATA. Las particiones A y B están bloqueadas con claves LUKS únicas, lo que significa que no se puede acceder a la partición de respaldo desde la partición activa. Se puede acceder a la partición DATA cifrada desde la partición A o B.

Configurar este esquema de particionamiento A/B suele ser bastante complicado y difícil de implementar. El software de arranque de Zymbit ha tomado ese proceso y lo ha simplificado de manera que es un proceso relativamente fácil. Así que repasemos ese proceso ahora y hagamos que su Pi sea seguro y resistente.

{{< pagenav prev="../page1" next="../../chapter2" >}}
