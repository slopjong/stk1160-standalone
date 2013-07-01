Update as of July 1st
=====================

Source updated to kernel 3.8 (taken from [here](http://www.cs.unca.edu/kernel/linux-3.8/drivers/media/usb/stk1160/)).

IMPORTANT UPDATE
================

    Completely deprecated, this driver is.
                                (Yoda)

The driver you are looking at is for Linux v3.2 **only**.
It will **not** work on any other kernel version. Please don't even
try unless you really know what you're doing.

Given stk1160 is fully mainlined since Linux v3.7 I've disabled issue tracking
for this github repository. If you have questions or want to report a bug
please use the video4linux mailing list (linux-media@vger.kernel.org) or
try asking in the IRC channel (#v4l).

If you want to stay updated with news about easycap devices you can
read the most excellent blog from Andrew Schalk.

  http://easycap.blogspot.com.ar/
  
I hope you enjoy using stk1160 as much as I've enjoyed writing it!

stk1160-standalone
==================

stk1160 release 0.9.3\_for\_v3.2

This branch is intended to be used with v3.2.
__Please don't even try to use it with another one__.

This is the stk1160 driver (formerly known as easycap driver).
It's the driver needed to capture audio and video in some of those
little devices named as Easycap (if they are based on stk1160 chip).

You need to have this modules installed, but don't worry,
you don't have to insert them manually:

- snd\_usb\_audio
- snd\_ac97\_codec
- ac97\_bus
- saa7115
- videodev
- videobuf2\_core
- videobuf2\_vmalloc
- videobuf2\_memops
- v4l2\_common

Building and installing
-----------------------

    make
    make install
    
After this, perhaps you will have to run 'depmod -a'.

You should *not* get any warning. 
If you do, most probably you did something wrong or you're using an incompatible kernel.

Usage
-----

After installing the driver you're ready to go.
If you now insert your usb easycap device you'll notice two modules get automatically loaded:
stk1160 and snd-usb-audio.
This can be seen using dmesg:

    $ dmesg | grep registered
    [48.561592] stk1160 2-2:1.0: V4L2 device registered as video0
    [48.562284] usbcore: registered new interface driver stk1160
    [49.169189] usbcore: registered new interface driver snd-usb-audio

Also, stk1160 will register a control-only alsa sound card.
This sound card is called 'stk1160-mixer'.
To enable sound capture you have to open alsamixer and selected
the 'Line' item on the capture menu for stk1160-mixer device.

This stk1160-mixer device is needed because snd-usb-audio
doesn't show mixer controls for the ac97 codec chip.
The real usb transfer for audio capture is done through snd-usb-audio.

Preventing old easycap driver from loading
------------------------------------------

If you want to prevent some module from loading
you can do so by adding it to blacklist file.

On many distributions this can be done like this:

    echo "blacklist easycap" >> /etc/modprobe.d/blacklist.conf

This way easycap won't be loaded automatically by udev;
you can still load it manually, of course.
