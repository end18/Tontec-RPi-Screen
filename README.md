Tontec-RPi-Screen
=================

How to set up a Tontec screen on a Raspberry Pi. I used [this screen on Amazon](http://www.amazon.com/gp/product/B00LN9MYCO).

All of these commands assume sudo/root. Very basic Linux terminology/usage expected.

# Setup

## System

### Install Packages

    aptitude update
    aptitude install build-essential zip

### Set Screen Resolution

    nano /boot/config.txt

Change:

    #framebuffer_width=1280
    #framebuffer_height=720

To:

    framebuffer_width=480
    framebuffer_height=320

## Screen

### Get Driver

    cd ~/
    wget https://s3.amazonaws.com/ttbox/35screen.zip
    unzip 35screen.zip
    unzip mzl350i-pi-ext.zip
    rm 35screen.zip mzl350i-pi-ext.zip

### Edit Driver Config

    cd ~/mzl350i-pi-ext/src
    mv mzl350i.c mzl350i.c.old
    nano mzl350i.c

Paste the contents of [this](https://github.com/33mhz/Tontec-RPi-Screen/blob/master/mzl350i.c).

Save the file, and...

    make

### Set to Run on Startup

    crontab -e

Add:

    @reboot /home/pi/mzl350i-pi-ext/lcd
    
Note: For whatever reason, it would not work if I followed their procedure with the script in /etc/init.d, and this does work.

### Prevent Screen From Timing Out (Optional)

    nano /etc/kbd/config

SET:

    BLANK_TIME=0
    
    POWERDOWN_TIME=0

## Terminal

### Set Font Size (Optional)

    sudo dpkg-reconfigure console-setup

    CHARMAP="UTF-8"

    CODESET="Uni2"
    FONTFACE="Terminus"
    FONTSIZE="14x28"

### Disable Raspberry Logo (Optional)

    nano /boot/cmdline.txt

Add:

    logo.nologo

### Automatically Log In (Optional)

    nano /etc/inittab

Change:

    1:2345:respawn:/sbin/getty --noclear 38400 tty1

To:

    1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2>&1



And you're done! Rebooting should cough it out to a command prompt.


It takes a while for it to turn on with the screen. Apparently the screen takes a bit of resources... But works like a charm for me once on. I boot it to terminal and run console programs on these displays.
