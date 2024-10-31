---
title:  „Ein Bild schaffen“
date: 2024-10-24T14:48:01-04:00
weight: 2
reading_time: 2 minutes
---

## Pi Imager starten

Wenn Sie den Pi Imager zum ersten Mal starten, werden Sie feststellen, dass Sie einige Entscheidungen treffen müssen:

![Pi Imager-Startbildschirm](images/pi-imager.png)

Zuerst müssen Sie auswählen, welches Pi-Modell Sie haben. Wir verwenden Pi 4s

![Pi-Hardwareauswahl](images/choose-hardware.png)

Als Nächstes wählen Sie das Betriebssystem aus. Wir verwenden die neueste Version (Bookworm, 64-Bit), benötigen aber nicht die vollständige Desktop-Umgebung. Wählen Sie daher die Version „Lite“.

![Software Anderes Betriebssystem auswählen](images/choose-os-2.png)

![Softwareauswahl Raspberry Pi Lite 64 Bit Bookworm](images/choose-os-1.png)

Als Nächstes identifizieren Sie die Micro-SD-Karte, auf die Sie schreiben möchten. Wenn Sie dies noch nicht getan haben, legen Sie die Micro-SD-Karte in den SD-Kartenschreiber ein und schließen Sie ihn an Ihren Computer an.

![SD-Kartenmedium auswählen](images/choose-media.png)

Der letzte Schritt vor dem eigentlichen Schreiben des Betriebssystems auf die Festplatte besteht darin, alle weiteren Einstellungen vorzunehmen, die Sie für den Pi wünschen. Ich empfehle zumindest, einen Hostnamen und einen Benutzernamen/ein Passwort einzurichten, und wenn Sie Ihr lokales WLAN verwenden möchten, die WLAN-Anmeldeinformationen.



![Zusätzliche Einstellungen bearbeiten](images/edit-settings.png)

Legen Sie für diese Übung Folgendes fest:
- `hostname`: Wählen Sie etwas Einzigartiges! Wir werden alle im selben LAN sein, also wählen Sie etwas Einzigartiges für dieses LAN.
- `Benutzername`: Ich verwende `zymbit` als Benutzernamen, aber Sie können wählen, was Sie möchten
- `password`: Der Einfachheit halber verwende ich hier auch `zymbit`, aber das ist eindeutig nicht sicher, also wählen Sie ein Passwort, das Sie sich zuverlässig merken können
- „SSID“: Wir verwenden einen lokalen WLAN-Hotspot, geben Sie hier also „zymbit-lab“ ein.
- „Passwort“: Das WLAN „zymbit-lab“ verwendet das Passwort „zymbit-lab-wifi“, geben Sie es hier ein.
![Richten Sie zusätzliche UPI-Parameter wie WLAN und Benutzernamen ein](images/customize.png)

Wir müssen uns per SSH anmelden, also schalten Sie SSH ein und erlauben Sie Passwort-Logins

![SSH-Anmeldungen zulassen](images/enable-ssh.png)

Sobald Sie alle Einstellungen richtig vorgenommen haben, ist es an der Zeit, alles auf die Karte zu schreiben. Beachten Sie, dass dabei alle vorhandenen Daten auf der SD-Karte vollständig gelöscht werden. Seien Sie also vorsichtig.

![Daten auf die Karte schreiben](images/Pi-warning.png)

Danach können Sie sich entspannt zurücklehnen und eine Tasse Kaffee genießen, während Ihr Betriebssystem auf die Karte geschrieben wird. Sobald dies abgeschlossen ist, können wir mit der Konfiguration der Hardware fortfahren.

{{< pagenav prev="../page1" next="../page3/" >}}
