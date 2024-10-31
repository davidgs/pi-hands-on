---
title:  ""
type: "home"
reading_time: 2 minutes
---

## Willkommen in der Werkstatt

### Übersicht

Wir werden einen Raspberry Pi (Modell 4B) von Grund auf neu bauen und ihn dann mit einem [Zymkey](https://zymbit.com/zymkey) sichern, indem wir das Root-Dateisystem „rootfs“ verschlüsseln. Wir werden [Bootware](https://zymbit.com/bootware) verwenden, um eine neue Partitionszuordnung zu erstellen.

Wir werden außerdem die A/B-Partitionierung aktivieren, um Updates im Falle eines fehlerhaften Updates widerstandsfähig, zuverlässig und wiederherstellbar zu machen.

### Dieses Tutorial wird geladen

Wir empfehlen Ihnen, dieses Tutorial auf Ihren Laptop zu laden, um die Dinge einfacher zu machen (wie etwa die Möglichkeit, Befehle zu kopieren/einzufügen), indem Sie zu [https://davidgs.com/zymbit-workshop](https://davidgs.com/zymbit-workshop) gehen.

### Tagesordnung

- Ein kurzer Überblick über die Themen dieses Workshops
- Einige Hintergrundinformationen zu einigen der Begriffe und Lösungen, die wir verwenden werden
- Baubeschläge
- Installieren von Software
- Software konfigurieren
- Testen unserer Konfiguration
- Senden Sie einige sichere Nachrichten

### Verwenden der Befehlszeile

In diesem Workshop geht es fast ausschließlich um die Interaktion mit Ihrem Raspberry Pi über die Befehlszeilenschnittstelle (CLI). Wenn Sie mit der Verwendung der CLI nicht vertraut sind, ist dies eine Gelegenheit, mehr über dieses leistungsstarke Tool zu erfahren.

> [!TIP]
> **Windows-Benutzer**
>
> Die beste Lösung für Windows besteht darin, sicherzustellen, dass WSL installiert und aktiviert ist. Dadurch erhalten Sie Zugriff auf eine richtige Befehlszeile, die Sie für „ssh“ usw. verwenden können. Informationen zur Installation von WSL finden Sie in den [hier bereitgestellten] Anweisungen (https://davidgs.com/zymbit-workshop/index.html).

> [!TIP]
> **Mac-Benutzer**
>
> Sie haben bereits eine geeignete Terminalanwendung zur Verfügung. Gehen Sie einfach zu Ihrem Anwendungsordner, dann zum Ordner „Dienstprogramme“ und suchen Sie nach Terminal.app. Sie können auch andere Terminalprogramme installieren, wenn Sie möchten.

> [!TIP]
> **Linux-Benutzer**
>
> Mit Ihrer Linux-Distribution stehen Ihnen native Terminals zur Verfügung. Suchen Sie nach den Anwendungen „Terminal“ oder „XTerm“.

### Hauswirtschaft

Während wir diesen Workshop durchgehen, werden Sie einige Dinge auf unterschiedliche Weise hervorheben sehen. Auf diese Dinge sollten Sie besonders achten.

> [!CAUTION]
> Gibt Hinweise zu Risiken oder negativen Folgen bestimmter Handlungen.

> [!IMPORTANT]
> Wichtige Informationen, die Benutzer kennen müssen, um ihr Ziel zu erreichen.

> [!NOTE]
> Nützliche Informationen, die Benutzer kennen sollten, auch wenn sie den Inhalt nur überfliegen.

> [!TIP]
> Hilfreiche Ratschläge, um Dinge besser oder einfacher zu machen.

> [!WARNING]
> Dringende Informationen, die die sofortige Aufmerksamkeit des Benutzers erfordern, um Probleme zu vermeiden.

## Lasst uns anfangen!

{{< pagenav next="chapter1/" >}}
