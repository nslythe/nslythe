---
title: "Créer un image cloud-init"
date: 2023-07-12T22:36:07-04:00
draft: false
---

https://cloud.debian.org/images/cloud/bookworm/20230711-1438/debian-12-nocloud-amd64-20230711-1438.raw


Préparation

```
sudo apt install qemu-utils
sudo modprobe nbd max_part=8
```
Téléchargez l'image que vous voulez modifier en format qcow2
Dans mon cas la version èa jour de debian 12
```
wget https://cloud.debian.org/images/cloud/bookworm/20230711-1438/debian-12-genericcloud-amd64-20230711-1438.qcow2
```


```
sudo mkdir /mnt/cloud-disk
sudo qemu-nbd --connect=/dev/nbd0 debian-12-nocloud-amd64-20230711-1438.qcow2
sudo mount /dev/nbd0p1 /mnt/cloud-disk
```



```
sudo umount /mnt/cloud-disk
sudo qemu-nbd --disconnect /dev/nbd0
sudo rmmod nbd
```
