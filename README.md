# Dev Logs

Configuring rpi through HDMI monitor, mouse and keyboard. The rpi has a wifi 
and bluetooth dongle and runs Rasbian on a 8GB SD card.  

HDMI revieved no signal so we opened the `config.txt` from the SD card throgh an
SD card reader. On the file added the following lines:  

``` 
hdmi_force_hotplug=1
hdmi_drive=2
``` 

We'll be connecting through SSH so we setup Raspbian with our Wifi network. All 
other configs will happen through SSH.

## Connect to through SSH

The challenge is getting the ip for which there are various options. What works 
best for me is to access my router and get the list of DHCP clients; client is 
called raspberrypi.

```
ssh pi@<raspberrypi-ip>
pi@192.168.0.18's password: raspberry
``` 

After you've logged in update your system:

```
sudo apt-get update
sudo apt-get upgrade 
```

## Configure HDMI Touchscreen (optional)

As an optional step we configured a Waveshare 5inch touchscreen HDMI to improve 
user interaction. Theprocess is straight forward and can be followed with its 
respective link found in the External resources section below.

## Testing with 16 channel Adafruit HAT

This is not the hat we'll be using but considering we have this one and there is 
better docs for it we'll run tests on this one first. The main reason to not use 
this module is that we'll be controlling two geared motors not servos.

## Configure I2C



## External resources
*  http://blog.mivia.dk/solved-hdmi-working-raspberry-pi/ 
*  https://www.raspberrypi.org/downloads/raspbian/ 
*  https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c 
*  https://learn.adafruit.com/adafruit-16-channel-pwm-servo-hat-for-raspberry-pi/attach-and-test-the-hat 
*  https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
*  http://www.waveshare.com/wiki/5inch_HDMI_LCD