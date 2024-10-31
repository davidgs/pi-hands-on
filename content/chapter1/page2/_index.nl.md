---
title:  'Partitioneren voor updates'
date: 2024-10-24T12:23:03-04:00
weight: 2
reading_time: 2 minutes
---

## A/B-partitionering

Een beetje achtergrondinformatie is hier waarschijnlijk wel op zijn plaats. Het idee van A/B-partitionering is een belangrijk concept voor herstelbaarheid. Als u een enkele schijfpartitie hebt waar uw apparaten vanaf opstarten, en u werkt kritieke items in die partitie bij die op de een of andere manier beschadigd zijn, kan uw apparaat in een staat achterblijven waarin het onmogelijk is om op te starten of te herstellen. Het is geblokkeerd. De enige manier om zo'n apparaat te herstellen is doorgaans om fysiek toegang te krijgen tot het apparaat en rechtstreeks wijzigingen aan te brengen op de SD-kaart. Dit is niet altijd praktisch, of zelfs maar mogelijk.

Met A/B-partitionering maakt u dual boot-partities en voert u deze alleen uit vanaf één partitie. Dat is de bekende goede of primaire partitie. Vervolgens hebt u een secundaire partitie waarop u updates kunt toepassen. Zodra een update is toegepast op de secundaire partitie, start het apparaat opnieuw op vanaf die nieuw bijgewerkte partitie. Als de update succesvol is, is uw systeem weer up and running en wordt die partitie gemarkeerd als de primaire partitie en start het vanaf nu opnieuw op vanaf die bekende goede partitie.

Als de update om een of andere reden mislukt en het apparaat niet goed kan opstarten vanaf de bijgewerkte partitie, start het systeem opnieuw op vanaf de eerder gebruikte primaire partitie. Het apparaat kan dan blijven werken totdat er een verbeterde update kan worden geïmplementeerd.

![A/B-partitionering](images/AB-part.png)

Met dit partitieschema is de kans veel kleiner dat uw Pi vastloopt, omdat u altijd een partitie kunt gebruiken waarvan u weet dat deze goed werkt en waarvan u kunt opstarten.

Bootware versleutelt de A-, B- en DATA-partities. De A- en B-partitie zijn vergrendeld met unieke LUKS-sleutels, wat betekent dat u de Backup-partitie niet kunt benaderen vanaf de Active-partitie. De versleutelde DATA-partitie is toegankelijk vanaf de A- of B-partitie.

Het instellen van dit A/B-partitioneringsschema is meestal vrij omslachtig en moeilijk te implementeren. Zymbit's Bootware heeft dat proces overgenomen en vereenvoudigd, zodat het een relatief eenvoudig proces is. Laten we dat proces nu eens doorlopen en uw Pi zowel veilig als veerkrachtig maken.

{{< pagenav prev="../page1" next="../../chapter2" >}}
