---
layout: default
modal-id: 6
date: 2017-04-25
img: game.png
alt: img-alt
project-date: 2017

---
If you simply want to use off-the-shelf `opencv` with python you can get it by using `apt-get` or `pip` as following:

1. For `opencv2`
```
sudo apt-get install python-opencv
```

2. For `opencv3`
```
pip install opencv-python
```

Please note the different in the order of `python` and `opencv` of those 2 packages. They took me days to figure out!

If you are a real geek, who wants to build everything from source, and moreover you want to use some packages excluding from the `shipped` packages (e.g. gui in opencv3, SIFT, SUFT), you are welcome to `git clone` opencv and build from the source. You can follow the detail [manual](http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/). In short:  

1. Install requirements
```
sudo apt-get update  
sudo apt-get upgrade  
sudo apt-get install build-essential cmake pkg-config  
sudo apt-get install libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev  
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev  
sudo apt-get install libxvidcore-dev libx264-dev  
sudo apt-get install libgtk-3-dev  
sudo apt-get install libatlas-base-dev gfortran  
sudo apt-get install python2.7-dev python3.5-dev  
```
2. Download and install opencv from source
```
cd <PATH_TO_CONTAIN_OPENCV>
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
```
3. Configure `opencv` with `OPENCV_EXTRA_MODULES_PATH` pointing to the path to `opencv_contrib/modules`
4. Compile `opencv`
5. If python cannot find out the `cv2` from `opencv`, you should symbolic link `cv2.so` bindings to `python library path`, e.g.:
```
sudo ln -s /usr/local/lib/python2.7/site-packages/cv2.so /usr/lib/python2.7/cv2.so
```


#### Reference  




