---
title:  „Partitionierung für Updates“
date: 2024-10-24T12:23:03-04:00
weight: 2
reading_time: 2 minutes
---

## A/B-Partitionierung

Hier sind einige Hintergrundinformationen wahrscheinlich angebracht. Die Idee der A/B-Partitionierung ist ein wichtiges Konzept für die Wiederherstellbarkeit. Wenn Sie eine einzelne Festplattenpartition haben, von der Ihre Geräte booten, und Sie kritische Elemente in dieser Partition aktualisieren, die irgendwie beschädigt sind, kann Ihr Gerät in einem Zustand zurückbleiben, in dem es unmöglich ist, es zu booten oder wiederherzustellen. Es ist blockiert. Die einzige Möglichkeit, ein solches Gerät wiederherzustellen, besteht normalerweise darin, physisch auf das Gerät zuzugreifen und direkte Änderungen an der SD-Karte vorzunehmen. Dies ist nicht immer praktisch oder sogar möglich.

Mit der A/B-Partitionierung erstellen Sie Dual-Boot-Partitionen und führen nur von einer aus. Das ist die bekanntermaßen funktionierende oder primäre Partition. Sie haben dann eine sekundäre Partition, auf die Sie Updates anwenden können. Sobald ein Update auf die sekundäre Partition angewendet wurde, startet das Gerät von dieser neu aktualisierten Partition neu. Wenn das Update erfolgreich ist, ist Ihr System wieder einsatzbereit und diese Partition wird dann als primäre Partition markiert und wird von nun an von dieser bekanntermaßen funktionierenden Partition neu gestartet.

Wenn die Aktualisierung aus irgendeinem Grund fehlschlägt und das Gerät nicht ordnungsgemäß von der aktualisierten Partition booten kann, wird das System von der zuvor verwendeten primären Partition neu gestartet und kann weiter ausgeführt werden, bis eine korrigierte Aktualisierung bereitgestellt werden kann.

![A/B-Partitionierung](images/AB-part.png)

Mit diesem Partitionierungsschema ist die Wahrscheinlichkeit, dass Ihr Pi blockiert wird, wesentlich geringer, da Sie jederzeit eine bekanntermaßen funktionierende Partition zum Booten aufrechterhalten können.

Bootware verschlüsselt die Partitionen A, B und DATA. Die Partitionen A und B sind mit einzigartigen LUKS-Schlüsseln gesperrt, was bedeutet, dass Sie von der aktiven Partition aus nicht auf die Backup-Partition zugreifen können. Auf die verschlüsselte DATA-Partition kann entweder von der Partition A oder B aus zugegriffen werden.

Das Einrichten dieses A/B-Partitionierungsschemas ist normalerweise recht umständlich und schwierig umzusetzen. Die Bootware von Zymbit hat diesen Prozess übernommen und so vereinfacht, dass er relativ einfach ist. Lassen Sie uns diesen Prozess jetzt durchgehen und Ihren Pi sowohl sicher als auch widerstandsfähig machen.

{{< pagenav prev="../page1" next="../../chapter2" >}}
