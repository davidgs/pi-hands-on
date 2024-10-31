---
title:  „Sicheres Booten“
date: 2024-10-25T13:12:15-04:00
weight: 4
---

### Bootware konfigurieren

Hier beginnt der wahre Spaß! Wenn Sie schon einmal LUKS zum Verschlüsseln eines Pi-Dateisystems verwendet haben, wissen Sie, dass dies zwar ein großer Schritt zur Sicherung Ihres Pi ist, Sie den Verschlüsselungsschlüssel jedoch trotzdem irgendwo speichern müssen, wo er beim Booten zugänglich ist.

Mit Bootware und einem Zymbit-HSM wird der LUKS-Verschlüsselungsschlüssel durch das Zymbit-HSM gesperrt, was ihn wesentlich sicherer macht. Bootware erwartet, dass das Boot-Image in einem bestimmten, verschlüsselten Format vorliegt, das als Z-Image bezeichnet wird. Das Bootware-CLI-Tool unterstützt Sie beim Erstellen und Verwalten dieser Images für die Bereitstellung in Ihrem Unternehmen.

Erstellen wir also unser erstes Z-Image und verwenden das aktuelle System als Grundlage dafür.

Zuerst müssen wir das USB-Laufwerk mounten, damit wir einen Platz für unser Z-Image haben:

```bash
sudo mount /dev/sda1 /mnt
```

Als Nächstes führen wir das Imaging-Tool aus, um ein verschlüsseltes Z-Image unseres aktuellen Systems zu erstellen:

```bash
sudo zbcli imager
```
```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	---------
     Created '/etc/zymbit/zboot/update_artifacts/tmp'
✔ Enter output directory · /mnt
✔ Enter image name · z-image-1
✔ Select image type · Full image of live system
✔ (Optional) enter image version · 1.0
✔ Select key · Create new software key
```

Beachten Sie, dass ich den Einhängepunkt für das USB-Laufwerk als Ausgabeverzeichnis verwendet habe. Anschließend habe ich einen Namen und eine Versionsnummer für das Image ausgewählt und mich für die Verwendung eines Softwareschlüssels entschieden, da ich einen Zymkey verwende.

Seien Sie nicht überrascht, wenn dieser Schritt eine Weile dauert. Dabei wird eine vollständige Kopie der Dateien auf der laufenden Festplatte erstellt und mit dem generierten Hardwareschlüssel signiert.


```bash
     Created signing key
     Created '/etc/zymbit/zboot/update_artifacts/file_manifest'
     Created '/etc/zymbit/zboot/update_artifacts/file_deletions'
    Verified path unmounted '/etc/zymbit/zboot/mnt'
     Cleaned '/etc/zymbit/zboot/mnt'
     Deleted '/etc/crypttab'
    Verified disk size (required: 2.33 GiB, free: 26.39 GiB)
     Created initramfs
     Created snapshot of boot (/etc/zymbit/zboot/update_artifacts/tmp/.tmpBgEBJk/z-image-1_boot.tar)
     Created snapshot of root (/etc/zymbit/zboot/update_artifacts/tmp/.tmpBgEBJk/z-image-1_rfs.tar)
     Created '/mnt/tmp'
     Cleaned '/mnt/tmp'
     Created staging directory (/mnt/tmp/.tmpEhjNN7)
     Created '/mnt/tmp/.tmpEhjNN7/header.txt'
     Created tarball (/mnt/tmp/.tmpEhjNN7/update_artifact.tar)
     Created header signature
     Created update artifact signature
     Created file manifest signature
     Created file deletions signature
     Created '/mnt/tmp/.tmpEhjNN7/signatures'
     Created signatures (/mnt/tmp/.tmpEhjNN7/signatures)
      Copied file (/etc/zymbit/zboot/update_artifacts/file_manifest) to (/mnt/tmp/.tmpEhjNN7/file_manifest)
      Copied file (/etc/zymbit/zboot/update_artifacts/file_deletions) to (/mnt/tmp/.tmpEhjNN7/file_deletions)
     Created tarball (/mnt/z-image-1.zi)
     Created '/mnt/z-image-1_private_key.pem'
       Saved private key '/mnt/z-image-1_private_key.pem'
     Created '/mnt/z-image-1_pub_key.pem'
       Saved public key '/mnt/z-image-1_pub_key.pem'
     Cleaned '/mnt/tmp'
       Saved image '/mnt/z-image-1.zi' (2.33 GiB)
    Finished in 384.8s
```

Das öffentliche/private Schlüsselpaar ist auf dem USB-Laufwerk gespeichert und wir werden es später brauchen.

Wenn Sie sich an den [Anfang](/Kapitel1/Seite2) erinnern, werden wir A/B-Partitionen erstellen

Bootware verschlüsselt die Partitionen A, B und DATA. Die Partitionen A und B sind mit einzigartigen LUKS-Schlüsseln gesperrt, was bedeutet, dass Sie von der aktiven Partition aus nicht auf die Backup-Partition zugreifen können. Auf die verschlüsselte DATA-Partition kann entweder von der Partition A oder B aus zugegriffen werden.

Das Einrichten dieses A/B-Partitionierungsschemas ist normalerweise recht umständlich und schwierig umzusetzen. Die Bootware von Zymbit hat diesen Prozess übernommen und so vereinfacht, dass er relativ einfach ist. Lassen Sie uns diesen Prozess jetzt durchgehen und Ihren Pi sowohl sicher als auch widerstandsfähig machen.

### Erstellen Sie A/B-Partitionen

Da wir bisher keine Backup-Partition B hatten, erstellen wir eine und platzieren das aktuelle Image (von dem wir wissen, dass es gut ist, da wir es gerade ausführen) in dieser Partition. Dazu aktualisieren wir die Konfiguration (erstellen sie wirklich) mit dem Tool `zbcli`.

```bash
$ sudo zbcli update-config
```
```
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	------reading_time: 5 minutes
---
        Info the root file system will be re-partitioned with your chosen configuration.
```

Bei diesem Vorgang werden Ihnen einige Fragen gestellt, um zu bestimmen, wie Sie Ihre Partitionen anordnen möchten. Die erste ist, welches Gerätepartitionslayout Sie verwenden möchten. Wählen Sie die empfohlene Option:
```bash
? Select device partition layout after an update ›
❯   [RECOMMENDED] A/B: This will take the remaining disk space available after the boot partition and create two encrypted partitions, each taking up half of the remaining space. Most useful for rollback and reco
       Using partition layout (A/B)
        Info the root file system will be re-partitioned with your chosen configuration.
```
Als Nächstes wählen Sie die Update-Richtlinie aus. Wählen Sie auch hier einfach die empfohlene Richtlinie aus.

```bash
? Select update policy ›
❯   [RECOMMENDED] BACKUP: Applies new updates to current backup filesystem and swap to booting the new updated backup partition as the active partition now. If the new update is bad, it will rollback into the pre
     Running [========================================] 2/1 (00:00:17):                                                                                                                                             WARNING! Detected active partition (28.71GB) is larger than 14.86GB needed for two filesystems.
 Active partition won't be saved!!!
 Changing update mode to UPDATE_BOTH!!!
       Using update mode (UPDATE_BOTH)
        Data partition size currently set to: 512 MB
        Info bootware will create a shared data partition after A/B in size MB specified
```

Als Nächstes können Sie die Größe der Datenpartition auswählen. Der Standardwert beträgt 512 MB, ich schlage jedoch vor, diesen Wert auf 1024 MB zu erhöhen.

```bash
✔ Enter size of data partition in MB · 1024
       Using Data Partition Size 1024MB
  Defaulting to configured endpoint '/dev/sda1'
        Info update endpoints can be either an HTTPS URL or an external mass storage device like a USB stick.
       Found update name 'z-image-1'
       Saved update name 'z-image-1'
       Using update endpoint '/dev/sda1'
Configuration settings saved
    Finished in 42.1s
```

Wir verfügen jetzt über ein System, das für die A/B-Partitionierung konfiguriert ist und Updates auf die Sicherungspartition anwendet, sobald diese verfügbar sind.

> [!TIP]
> **Protokolle**
>
> Bootware-Meldungen werden in „/boot/firmware/zboot.log“ protokolliert. Wenn Sie also sehen möchten, was die Bootware macht, können Sie dort nachsehen.
>

Um den Vorgang abzuschließen, wenden wir das Update an (das eigentlich nur eine Kopie des aktuell laufenden Systems ist). Dies löst die Neupartitionierung und einen Neustart aus.

{{< pagenav prev="../page3" next="../page5/" >}}
