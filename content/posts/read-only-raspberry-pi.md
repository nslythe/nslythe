---
title: "Read Only Raspberry Pi"
date: 2023-07-12T22:36:07-04:00
draft: false
---



## Préparation

Mettre à jour votre raspberry-pi

```
sudo apt-get update && apt-get upgrade
```

```
sudo apt-get remove --purge triggerhappy logrotate dphys-swapfile
```

```
sudo apt-get autoremove --purge
```

Dans /boot/cmdline.txt ajouter ceci à la fin de la ligne

```
astboot noswap ro
```

## Log manager
```
sudo apt-get install busybox-syslogd
sudo apt-get remove --purge rsyslog
```
Maintenant pour lire les log system **sudo logread**


## Système de fichier en lecture seule
Ajouter ro au système de fichier por **/** et **/boot** dans **/etc/fstab**

## Systène de fichier temporaire
Ajouter ces lignes dans **/etc/fstab**
```
tmpfs        /tmp            tmpfs   nosuid,nodev         0       0
tmpfs        /var/log        tmpfs   nosuid,nodev         0       0
tmpfs        /var/tmp        tmpfs   nosuid,nodev         0       0
```
