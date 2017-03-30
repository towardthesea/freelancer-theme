---
layout: default
modal-id: 5
date: 2017-03-29
img: cake.png
alt: img-alt
project-date: 2017

---
Last week, I build a new machine for deep learning to work at home after more than 10 years working with pre-built aptops only. Thanks to [pcpartpicker](https://pcpartpicker.com), the hardware configuration and compatibility check turn to be so easy. The details of my machine is [here](https://it.pcpartpicker.com/user/towardthesea/saved/qBGwP6)

Hardware done! Then, how can you go for the software part. Of course, I choose Ubuntu as Linux distro for my deep learning work but setting it in this GPU machine is more difficult than I expected. I thought it should be as easy as on a laptop. No, not at all!

#### Setup Ubuntu
Prepare your bootable Ubuntu USB key. As I have a Windows machine, I use [Lili](https://www.linuxliveusb.com/) to create one with my 2Gbs USB key. It is a recommendation to use a large enough USB key for this, normally not smaller than 2Gbs. The usage of the application is quite straight-forward.

Boot up your machine with the Ubuntu USB key plugged in, then following the [official guide](https://www.ubuntu.com/download/desktop/install-ubuntu-desktop) or [dual boot guide](http://www.tecmint.com/install-ubuntu-16-04-alongside-with-windows-10-or-8-in-dual-boot/). If you have a big HDD like mine (2Tbs) you should read about the [partitioning schemes](https://help.ubuntu.com/community/PartitioningSchemes) and create the Ubuntu partiions by yourself. For my case, it is *40Gbs* as *root: "/"*, *100Gbs* as *home: "/home"* and no *"swap"*. For the rest, I create a shared FAT32 to store my data.

It takes some minutes to install Ubuntu in your machine.

You think you now can work with your machine on Ubuntu, but...

First, if you have a dual boot system (Ubuntu & Windows) like mine, you have to configure your Motherboard to boot with Grub of Ubuntu first. As in my case, the Motherboad MSI Z270 Gaming Pro automatically sets the Windows Boot Manager as higher priority, so everytime I boot up, it starts with Windows and doesn't allow the system to go to Grub. For the same board, you can fix it by pressing *Del* just after turning on your machine to go to Motherboard setup, then go to:

```
Settings --> Boots --> UEFI Hard Disk Drive BBS Priorities
```
and set the **Boot Option #1** as **Ubuntu**. Since then, everytime you turn on your computer, it will bring you to Grub then you can choose between *Ubuntu* or *Windows*.

Then, you get into your Ubuntu and find out that the screen is blinking. Welcome to the world of GPU card!


#### Setup your NVIDIA GTX 1080 card
Now you have to find out a way to open the terminal, the only option to install your GTX driver. You can find a full [solution](http://abhay.harpale.net/blog/linux/nvidia-gtx-1080-installation-on-ubuntu-16-04-lts/) here. Following, I only show what did work for my case. 

Hit *Ctl+Alt+F1*, it should bring you to a black screen with login prompt. It sounds good. Let 's login and following the next steps. As this step, I assume that you haven't had any GPU driver before. 
1. Edit the /etc/modprobe.d/blacklist.conf file to blacklist some modules so that they do not interfere with your installations. Without GUI, you will have to use nano, emacs or vim for editing the file. Just append these commands at the bottom of the blacklist.conf file.
```
blacklist amd76x_edac  
blacklist vga16gb  
blacklist nouveau  
blacklist rivafb  
blacklist nvidiafb  
blacklist rivatv  
```

2. Use the following *apt-get* command to install one:
```
sudo apt-get install nvidia-xxx
``` 

3. Restart your machine
```
shutdown -r now
```

4. Install **unity** and **ubuntu-desktop**
If your Ubuntu starts without a top panel and Unity Launcher, you should reinstall them by:
```
sudo apt-get install --reinstall ubuntu-desktop
sudo apt-get install unity
```

5. Restart your machine again
```
shutdown -r now
```

#### Build and install Tensorflow

Following the [official manual](https://www.tensorflow.org/install/install_sources) of Tensorflow and [NVIDIA guide](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html).

1. Install NVIDIA CUDA  
  Follow the link [link](https://developer.nvidia.com/cuda-downloads), specify your system, and choose to download the *deb(local)* file. Then, you can continue following the guide from the website, as copied below. It will install the CUDA toolkit at */usr/local/cuda*. This path will need to be specified during the Tensorflow configuration.
```
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

2. Install NVIDIA cuDNN
  Download cuDNN at [link](https://developer.nvidia.com/cudnn), which will require you to register. Then uncompress the download tgz file to location you want (assume here to be in /usr/local/cuda/). Remember that this **cuDNN path** will be used later during the configuration of Tensorflow. 
```
sudo tar -xvf cudnn-8.0-linux-x64-v5.1-rc.tgz -C /usr/local
```

3. Install and upgrade PIP
```
sudo apt-get install python-pip python-dev
pip install --upgrade pip
```

4. Install Bazel
```
sudo apt-get install software-properties-common swig 
sudo add-apt-repository ppa:webupd8team/java 
sudo apt-get update 
sudo apt-get install oracle-java8-installer 
echo "deb http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list 
curl https://storage.googleapis.com/bazel-apt/doc/apt-key.pub.gpg | sudo apt-key add - 
sudo apt-get update 
sudo apt-get install bazel
```

5. Install Tensorflow
```
git clone https://github.com/tensorflow/tensorflow
cd tensorflow 
./configure
```
At this step, you should remember:
  - choose **YES** for **TensorFlow with GPU support**
  - specify **Cuda SDK version**, the newest is **8.0**
  - specify **CUDA toolkit path**
  - specify **cuDNN version**, the newest is **5**
  - specify **cuDNN path**
  - specify **compute capability** of your GPU card, for **GTX 1080** it should be **6.1**

Wait for some minutes for the configuration, then:
```
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package  
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

```


#### Reference  

(1) [Ubuntu partitioning schemes](https://help.ubuntu.com/community/PartitioningSchemes)  
(2) [Official Tensorflow installation](https://www.tensorflow.org/install/install_sources)  
(3) [NVIDIA manual for Tensorflow installation](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)


