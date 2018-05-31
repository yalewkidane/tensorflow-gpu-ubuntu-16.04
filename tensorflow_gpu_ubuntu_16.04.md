# tensorflow-gpu-ubuntu-16.04

## Step 0: Noveau drivers
Before you begin, you may need to disable the opensource ubuntu NVIDIA driver called nouveau.

* remove all nvidia packages ,skip this if your system is fresh installed

```text
sudo apt-get remove nvidia* && sudo apt autoremove
```
install some packages for build kernel:
```text
sudo apt-get install dkms build-essential linux-headers-generic
```
now block and disable nouveau kernel driver:
```text
sudo vim /etc/modprobe.d/blacklist.conf
```
Insert follow lines to the blacklist.conf:
```text
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```

save and exit.

Disable the Kernel nouveau by typing the following commands(nouveau-kms.conf may not exist,it is ok):
```text
echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
```

build the new kernel by:
```text
sudo update-initramfs -u
```

reboot
```text
sudo reboot
```

## Installation steps

1. update apt-get
```text
sudo apt-get update
```

Install apt-get deps
```text
sudo apt-get install openjdk-8-jdk git python-dev python3-dev python-numpy python3-numpy build-essential python-pip python3-pip python-virtualenv swig python-wheel libcurl3-dev curl   
```

2. install nvidia drivers
```text
sudo reboot
```
