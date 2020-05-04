---
title: Setting up networked photo frame with Raspberry Pi
tags: raspberrypi, photography
---

Having set up audio streaming with my RPi that was collecting dust (since my last post :), 
I ended up with a left-over RPi Zero W. And we finally decided to showcase all family photos, perfect timing!
Setting up a networked photo frame has a couple of quirks to it...

<!--break-->
## The plan
The idea is to show _all_ pictures from a network file share, and have non-interactive slideshow 
running from RPi & connected HDMI monitor

## Network share
I am using my always-on windows machine - I have set up separate user just for this purpose and shared the folder.
As optimization step, I also made a list of all jpeg files in the folder - as enumerating them over the network takes time.
This command dumps and shuffles the list (I am running this in the WSL console):
```
find . -name "*.jpg" | shuf --output images.txt
```

## RPi setup

### Distro
There are several tools that can display slideshows - some work using framebuffer (so no X windows required). 
I have been experimenting with them all and so far stopped at `feh` - it runs under X, so I installed full Raspbian distribution,
which conviniently auto-logs you in.

### RPi config
* Using raspi-config, enable SSH so you can configure the frame remotely (it's under Interfacing options)
* Also under Interfacing options enable VNC, so you can see what's going on remotely and control the terminal attached to graphical console. 
(you can do that from ssh, but this is more straightforward)
* If you already connected your screen and see black border, go to Advanced options and enable Overscan
* Under Boot options enable "Wait for Network to boot" - we'll need this later to auto-mount network share
* Enable auto login - go to Boot Options, and in "B1 Desktop / CLI" pick  "B4 Desktop Autologin"

### Mounting network share
There are couple of steps here - first test that you can mount your network share you set up:

Set up folder where you will be mounting, for example:
```
mkdir /mnt/photos 
```

Test that you can mount:
```
sudo mount -t cifs -o username=share,noserverino //192.168.11.102/Photos /mnt/photos
```

What are the options here:
* cifs - this is windows file sharing, or samba, protocol. Note that if you want to use NFS, you put NFS here.
* username=share - that's the user I set up on my server, yours can be different
* noserverino - super duper important option - if your server has >1TB disk, you'll have crashes with all the slide 
show tools I mentioned, as on 32 bit RPi they are not compiled to support this size.
* the IP address - since RPi doesn't really participate in network discovery, I fixed the IP of my server 
(in windows you go and assign IP address manually)
* /mnt/photos - that's where your photos will show up

When you run the command above it will ask you for password - type it in, and you should see your photos in /mnt/photos

### Automatically mounting
We want the RPi to restart unattended, so let's set this up:
```
sudo vi /etc/fstab
```

This file contains the partitions to mount at startup, so let's add ours here. 
The options from mount command also go into this file:

```//192.168.11.102/Photos /mnt/photos     cifs    username=share,noserverino,credentials=/home/pi/.smpasswd       0       0```

(that's tabs between the parameters)

You'll have to make the file /home/pi/.smpasswd, that's what i have there:
```
username=share
password=somepassword
```

If you are worred about someone logging into your photo frame and stealing this password you can store it elsewhere and 
only give root access - I just made special user in windows that can only see photos, so no worry.

Restart, and you should see your /mnt/photos populated with pictures - if not, check dmesg. 
Remember to enable this "wait for network to boot" option i mentioned before!

## Show the pictures!
As I mentioned before, I tried different tools - fbi, fim, eos and feh. The one that can accept list of files and has auto rotation seems to be feh (that's TBD, but this works fine). Here's the command I am using:
```
cd /mnt/photos
cat images.txt | feh --auto-rotate -Y -x -q -D 5 -B black -F -Z -z -r -f - 
```

Remember that I made image list on the server before. You can skip this, but I can't -  have 70k images with many subfolders. If you skip this, you can use --recursive option in feh instead.

### Automatically starting slideshow on start
Simply calling the script above is enough when starting RPi (remember that it logs you into X windows automatically). Create this folder structure and file and mention the script with feh command there:
```
mkdir -p ~/.config/lxsession/LXDE-pi
vi ~/.config/lxsession/LXDE-pi/autostart
```

this is all I have in the autostart file:
```
/home/pi/show.sh
```

## Done
At this point you should be able to restart RPi at any time and have slideshow come up.
In case of trouble your diagnostics is dmesg (for mounting folders) and running feh command manually.
