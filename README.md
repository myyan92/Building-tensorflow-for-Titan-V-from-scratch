# Building-tensorflow-for-Titan-V-from-scratch
building custom PC and deep learning environment for a Titan V GPU

I got a new Titan V GPU at NIPS2017 when it is annouced by NVIDIA CEO Jensen Huang. This document reports the process of building my PC from scratch and installing deep learning libraries including CUDA9.1, CuDNN7.0.5 and tensorflow1.5-rc0. At the time of this document I believe not a lot of Titan V GPUs have been shipped and used by researchers, and tensorflow support is not yet mature. I hope this document can help other early adopters of Titan V build their deep learning system faster.

## 1.1 Choosing parts for custom PC build.

Required parts to build your PC include the motherboard, CPU, CPU cooler, RAM, SSD, GPU, power supply and case. Optional parts include additional hard drive, CD ROM, wireless adapters, sound card, monitor, keyboard, mouse, etc. 

[PcPartPicker](https://pcpartpicker.com) is a good place to ensure compatibility of parts. I picked a gigabyte z370 gaming 5 motherboard, intel i7 8700k CPU, 32GB memory from corsair, WD 500G SSD, a cpu cooler capable of 150W, and a 850W power supply. All components took me 1.5k after tax.

## 1.2 Assemble the machine

There are enough materials online for steps and detailed instructions to assemble the machine, in addition to the user manuals from your motherboard, power supply, and case. I'll just briefly summarize the steps and some tips from my experience.

1. Install cpu on motherboard, take special care to insert in correct orientation.
2. Install cpu cooler on the cpu. take care not to hurt your fingers with the sharp pins on the back side of the motherboard.
3. Insert memory on the motherboard.
4. Fix motherboard into the case, fully push the IO shield in place so that your motherboard can align with the stand-offs on the case.
5. Attach power cables to the power supply unit before fixing it into the case, especially if your case is tight in this area. For my build I need one PCIE cable for the GPU, one SATA cable for the SSD, and one peripheral cable for case fans.
6. Plan cable placement. I end-up plug and unplug many times before finally getting a good cable layout. Required cables include:
(1) motherboard power (PSU to right edge of mobo)
(2) CPU power (PSU to upper-left corner of mobo)
(3) CPU fan power (mobo to cpu cooler)
(4) front panel cables (power switch, LEDs, USB3, audio) (lower and right edge of motherboard)
(5) case fan power (PSU to case fans)
(6) SSD data cable (SSD to right edge of mobo)
(7) SSD power cable (PSU to SSD)
Keep in mind you need to keep the GPU area clear. The connectors may have different positions on your motherboard.

7. Connect cables, connect power and boot your machine, check if you can reach BIOS.
8. Install GPU. For me installing the GPU makes the display stop working when no OS is on the machine. I installed Ubuntu before installing the GPU.

## 1.3 Optional tests
Just to be on the safe side, I want to verify all the hardware works well. I used checkbox to test all components work correctly, including audio, USB, keyboard and mouse, wifi, etc. Then I used stress and lm-sensors to make sure cpu cooler is working well and my cpu stays chill when heavily loaded. You can google how to use them, or choose other ways to test your build.

## 2.1 Installing Ubuntu
There are official tutorials on creating a bootable USB stick to install Ubuntu. I installed Ubuntu16.04.

## 2.2 Installing NVIDIA driver and CUDA9.1
This step gave me a lot of trouble and I had to reinstall OS four times for this. Here is the final way that works for me:

1. Download driver from [nvidia](http://www.nvidia.com/Download/index.aspx?). I selected Linux 64bit instead of Ubuntu16.04 (which will give a run file instead of deb file, however I believe both choices should work).
2. blacklist nouveau 

    create a file /etc/modprobe.d/disable-nouveau.conf and add these two lines:  
    ```
    blacklist nouveau  
    options nouveau modeset=0
    ```
    
3. reboot to level 3

    for Ubuntu16.04, run the following in terminal:  
    ```
    sudo systemctl enable multi-user.target
    sudo systemctl set-default multi-user.target
    reboot
    ```
    
4. press ctrl+alt+F2 after reboot to enter TTY2 and run:  
    ```
    cd Downloads
    chmod +x NVIDIA-Linux-x86-387.34.run
    ./NVIDIA-Linux-x86-387.34.run
    ```
    
    After installation finishes, check nvidia-smi can find your GPU.  
    To make sure the driver can work properly, reboot to level 5 by doing:  
    ```
    sudo systemctl enable graphical.target
    sudo systemctl set-default graphical.target
    reboot
    ```
    
5. Now I assume you can reboot into the GUI. Download CUDA9.1 run file (the deb file did not work for me and result in black screen upon reboot).
6. Check nouveau is blacklisted, and reboot to level 3.
7. install CUDA9.1 in TTY2, make sure you do not let the run file install driver.
8. check nvidia-smi can still find your GPU, and reboot to level 5.

However, each computer is different, the way that does not work for me (directly istall CUDA9.1 deb) may work for you.

## 2.3 Installing CuDNN7.0.5
Congratulations! You have get though the first hurdle I've gone through! It should be easy for you to install CuDNN I believe. Just download and do some copy-pasting.

## 2.4 Compile Tensorflow from source.
At the time of writing there are no pre-built tensorflow that work on CUDA9.1, which is recommended for Titan V. I believe they are actively working on it so you should check their github to see if you can just do a pip install to skip this step.

I mostly followed this [tutorial](http://www.python36.com/install-tensorflow141-gpu/) to compile tensorflow1.5-rc0, which is the latest at the time of writing. There are several notes:

1. git clone the latest tensorflow repo instead of the 1.4.1 archive to support CUDA9.1
2. Titan V has compute capability 7.0, it is not yet on the official list
3. If Bazel complains about libcublas not found, try this solution:  
    ```
    sudo echo "/usr/local/cuda-9.0/lib64" > /etc/ld.so.conf.d/cuda.conf
    sudo ldconfig
    ```
    
Bazel compilation finished in 20mins on my 6-core intel i7 cpu. Your time may be different.

TADA, you have finished building the basic deep learning environment!
