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

2. Install apt-get deps
```text
sudo apt-get install openjdk-8-jdk git python-dev python3-dev python-numpy python3-numpy build-essential python-pip python3-pip python-virtualenv swig python-wheel libcurl3-dev curl   
```

3. install nvidia drivers

```text
# The 16.04 installer works with 16.10.
# download drivers
curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb

# download key to allow installation
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub

# install actual package
sudo dpkg -i ./cuda-repo-ubuntu1604_9.0.176-1_amd64.deb

#  install cuda (but it'll prompt to install other deps, so we try to install twice with a dep update in between
sudo apt-get update
sudo apt-get install cuda-9-0   
```
2a. reboot Ubuntu
```text
sudo reboot
```
2b. check nvidia driver install
```text
nvidia-smi   

# you should see a list of gpus printed    
# if not, the previous steps failed.  
```

3. Install cudnn
```text
wget https://s3.amazonaws.com/open-source-william-falcon/cudnn-9.0-linux-x64-v7.1.tgz  
sudo tar -xzvf cudnn-9.0-linux-x64-v7.1.tgz  
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```


4. Add these lines to end of ~/.bashrc:
```text
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
export CUDA_HOME=/usr/local/cuda
```

4a. Reload bashrc
```text
source ~/.bashrc
```

5. Install miniconda
```text
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh   

# press s to skip terms   

# Do you approve the license terms? [yes|no]
# yes

# Miniconda3 will now be installed into this location:
# accept the location

# Do you wish the installer to prepend the Miniconda3 install location
# to PATH in your /home/ghost/.bashrc ? [yes|no]
# yes 
```

5a. Reload bashrc
```text
source ~/.bashrc
```

6. Create conda env to install tf
```text
conda create -n tensorflow

# press y a few times 
```

7. Activate env
```text
source activate tensorflow  
```

8. Install tensorflow with GPU support for python 3.6


```text
pip install tensorflow-gpu
# If the above fails, try the part below
# pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.2.0-cp36-cp36m-linux_x86_64.whl
```


9. Test tf install
```text
# start python shell   
python

# run test script   
import tensorflow as tf   

hello = tf.constant('Hello, TensorFlow!')

# when you run sess, you should see a bunch of lines with the word gpu in them (if install worked)
# otherwise, not running on gpu
sess = tf.Session()
print(sess.run(hello))
```


