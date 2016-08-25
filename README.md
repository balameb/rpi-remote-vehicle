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
user interaction. The process is straight forward and can be followed at 
[waveshare](http://www.waveshare.com/wiki/5inch_HDMI_LCD).

## Testing with 16 channel Adafruit HAT

This is not the hat we'll be using but considering we have this one and there is 
better docs for it we'll run tests on this one first. The main reason to not use 
this module is that we'll be controlling two geared motors not servos.

## Configure I2C

>The I2C bus allows multiple devices to be connected to your Raspberry Pi, each 
with a unique address, that can often be set by changing jumper settings on the 
module. It is very useful to be able to see which devices are connected to your 
Pi as a way of making sure everything is working.

1. Install i2c tools and smbus.

```
sudo apt-get install python-smbus
sudo apt-get install i2c-tools
```

2. Run `sudo raspi-config` and under advanced options choose i2c and enable it. 
3. Reboot
4. Add i2c modules

``` 
# /etc/modules
i2c-bcm2708 
i2c-dev
``` 

5. Make sure we don't have i2c or spi blacklisted at 
`/etc/modprobe.d/raspi-blacklist.conf`.

6. Test with `sudo i2cdetect -y 1`. You should get a table with any pins that 
you are using mapped on it. 

Follow this Adafruit [guide](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c) 
for more details.

## Adafruit Python Library

While there is a `pip` version of the libraery we'll install it from source to 
have access to the example code. 

``` 
sudo apt-get install git build-essential python-dev
cd ~
mkdir code
cd code
git clone https://github.com/adafruit/Adafruit_Python_PCA9685.git
cd Adafruit_Python_PCA9685
sudo python setup.py install
```

Turn off the rpi, attach the Motor HAT and the pin conectors for our seros. I'm 
using a Tower Price Micro Server 9g and connecting it to channel 0 where ground 
is at the bottom of the pins. Finally make sure you have an external power 
source pluged in. We're using a 5V DC jack, if you've connected them correctly 
we'll see a green LED turn on.

Run the sample code to verify motor setup.

```
# Assuming we're at ~/code/Adafruit_Python_PCA9685
cd examples
sudo python simpletest.py
```

You'll have the servo motort turn clock and counter clockwise. 

## Battery Holder

Cut a out a box from cartboard for the amount of bateries you'd like to use for 
your motors, 5 AA in my case for an 8V output. Then wrap a few (i.e. four) small 
pieces of cardboard to connect the batteries in series. Finally attack a couple 
of wires to bothe free extremes and you're done.

## DC and Stepper Motor HAT

The main difference fro this HAT is that we can also controls DC motors which 
we'll be using for each rear wheel. Another thing to notice is that you can feed 
it with 6 to 12V. 

Example code and library can be downloaded as follows:

```
git clone https://github.com/adafruit/Adafruit-Motor-HAT-Python-Library.git
cd Adafruit-Motor-HAT-Python-Library
sudo apt-get install python-dev
sudo python setup.py install
```

Now try connecting a DC motor to M3 and plugin the battery source. You can run 
the DC motor example code by using this:

```
cd examples
sudo python DCTest.py 
```

The motoraxis should spin in one direction, stop, move to the opposite direction 
and repeat the cycle. 


## External resources
*  http://blog.mivia.dk/solved-hdmi-working-raspberry-pi/ 
*  https://www.raspberrypi.org/downloads/raspbian/ 
*  https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c 
*  https://learn.adafruit.com/adafruit-16-channel-pwm-servo-hat-for-raspberry-pi/attach-and-test-the-hat 
*  https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
*  http://www.waveshare.com/wiki/5inch_HDMI_LCD 
*  https://github.com/adafruit/Adafruit-Raspberry-Pi-Python-Code
*  https://learn.adafruit.com/adafruit-16-channel-pwm-servo-hat-for-raspberry-pi/downloads 
*  https://learn.adafruit.com/adafruit-dc-and-stepper-motor-hat-for-raspberry-pi/using-stepper-motors?view=all