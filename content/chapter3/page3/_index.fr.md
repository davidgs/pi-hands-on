---
title:  « Configuration Pi »
date: 2024-10-25T08:53:52-04:00
weight: 3
reading_time: 2 minutes
---

Il est maintenant temps de brancher le Pi à l'alimentation, d'attendre qu'il démarre et de commencer à configurer notre sécurité !

Tout d'abord, connectons-nous à votre Pi. Vous vous souvenez du nom d'hôte que vous lui avez donné pendant que vous [Configurez votre carte SD](chapitre 3/page 2/) ? Vous en aurez besoin, ainsi que du nom d'utilisateur/mot de passe que vous avez défini. Vous aurez également besoin d'une application de terminal.

> [!IMPORTANT]
> Sous macOS, vous pouvez aller dans Applications –> Utilitaires –> Terminal.app
> Sous Linux, l'application XTerm ou Terminal fonctionnera
> Sous Windows, vous aurez besoin de [Putty](https://www.putty.org) ou d'une application Terminal similaire

> [!TIP]
> **Utilisateurs Windows**
>
> La meilleure solution pour Windows est de vous assurer que WSL est installé et actif. Cela vous donnera accès à une ligne de commande appropriée que vous pourrez utiliser pour `ssh`, etc. Pour installer WSL, consultez les instructions fournies [ici](https://davidgs.com/zymbit-workshop/index.html)


```bash
ssh <username>@<hostname.local>
```
Entrez ensuite le mot de passe que vous avez défini précédemment. Puisque j'ai appelé mon Pi « zymbit-pi » et défini le nom d'utilisateur et le mot de passe sur « zymbit », j'utiliserais

```bash
ssh zymbit@zymbit-pi.local
zymbit@zymbit-pi.local's password: <zymbit>
...
zymbit-pi:~  $
```
Et vous êtes maintenant connecté à votre Raspberry Pi !

Avant de pouvoir configurer le Zymkey, nous devons nous assurer que le Pi peut lui communiquer. Le logiciel Zymkey communique avec l'appareil via I2C, nous devons donc nous assurer que l'interface I2C du Pi est activée.

```bash
$ sudo raspi-config
```
Vous amène à l'utilitaire de configuration.

![Écran initial de Raspi-config](images/interface-options.png)

Vous sélectionnerez ensuite « Options d'interface » puis « I2C »

![Activer l'interface I2C](images/enable-i2c.png)

Vous pouvez ensuite quitter et enregistrer raspi-config

![enregistrer les modifications de l'interface I2C](images/ic2-enabled1.png)

Il n'est plus nécessaire de redémarrer votre Pi après cela.

Une dernière chose avant de continuer. Il est probablement judicieux de s'assurer que le système d'exploitation est entièrement à jour. Vous pouvez exécuter les 2 commandes suivantes :

```bash
sudo apt update
sudo apt upgrade
```

> [!TIP]
> Si vous en avez assez d'ajouter `sudo` à toutes ces commandes de gestion, vous pouvez entrer
> ```bash
> sudo -i
> ```
> Cela vous permettra d'obtenir un shell `root`. Mais n'oubliez pas qu'un grand pouvoir implique de grandes responsabilités !

Vous souhaiterez ensuite redémarrer votre Pi pour appliquer toutes les modifications.

```bash
sudo reboot
```

Et attendez que le redémarrage soit terminé.

{{< pagenav prev="../page2" next="../../chapter4/" >}}
