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
sudo qemu-nbd --connect=/dev/nbd0 debian-12-genericcloud-amd64-20230711-1438.qcow2
sudo mount /dev/nbd0p1 /mnt/cloud-disk
```



```
sudo umount /mnt/cloud-disk
sudo qemu-nbd --disconnect /dev/nbd0
sudo rmmod nbd
```

Sur votre serveur Proxmox
```
qm destroy 900 --destroy-unreferenced-disks --purge
qm create 900 --memory 2048 --net0 virtio,bridge=vmbr1 --scsihw virtio-scsi-pci --name debian-12-slythe
qm importdisk 900 debian-12-slythe-amd64-20230711-1438.qcow2  local-lvm -format qcow2
qm set 900 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-900-disk-0
qm set 900 --ide2 local-lvm:cloudinit --boot c --bootdisk scsi0 --serial0 socket --vga serial0


qm template 900
qm clone 900 901 --name test-00

qm resize 901 scsi0 30G

qm set 901 --ipconfig0 ip=dhcp
#qm set 901 --ipconfig0 ip=10.10.10.222/24,gw=10.10.10.1

qm set 901 --sshkey ~/.ssh/id_rsa.pub
#qm set 901 --cipassword AweSomePassword

qm cloudinit dump 901 user

```
