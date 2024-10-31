---
title:  ""
type: "home"
reading_time: 2 minutes
---

## Welkom bij de Workshop

### Overzicht

We gaan een Raspberry Pi (model 4B) helemaal opnieuw bouwen en dan een [Zymkey](https://zymbit.com/zymkey) gebruiken om het te beveiligen door het root-bestandssysteem `rootfs` te versleutelen. We gaan [Bootware](https://zymbit.com/bootware) gebruiken om een nieuwe partitiemap te maken.

We gaan ook A/B-partitionering inschakelen om updates veerkrachtig, betrouwbaar en herstelbaar te maken in het geval van een slechte update.

### Deze tutorial laden

U wordt aangemoedigd om deze tutorial op uw laptop te laden om het gemakkelijker te maken (zoals de mogelijkheid om opdrachten te kopiëren/plakken) door naar [https://davidgs.com/zymbit-workshop](https://davidgs.com/zymbit-workshop) te gaan

### Agenda

- Een kort overzicht van wat we in deze workshop zullen behandelen
- Enkele achtergrondinformatie over enkele termen en oplossingen die we zullen gebruiken
- Bouwbeslag
- Software installeren
- Software configureren
- Onze configuratie testen
- Stuur een aantal beveiligde berichten

### De opdrachtregel gebruiken

Bijna de hele workshop zal bestaan uit interactie met uw Raspberry Pi via de Command Line Interface (CLI). Als u niet bekend bent met het gebruik van de CLI, is dit een kans om meer te leren over deze krachtige tool.

> [!TIP]
> **Windows-gebruikers**
>
> De beste oplossing voor Windows is om ervoor te zorgen dat u WSL geïnstalleerd en actief hebt. Dit geeft u toegang tot een juiste opdrachtregel die u kunt gebruiken voor `ssh`, etc. Om WSL te installeren, zie de instructies die [hier](https://davidgs.com/zymbit-workshop/index.html) worden gegeven

> [!TIP]
> **Mac-gebruikers**
>
> U hebt al een geschikte terminalapplicatie beschikbaar. Ga gewoon naar uw Applications-map, dan naar de Utilities-map en zoek naar Terminal.app. U kunt ook andere terminalprogramma's installeren als u dat wilt.

> [!TIP]
> **Linux-gebruikers**
>
> U hebt native terminals beschikbaar met uw Linux-distributie. Zoek naar 'Terminal' of 'XTerm'-applicaties.

### Huishouden

Terwijl we deze workshop doorlopen, zul je nog steeds dingen zien die op verschillende manieren worden genoemd. Dit zijn dingen waar ik wil dat je speciale aandacht aan besteedt.

> [!CAUTION]
> Adviseert over risico's of negatieve uitkomsten van bepaalde acties.

> [!IMPORTANT]
> Belangrijke informatie die gebruikers nodig hebben om hun doel te bereiken.

> [!NOTE]
> Nuttige informatie waar gebruikers op moeten letten, zelfs wanneer ze de inhoud snel doornemen.

> [!TIP]
> Handige tips om dingen beter of gemakkelijker te doen.

> [!WARNING]
> Dringende informatie die onmiddellijke aandacht van de gebruiker vereist om problemen te voorkomen.

## Laten we beginnen!

{{< pagenav next="chapter1/" >}}
