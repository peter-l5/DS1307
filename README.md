# DS1307

## MicroPython driver for the DS1307 real time clock

This module provides a MicroPython driver for the DS1307 real time clock. 

Acknowledgement: This module was derived in outline from the Adafruit circuit python DS1307 real time clock library. That library was authored by Philip R. Moyer and Radomir Dopieralski for Adafruit Industries. See repository: [Adafruit_CircuitPython_DS1307](https://github.com/adafruit/Adafruit_CircuitPython_DS1307.git).

Beware the DS1307 breakout works at is a 5V board but many micro-controller and computer boards have a 3.3v logic level. Make sure that the SCL and SDA pins on the board used are 5v tolerant or level shifted to 3.3v!

The accompanying example code uses the `SoftI2C` class. With the Raspberry Pi Pico, using the SoftI2C class instead of the hardware I2C class can avoid EIO errors that may be otherwise experienced, perhaps due to I2C clock stretching (see: [RP2: Hard I2C is intolerant of clock stretching](https://github.com/micropython/micropython/issues/8167)). 

## Version

This is a production version (v104).

## Usage

Here is an example of using the `DS1307` class.

First you will need to import the MicroPython Pin and SoftI2C modules to use the sensor
```
from machine import Pin, SoftI2C
import ds1307
```
Once this is done you can construct your `SoftI2C` object and  your `ds1307.DS1307` object
```
# uses SoftI2C class and pins for Raspberry Pi pico 
i2c0 = SoftI2C(scl=Pin(1), sda=Pin(0), freq=100000)
ds1307rtc = ds1307.DS1307(i2c0, 0x68)
```
Now you can give the current time to the DS1307 device using the class' `datetime` property. This takes a 7-tuple consisting of the year (4-digit format), month, day, hour (24-hour clock), minutes, seconds, weekday (in the range 0-6). Setting the time with this property also enables the clock's oscilator. The base for the weekday (that is zero) can be choosen according to user preferences. (The MicroPython port for the Pi pico sets Monday to zero for the pico internal RTC so Monday=0 may be a good choice for consistency.) 
```
# set time (year, month, day, hours. minutes, seconds, weekday: integer: 0-6 )
ds1307rtc.datetime = (2022, 12, 18, 18, 9, 17, 6)
```
You can access the current time from the DS1307 using the `datetime` property. This property returns the date, time and weekday as a 7-tuple as follows: 4-digit year, month, day, hour (24-hour clock), minutes, seconds, weekday (in the range 0-6).
```
current_time = ds1307rtc.datetime
```
The `datetimeRTC` property returns the current time from the DS1307 in a format suitable for setting the Pico's internal real time clock. This is as an 8-tuple with the date as the first three elements followed by a `None` value, then the time and finally another `None` value. In order to set the Pico's clock the machine.RTC class must be imported.  
```
from machine import RTC
machine.RTC().datetime(ds1307rtc.datetimeRTC)
```
Finally, the disable oscillator property may be useful to stop the clock when not in use and reduce demand on the standby battery.

See also the example code provided in this repository.

## Features

The current time is set and access by means of properties. A 12 hour am/pm time stored in the DS1307 will be read as a 24 hour clock time. The time can only be set using 24-hour clock format

## Interfaces and tested devices 

The code uses the I2C interface of the DS1307. 

This driver has been tested with a Raspberry Pi Pico and an Adafruit DS1307 breakout. As the DS1307 works at 5V and the Pico uses 3.3V logic, an I2C-compatible level shifter breakout was also used. 
- [Adafruit DS1307](https://www.adafruit.com/product/264 "DS1307 Real Time Clock breakout board kit")
- [Adafruit 4-channel I2C-safe Bi-directional Logic Level Converter - BSS138](https://www.adafruit.com/product/757 "4-channel I2C-safe Bi-directional Logic Level Converter - BSS138") 

## Requirements

This code has been tested with MicroPython version 1.19.1.

## Datasheet

[DS1307.pdf at Analog industries](https://www.analog.com/media/en/technical-documentation/data-sheets/ds1307.pdf)

## Release notes

#### Version 104

Initial version
