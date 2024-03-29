---
title: "Installer les pilotes NVIDIA sur debian server"
date: 2023-07-21T22:36:07-04:00
draft: false
---

## Install driver
```
sudo apt-get install linux-headers-$(uname -r)
sudo apt install build-essential
```

### Download NVIDIA driver
```
BASE_URL=https://us.download.nvidia.com/XFree86/Linux-x86_64
DRIVER_VERSION=535.86.05
wget $BASE_URL/$DRIVER_VERSION/NVIDIA-Linux-x86_64-$DRIVER_VERSION.run
```

### Runs the installer
```
sudo chmod +x NVIDIA-Linux-x86_64-$DRIVER_VERSION.run
sudo ./NVIDIA-Linux-x86_64-$DRIVER_VERSION.run --ui=none
```

### Reference for this post
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.13.5/install-guide.html#docker

## Update driver
sudo apt-get install linux-headers-$(uname -r)


## Docker
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

sudo docker run --rm --runtime=nvidia --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi

 
