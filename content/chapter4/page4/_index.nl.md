---
title:  'Veilig opstarten'
date: 2024-10-25T13:12:15-04:00
weight: 4
---

### Bootware configureren

Hier begint het echte plezier! Als je ooit LUKS hebt gebruikt om een Pi-bestandssysteem te versleutelen, weet je dat, hoewel het een geweldige stap is in het beveiligen van je Pi, je die versleutelingssleutel nog steeds ergens moet opslaan die toegankelijk is tijdens het opstarten.

Met Bootware en een Zymbit HSM wordt de LUKS-encryptiesleutel vergrendeld door de Zymbit HSM, waardoor deze veel veiliger is. Bootware verwacht dat de bootimage in een specifiek, gecodeerd formaat is, een z-image genaamd. De Bootware CLI-tool helpt u bij het maken en beheren van deze images voor implementatie in uw hele onderneming.

Laten we ons eerste z-beeld maken en het huidige systeem als basis gebruiken.

Eerst moeten we de USB-stick koppelen, zodat we een plek hebben om onze z-image op te slaan:

```bash
sudo mount /dev/sda1 /mnt
```

Vervolgens voeren we de imagingtool uit om een gecodeerde z-image van ons huidige systeem te maken:

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

Let op dat ik het koppelpunt voor de USB-drive als onze uitvoerdirectory heb gebruikt. Vervolgens heb ik een naam en versienummer voor de image gekozen en ervoor gekozen om een softwaresleutel te gebruiken, aangezien ik een Zymkey gebruik.

Wees niet verbaasd als deze stap even duurt. Wat het doet is een complete kopie maken van de bestanden op de draaiende schijf, en deze ondertekenen met de hardwaresleutel die het heeft gegenereerd


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

Het openbare/privé-sleutelpaar wordt op de USB-stick opgeslagen. We hebben het later nodig.

Als je je het [begin](/chapter1/page2) nog herinnert, gaan we A/B-partities maken

Bootware versleutelt de A-, B- en DATA-partities. De A- en B-partitie zijn vergrendeld met unieke LUKS-sleutels, wat betekent dat u de Backup-partitie niet kunt benaderen vanaf de Active-partitie. De versleutelde DATA-partitie is toegankelijk vanaf de A- of B-partitie.

Het opzetten van dit A/B-partitioneringsschema is meestal vrij omslachtig en moeilijk te implementeren. Zymbit's Bootware heeft dat proces overgenomen en vereenvoudigd, zodat het een relatief eenvoudig proces is. Laten we dat proces nu eens doorlopen en uw Pi zowel veilig als veerkrachtig maken.

### A/B-partities maken

Omdat we nog geen backup B-partitie hebben gehad, maken we er een aan en plaatsen we de huidige image (waarvan we weten dat die goed is, omdat we die momenteel draaien) in die partitie. Om dat te doen, updaten we de configuratie (maken we hem echt) met de `zbcli`-tool.

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

Dit proces zal u enkele vragen stellen om te bepalen hoe u uw partities moet indelen. De eerste is welke apparaatpartitie-indeling u wilt gebruiken. Kies de aanbevolen optie:
```bash
? Select device partition layout after an update ›
❯   [RECOMMENDED] A/B: This will take the remaining disk space available after the boot partition and create two encrypted partitions, each taking up half of the remaining space. Most useful for rollback and reco
       Using partition layout (A/B)
        Info the root file system will be re-partitioned with your chosen configuration.
```
Vervolgens selecteert u het updatebeleid. Kies wederom gewoon de aanbevolen.

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

Vervolgens kunt u de grootte van de datapartitie selecteren. Standaard is dit 512 MB, maar ik raad aan om dit te verhogen naar 1024 MB.

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

We hebben nu een systeem dat is geconfigureerd voor A/B-partitionering en dat updates op de back-uppartitie toepast zodra ze beschikbaar zijn.

> [!TIP]
> **Logboeken**
>
> Bootwareberichten worden vastgelegd in `/boot/firmware/zboot.log`. Als u wilt zien wat Bootware doet, kunt u daar terecht.
>

Om het proces te voltooien, zullen we de update daadwerkelijk toepassen (wat eigenlijk gewoon een kopie is van het momenteel draaiende systeem). Dit zal het opnieuw partitioneren en opnieuw opstarten activeren.

{{< pagenav prev="../page3" next="../page5/" >}}
