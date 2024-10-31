---
title:  'Pi-Konfiguration'
date: 2024-10-25T08:53:52-04:00
weight: 3
reading_time: 2 minutes
---

Jetzt ist es an der Zeit, den Pi an die Stromversorgung anzuschließen, auf den Bootvorgang zu warten und mit der Einrichtung unserer Sicherheit zu beginnen!

Melden Sie sich zunächst bei Ihrem Pi an. Erinnern Sie sich an den Hostnamen, den Sie ihm beim [Konfigurieren Ihrer SD-Karte](Kapitel 3/Seite 2/) gegeben haben? Sie benötigen diesen und den Benutzernamen/das Passwort, das Sie festgelegt haben. Sie benötigen außerdem eine Terminalanwendung.

> [!IMPORTANT]
> Unter macOS können Sie zu Anwendungen –> Dienstprogramme –> Terminal.app gehen
> Unter Linux funktionieren XTerm oder die Terminal-App
> Unter Windows benötigen Sie [Putty](https://www.putty.org) oder eine ähnliche Terminalanwendung

> [!TIP]
> **Windows-Benutzer**
>
> Die beste Lösung für Windows besteht darin, sicherzustellen, dass WSL installiert und aktiviert ist. Dadurch erhalten Sie Zugriff auf eine richtige Befehlszeile, die Sie für „ssh“ usw. verwenden können. Informationen zur Installation von WSL finden Sie in den [hier bereitgestellten] Anweisungen (https://davidgs.com/zymbit-workshop/index.html).


```bash
ssh <username>@<hostname.local>
```
Geben Sie dann das zuvor festgelegte Passwort ein. Da ich meinen Pi `zymbit-pi` genannt und sowohl den Benutzernamen als auch das Passwort auf `zymbit` gesetzt habe, würde ich verwenden

```bash
ssh zymbit@zymbit-pi.local
zymbit@zymbit-pi.local's password: <zymbit>
...
zymbit-pi:~  $
```
Und Sie sind jetzt bei Ihrem Raspberry Pi angemeldet!

Bevor wir den Zymkey konfigurieren können, müssen wir sicherstellen, dass der Pi mit ihm kommunizieren kann. Die Zymkey-Software kommuniziert über I2C mit dem Gerät, daher müssen wir sicherstellen, dass die I2C-Schnittstelle des Pi aktiviert ist.

```bash
$ sudo raspi-config
```
Bringt Sie zum Konfigurationsprogramm.

![Raspi-config-Startbildschirm](images/interface-options.png)

Wählen Sie dann „Schnittstellenoptionen“ und dann „I2C“

![I2C-Schnittstelle aktivieren](images/enable-i2c.png)

Sie können dann raspi-config beenden und speichern

![Änderungen an der I2C-Schnittstelle speichern](images/ic2-enabled1.png)

Ein Neustart Ihres Pi ist danach nicht mehr notwendig.

Eine letzte Sache, bevor wir fortfahren. Es ist wahrscheinlich eine gute Idee, sicherzustellen, dass das Betriebssystem vollständig auf dem neuesten Stand ist. Sie können die folgenden beiden Befehle ausführen:

```bash
sudo apt update
sudo apt upgrade
```

> [!TIP]
> Wenn Sie es leid sind, `sudo` zu all diesen Verwaltungsbefehlen hinzuzufügen, können Sie eingeben
> ```bash
> sudo -i
> ```
> Damit erhalten Sie eine `root`-Shell. Aber denken Sie daran: Mit viel Macht geht auch viel Verantwortung einher!

Anschließend müssen Sie Ihren Pi neu starten, um alle Änderungen zu übernehmen.

```bash
sudo reboot
```

Und warten Sie, bis der Neustart abgeschlossen ist.

{{< pagenav prev="../page2" next="../../chapter4/" >}}
