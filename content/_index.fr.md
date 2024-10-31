---
title:  " "
type: "home"
reading_time: 2 minutes
---

## Bienvenue à l'atelier

### Aperçu

Nous allons construire un Raspberry Pi (modèle 4B) à partir de zéro, puis utiliser une [Zymkey](https://zymbit.com/zymkey) pour le sécuriser en chiffrant le système de fichiers racine « rootfs ». Nous utiliserons [Bootware](https://zymbit.com/bootware) pour créer une nouvelle table de partition.

Nous activerons également le partitionnement A/B pour rendre les mises à jour résilientes, fiables et récupérables en cas de mauvaise mise à jour.

### Chargement de ce tutoriel

Nous vous encourageons à charger ce tutoriel sur votre ordinateur portable afin de faciliter les choses (comme la possibilité de copier/coller des commandes) en allant sur [https://davidgs.com/zymbit-workshop](https://davidgs.com/zymbit-workshop)

### Ordre du jour

- Un bref aperçu de ce que nous aborderons dans cet atelier
- Quelques informations générales sur certains termes et solutions que nous utiliserons
- Quincaillerie de bâtiment
- Installation du logiciel
- Configuration du logiciel
- Tester notre configuration
- Envoyer des messages sécurisés

### Utilisation de la ligne de commande

La quasi-totalité de cet atelier consistera à interagir avec votre Raspberry Pi via l'interface de ligne de commande (CLI). Si vous n'êtes pas familier avec l'utilisation de la CLI, ce sera l'occasion d'en apprendre davantage sur cet outil puissant.

> [!TIP]
> **Utilisateurs Windows**
>
> La meilleure solution pour Windows est de vous assurer que WSL est installé et actif. Cela vous donnera accès à une ligne de commande appropriée que vous pourrez utiliser pour `ssh`, etc. Pour installer WSL, consultez les instructions fournies [ici](https://davidgs.com/zymbit-workshop/index.html)

> [!TIP]
> **Utilisateurs Mac**
>
> Vous disposez déjà d'une application de terminal appropriée. Accédez simplement à votre dossier Applications, puis au dossier Utilitaires et recherchez Terminal.app. Vous pouvez également installer d'autres programmes de terminal si vous le préférez.

> [!TIP]
> **Utilisateurs Linux**
>
> Vous disposez de terminaux natifs disponibles avec votre distribution Linux. Recherchez les applications « Terminal » ou « XTerm ».

### Ménage

Au cours de cet atelier, vous verrez que certaines choses seront évoquées de différentes manières. Ce sont des choses auxquelles je voudrais que vous accordiez une attention particulière.

> [!CAUTION]
> Informe sur les risques ou les conséquences négatives de certaines actions.

> [!IMPORTANT]
> Informations clés que les utilisateurs doivent connaître pour atteindre leur objectif.

> [!NOTE]
> Informations utiles que les utilisateurs devraient connaître, même en survolant le contenu.

> [!TIP]
> Des conseils utiles pour faire les choses mieux ou plus facilement.

> [!WARNING]
> Informations urgentes qui nécessitent une attention immédiate de l'utilisateur pour éviter des problèmes.

## C'est parti !

{{< pagenav next="chapter1/" >}}
