# Astuces pour Jeux sur Linux

> [!WARNING]
> Merci de bien lire cette introduction en entier ;)

Certains jeux sous Linux nécessitent parfois des ajustements ou des commandes spécifiques pour fonctionner de manière optimale. Bien que le site **[ProtonDB](https://www.protondb.com/)** regorge d'astuces, il est essentiel de faire le tri, car certaines recommandations peuvent ne pas être fiables. Cette page a pour objectif de vous fournir des solutions vérifiées afin d’améliorer votre expérience de jeu sous Linux.

Assurez-vous que vos jeux sont compatibles avec Linux. Aujourd’hui, Proton offre une compatibilité exceptionnelle, permettant à la majorité des jeux solo de se lancer sans problème. Cependant, de nombreux jeux multijoueurs posent encore problème sous Linux en raison de leurs systèmes anti-triche incompatibles. Vous pouvez vérifier la compatibilité de ces jeux sur le site **[Are We Anti-Cheat Yet?](https://areweanticheatyet.com/)**. Multijoueurs notables incompatibles : **Fortnite, Battlefield récents, Valorant, League Of Legend, Call of Duty récents, Fifa**...

Pour des astuces plus générales sur les jeux sous Linux, vous pouvez également consulter cette page complémentaire : **[GLF Astuces sur Codeberg](https://codeberg.org/Gaming-Linux-FR/glf-astuces)**, en particulier ces deux chapitres : **[Problème de performance avec BAR ou SAM](https://codeberg.org/Gaming-Linux-FR/glf-astuces#probl%C3%A8me-de-performance-avec-bar-ou-sam)** et **[Problème de performance avec les CPU Intel de 12ᵉ génération et plus](https://codeberg.org/Gaming-Linux-FR/glf-astuces#probl%C3%A8me-de-performance-avec-les-cpu-intel-de-12%E1%B5%89-g%C3%A9n%C3%A9ration-et-plus)**.

L’ordre des commandes peut jouer un rôle crucial dans leur efficacité. Par exemple, j’ai remarqué que si je lance Dragon’s Dogma 2 avec `VKD3D_CONFIG=no_upload_hvv gamemoderun %command%`, le jeu se lance correctement. En revanche, si je mets `gamemoderun VKD3D_CONFIG=no_upload_hvv %command%`, le jeu plante.

Enfin, si vous avez une astuce non répertoriée ou que, au contraire, vous notez qu’une astuce n’est plus utile, merci de me le signaler en ouvrant un [ticket](https://codeberg.org/Gaming-Linux-FR/astuces-jeux/issues/new).

---

### Table des Matières

**[Dragon's Dogma 2](#dragon-s-dogma-2)**

**[Diablo IV](#diablo-iv)**

**[Cyberpunk 2077](#cyberpunk-2077)**

**[The Elder Scrolls Online](#the-elder-scrolls-online)**

**[God of War](#god-of-war)**

**[Marvel Rivals](#marvel-rivals)**

**[Ankama Launcher (DOFUS)](#ankama-launcher)**

---

## Dragon's Dogma 2

### Problème : Performances et compatibilité

#### Solution pour **Nvidia** et **AMD** :

Désactiver SAM/BAR (Smart Access Memory/Resizable BAR) peut améliorer les performances en utilisant la commande suivante :

```bash
VKD3D_CONFIG=no_upload_hvv %command%
```

**Source** : [vkd3d-proton Issue #1406](https://github.com/HansKristian-Work/vkd3d-proton/issues/1406#issuecomment-2014752410)

#### Problème spécifique aux cartes **Nvidia** :
Nvidia Reflex est incompatible avec ce jeu, causant divers problèmes. Pour le désactiver, utilisez la commande suivante :

```bash
VKD3D_DISABLE_EXTENSIONS=VK_NV_low_latency2 %command%
```

**Note** : De manière générale, les technologies **Nvidia** comme DLSS et RTX sont également problématiques dans ce jeu. Il est recommandé de tout désactiver dans les options du jeu pour éviter les bugs.

**Sources** : [Protondb](https://www.protondb.com/app/2054970/)

## Diablo IV

### Problème : Fuite de mémoire VRAM (Nvidia uniquement)

Diablo IV souffre d’une fuite de mémoire VRAM sur les cartes **Nvidia**, rendant le jeu injouable sur certaines configurations. Pour contourner ce problème, suivez les étapes suivantes :

1. Créez un fichier `dxvk.conf` dans le répertoire d’installation du jeu.
2. Ajoutez les lignes suivantes dans ce fichier, en remplaçant `XXXXX` par une valeur légèrement inférieure à votre VRAM totale (en Mo). Par exemple, pour une carte avec 12 Go de VRAM, utilisez `11000` :

```bash
dxgi.maxDeviceMemory=11000
dxgi.maxSharedMemory=11000
```

**Tutoriel Vidéo** : Vous pouvez suivre ce processus en détail dans cette [vidéo YouTube](https://www.youtube.com/watch?v=MHY9eYDNZLA).

## Cyberpunk 2077

### Problème : Le launcher peut causer des erreurs

Le launcher de **Cyberpunk 2077** peut parfois causer des problèmes que ce soit avec **AMD** ou **Nvidia**, quel que soit le DE et la distribution, empêchant le jeu de se lancer correctement. Une solution consiste à ignorer le launcher en ajoutant la commande suivante dans les options de lancement du jeu :

```bash
--launcher-skip
```

Attention, si vous avez déjà des options de lancement, placez cette commande après `%command%`, par exemple :

```bash
gamemoderun %command% --launcher-skip
```

Cela permet de lancer le jeu directement sans passer par le launcher, réduisant ainsi les risques de bugs ou de plantages liés à celui-ci.

**Sources** : [Proton Issue #4450](https://github.com/ValveSoftware/Proton/issues/4450#issuecomment-2030559290), [Protondb](https://www.protondb.com/app/1091500)

## The Elder Scrolls Online

### Problème : Consommation de performances par le launcher

Le launcher de **The Elder Scrolls Online** peut parfois causer une consommation excessive de ressources, impactant ainsi les performances du jeu, en particulier sur certaines configurations **AMD** et **Nvidia**. Une solution simple consiste à fermer le launcher une fois le jeu lancé.

Fermez simplement le launcher après avoir cliqué sur "Jouer". Cela libère des ressources système, améliorant ainsi les performances du jeu.

## God of War

### Problème : Performance sur les CPU Intel de 12ᵉ génération et plus

Les utilisateurs de **God of War** sur Linux avec des processeurs Intel de 12ᵉ génération et au-delà peuvent rencontrer des problèmes de performance dus à la détection des split-locks par le noyau Linux. Ce problème se manifeste par un ralentissement intentionnel des applications mal conçues, comme certains jeux. Une solution consiste à désactiver cette détection pour améliorer les performances du jeu.

Pour plus de détails sur ce problème et comment le résoudre, consultez [cet article](https://github.com/Cardiacman13/astuces-gaming).

## Marvel Rivals

### Problème : Le jeu ne se lance pas

Si **Marvel Rivals** ne se lance pas, essayez d’ajouter la commande suivante dans les options de lancement du jeu :

```bash
SteamDeck=1 %command%
```

Cela permet de simuler un environnement Steam Deck, résolvant souvent des problèmes de compatibilité.

---

## Ankama Launcher

### Vidéo explicative

Si vous préférez un tutoriel vidéo, découvrez mon guide complet en vidéo :
[Voir la vidéo sur DOFUS 3 Unity Linux](https://www.youtube.com/watch?v=Pw5FSS-W_qw)

### 1. Télécharger et rendre exécutable l’AppImage

Le launcher DOFUS 3 est distribué sous forme d’AppImage. Voici les étapes pour l’utiliser :

1. Télécharger le fichier AppImage depuis le [site officiel d’Ankama](https://www.ankama.com/fr/launcher).
2. Rendre le fichier exécutable :
   ```bash
   chmod +x Dofus_3.0-x86_64.AppImage
   ```

**Note :** Selon votre DE, par exemple KDE Plasma, vous pourrez directement double-cliquer sur l’AppImage sans avoir à passer par le terminal, Plasma vous proposera de le rendre exécutable.

Si votre distribution n’a pas FUSE d’installé, le launcher ne s’ouvrira pas. Pas de panique, on va voir ça dans l’étape suivante.

### 2. Installer FUSE : la dépendance essentielle

Les AppImages dépendent de FUSE (Filesystem in Userspace) pour fonctionner. Assurez-vous qu’il est correctement installé sur votre système.

#### Pour Ubuntu/Debian :

Il vous faudra sur Ubuntu, `libfuse2` pour lancer les AppImages. Ne suivez pas tous les vieux tutoriels recommandant « fuse2 ». Installez uniquement la bibliothèque nécessaire :

```bash
sudo apt install libfuse2
```

#### Pour Arch Linux :

FUSE est disponible dans les dépôts officiels :

```bash
sudo pacman -S fuse
```

Vous pouvez aussi utiliser le launcher Ankama via l’AUR. Installez-le avec votre gestionnaire AUR préféré, par exemple `paru` :

```bash
paru -S ankama-launcher
```

Ensuite, si vous avez la version du AUR, ajoutez votre utilisateur au groupe `games` pour que le launcher puisse se mettre à jour correctement :

```bash
sudo usermod -aG games $(whoami)
```

### 3. Organisation et conseils pratiques

- Les AppImages sont portables : vous pouvez les placer où bon vous semble (dossier personnel, `/opt`, etc.).
- Pour un accès rapide, créez un raccourci dans votre menu d’applications avec un outil comme `AppImageLauncher`.
