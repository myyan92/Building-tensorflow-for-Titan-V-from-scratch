# Building-tensorflow-for-Titan-V-from-scratch
building custom PC and deep learning environment for a Titan V GPU

I got a new Titan V GPU at NIPS2017 when it is annouced by NVIDIA CEO Jensen Huang. This document reports the process of building my PC from scratch and installing deep learning libraries including CUDA9.1, Cudnn7.0.5 and tensorflow1.5-rc0. At the time of this document I believe not a lot of Titan V GPUs has been shipped and used by researchers, and tensorflow support is not yet mature. I hope this document can help other early adopters of Titan V build their deep learning system faster.

1.1 Choosing parts for custom PC build.
Required parts to build your PC include the motherboard, CPU, CPU cooler, RAM, SSD, GPU, power supply and case. Optional parts include additional hard drive, CD ROM, wireless adapters, sound card, monitor, keyboard, mouse, etc. [PcPartPicker](https://pcpartpicker.com) is a good place to ensure compatibility of parts. I picked a gigabyte z370 gaming 5 motherboard, intel i7 8700k CPU, 32GB memory from corsair, WD 500G SSD, a cpu cooler capable of 150W, and a 850W power supply. All components took me 1.5k after tax.

1.2 Assemble the machine
There are enough materials online for steps and detailed instructions to assemble the machine, in addition to the user manuals from your motherboard, power supply, and case. I'll just briefly summarize the steps and some tips from my experience.

1. Install cpu on motherboard, take specail care to insert in correct orientation.
2. Install cpu cooler on the cpu. take care not to hurt your fingers with the sharp pins on the back side of the motherboard.
3. Insert memory on the motherboard.
4. Fix motherboard into the case, fully push the IO shield in place so that your motherboard can align with the stand-offs on the case.
5. Attach power cables to the power supply unit before fixing it into the case, especially if your case is tight in this area. For my build I need one Pcie cable for the GPU, one SATA cable for the SSD, and one peripheral cable for case fans.
6. Plan cable placement. I end-up plug and unplug many times before finally getting a good cable layout. Required cables include: (1) motherboard power (PSU to right edge of mobo)
(2) CPU power (PSU to upper-left corner of mobo)
(3) CPU fan power (mobo to cpu cooler)
(4) front panel cables (power switch, LEDs, USB3, audio) (lower and right edge of motherboard)
(5) case fan power (PSU to case fans)
(6) SSD data cable (SSD to right edge of mobo)
(7) SSD power cable (PSU to SSD)
Keep in mind you need to keep the GPU area clear.
7. Connect cables, connect power and boot your machine, check if you can reach BIOS.
8. Install GPU. For me installing the gpu makes the display stop working when no OS is on the machine. I installed Ubuntu before installing the GPU.

2.1 Installing Ubuntu
There are official tutorials on creating a bootable USB stick to install Ubuntu. I installed Ubuntu16.04.

2.2 Installing NVIDIA driver and CUDA9.1
This step gave me a lot of trouble and I had to reinstall OS four times for this. Here is the final way that works for me:

