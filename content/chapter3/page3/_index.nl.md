---
title:  'Pi-configuratie'
date: 2024-10-25T08:53:52-04:00
weight: 3
reading_time: 2 minutes
---

Nu is het tijd om de Pi aan te sluiten op de voeding, te wachten tot hij is opgestart en te beginnen met het instellen van de beveiliging!

Laten we eerst inloggen op je Pi. Weet je nog welke hostnaam je hem teruggaf toen je [Je SD-kaart configureerde](hoofdstuk3/pagina2/)? Die heb je nodig, en de gebruikersnaam/het wachtwoord dat je hebt ingesteld. Je hebt ook een terminalapplicatie nodig.

> [!IMPORTANT]
> Op macOS kunt u naar Toepassingen -> Hulpprogramma's -> Terminal.app gaan
> Op Linux werken de XTerm- of Terminal-apps
> Op Windows heb je [Putty](https://www.putty.org) of een vergelijkbare Terminal-applicatie nodig

> [!TIP]
> **Windows-gebruikers**
>
> De beste oplossing voor Windows is om ervoor te zorgen dat u WSL geïnstalleerd en actief hebt. Dit geeft u toegang tot een juiste opdrachtregel die u kunt gebruiken voor `ssh`, etc. Om WSL te installeren, zie de instructies die [hier](https://davidgs.com/zymbit-workshop/index.html) worden gegeven


```bash
ssh <username>@<hostname.local>
```
Voer vervolgens het wachtwoord in dat u eerder hebt ingesteld. Omdat ik mijn Pi `zymbit-pi` heb genoemd en de gebruikersnaam en het wachtwoord beide heb ingesteld op `zymbit`, zou ik gebruiken

```bash
ssh zymbit@zymbit-pi.local
zymbit@zymbit-pi.local's password: <zymbit>
...
zymbit-pi:~  $
```
En je bent nu ingelogd op je Raspberry Pi!

Voordat we de Zymkey kunnen configureren, moeten we ervoor zorgen dat de Pi ermee kan communiceren. De Zymkey-software communiceert met het apparaat via I2C, dus we moeten ervoor zorgen dat de I2C-interface van de Pi is ingeschakeld.

```bash
$ sudo raspi-config
```
Hiermee gaat u naar het configuratiehulpprogramma.

![Raspi-config beginscherm](images/interface-options.png)

Vervolgens selecteert u 'Interfaceopties' en vervolgens 'I2C'

![I2C-interface inschakelen](images/enable-i2c.png)

U kunt dan afsluiten en raspi-config opslaan

![I2C-interfacewijzigingen opslaan](images/ic2-enabled1.png)

Hierna is het niet meer nodig om uw Pi opnieuw op te starten.

Nog één ding voordat we verder gaan. Het is waarschijnlijk een goed idee om ervoor te zorgen dat het besturingssysteem volledig up-to-date is. U kunt de volgende 2 opdrachten uitvoeren:

```bash
sudo apt update
sudo apt upgrade
```

> [!TIP]
> Als u het zat bent om `sudo` aan al deze beheeropdrachten toe te voegen, kunt u
> ```bash
> sudo -i
> ```
> Waarmee je een `root` shell krijgt. Maar onthoud, met grote macht komt grote verantwoordelijkheid!

Vervolgens moet u uw Pi opnieuw opstarten om alle wijzigingen toe te passen.

```bash
sudo reboot
```

En wacht tot het opnieuw opstarten voltooid is.

{{< pagenav prev="../page2" next="../../chapter4/" >}}
