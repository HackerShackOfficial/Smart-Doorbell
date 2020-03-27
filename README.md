# Smart-Doorbell
A wifi-connected raspberry pi doorbell with video chat

Github repository for the Smart Doorbell Hackershack video

This project was run on a Raspberry Pi 3 with Raspbian Buster. The video call wasn't as smooth as I would've liked, so it would probably be better to run this on a Pi 4. However, I haven't run this on the Pi 4, so I can't guarantee that it works.

## Instructions

###  Enable the Camera

Enable the camera with raspi-config

In the terminal, type:

```
sudo raspi-config
```

Navigate to `Interfacing Options`

Navigate to `camera`

Select `yes` and reboot

Test that the camera is working by running the following command in the terminal to save an image to your home directory

```
sudo raspistill -o test.jpg
```

### Enable the Microphone

Open the sound input settings

```
alsamixer -c l
```

Press `F4` to open the capture settings and bump the level up to 100. Press `Esc` to exit.

Test the audio capture with the following command:

```
arecord --device=hw:1,0 --format S16_LE --rate 44100 -c1 test.wav -V mono
```

press `control-c` to stop. It will generate a audio file in your local directory called `test.wav`

You can play the file with

```
aplay test.wav
```

Store the settings so that they persist on reboot

```
sudo alsactl store
```

### Enable Video Calls

Visit [Jitsi Meet](https://meet.jit.si/) and configure the site to use your camera/microphone

My settings were:

```
Camera: mmal service 16.1
Microphone: USB PnP Sound Device, USB Audio-Default Audio Device
```

### Download the Code

I navigated to the repo from my Raspberry Pi in the browser and clicked "Download Zip". I extracted the package to my home directory and them moved the doorbell.py file to `/home/pi/doorbell.py`.

You can test that the program works by running

```
python doorbell.py
```

from `/home/pi`.

To edit the file, install vim and open the file

```
apt-get install vim

vim doorbell.py
```

press, `i` to enter edit mode then make your changes. To exit, press `esc` then upper-case `ZZ` to save.

The variables for the code are documented at the top. Feel free to change any of these for your program. 

### Rotate the Screen

Open the boot.txt file with vim

```
sudo vim /boot/config.txt
```

press `i` to enter insert mode then add a line

```
display_rotate=1
```

press `esc` then `ZZ` to save and exit. Reboot.


### Start the Program on Boot

Create a autostart directory

```
mkdir /home/pi/.config/autostart
```

Create a file to run the doorbell program

```
vim /home/pi/.config/autostart/doorbell.desktop
```

press `i`, then add the following to the file

```
[Desktop Entry]
Type=Application
Name=Doorbell
Exec=python /home/pi/doorbell
```

press `esc` and `ZZ` to save and exit.

*WARNING: This will make your local display unusable. Make sure you have a way to connect to the pi through VNC or SSH to remove this file if you want to stop the program from starting at boot.

Reboot.

If you have any issues getting the doorbell to start, please leave an issue on this repository and the community will help you get it working.


