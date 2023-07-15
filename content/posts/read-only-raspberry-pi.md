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
fastboot noswap ro
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

### Symlink ver /tmp
```
sudo rm -rf /var/lib/dhcp /var/lib/dhcpcd5 /var/spool /etc/resolv.conf
sudo ln -s /tmp /var/lib/dhcp
sudo ln -s /tmp /var/lib/dhcpcd5
sudo ln -s /tmp /var/spool
sudo touch /tmp/dhcpcd.resolv.conf
sudo ln -s /tmp/dhcpcd.resolv.conf /etc/resolv.conf
```

```
sudo rm /var/lib/systemd/random-seed
sudo ln -s /tmp/random-seed /var/lib/systemd/random-seed
```

Modifier le fichier **/lib/systemd/system/systemd-random-seed.service**
Ajouter 
```ExecStartPre=/bin/echo "" >/tmp/random-seed``` 
dans la section **servier**


## Reboot

## Config optionel
Ajouter ces lignes dans **/etc/bash.bashrc**
```
set_bash_prompt() {
    fs_mode=$(mount | sed -n -e "s/^\/dev\/.* on \/ .*(\(r[w|o]\).*/\1/p")
    PS1='\[\033[01;32m\]\u@\h${fs_mode:+($fs_mode)}\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
}
alias ro='sudo mount -o remount,ro / ; sudo mount -o remount,ro /boot'
alias rw='sudo mount -o remount,rw / ; sudo mount -o remount,rw /boot'
PROMPT_COMMAND=set_bash_prompt
```

Ajouter cest lignes dans **/etc/bash.bash_logout** (Créer ou éditer)
```
sudo mount -o remount,ro /
sudo mount -o remount,ro /boot
```
