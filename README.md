# Effacement de données sur support de stockage magnétique

Par Adrien Ferron / lacapsule.org / Fédération régionale des reconditionneurs bretons OGO 2024

## Détection de disque

Avant de lancer une procédure de destruction, il est nécessaire d'identifier les disques.

### Utilitaire `fdisk`

Ceci peut se faire par le biais de la commande suivante :

```bash
sudo fdisk -l 
```

Le retour de cette commande doit être semblable à celui-ci :

```bash
Disque /dev/sda : 931,51 GiB, 1000204886016 octets, 1953525168 secteurs
Disk model: Samsung SSD 870 
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0xd535506e
```

D'autres utilitaires peuvent permettre l'identification des disques :

### Utilitaire `blkid`

```bash
blkid
```

Le retour de cette commande doit être semblable à celui-ci :

```bash
/dev/mmcblk0p1: LABEL_FATBOOT="EOS_DIGITAL" LABEL="EOS_DIGITAL" UUID="120E-0C64" BLOCK_SIZE="512" TYPE="vfat"
/dev/sda2: LABEL_FATBOOT="DATA" LABEL="DATA" UUID="A44A-035A" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="d535506e-02"
/dev/sda1: UUID="d4c8b862-7d36-4717-a4da-c2444dedc2e1" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="d535506e-01"
```

## Destruction de données

Pour détruire les données d'un disque dur magnétique, il est possible d'utiliser l'utilitaire suivant :

```bash
sudo shred -n 3 -z -u -v /dev/sdX
```

## Vérification de la bonne destruction des données

Afin de vérifier la bonne réussite du protocole de destruction, il est nécessaire de le brancher et vérifier la présence d'une table de partition.

Par la suite, une recherche approfondie peut être effectuée grâce à l'utilitaire TestDisk de Christophe Grenier (CGSecurity).

```bash
sudo testdisk 
```

Si aucun de ces tests ne permettent de monter une partition ou de récupérer la table antérieure, le disque est considéré comme étant clair. 
