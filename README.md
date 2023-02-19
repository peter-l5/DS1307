# DS1307

## MicroPython driver for the DS1307 real time clock

This module provides a MicroPython driver for the DS1307 real time clock. 

Acknowledgement: This module was derived in outline from the Adafruit circuit python DS1307 real time clock library. That library was authored by Philip R. Moyer and Radomir Dopieralski for Adafruit Industries. See repository: [Adafruit_CircuitPython_DS1307](https://github.com/adafruit/Adafruit_CircuitPython_DS1307.git).

Beware the DS1307 breakout is a 5V board but many boards have a 3.3v logic level. Make sure that the SCL and SDA pins on the board used are 5v tolerant or level shifted to 3.3v!

With the Raspberry Pi Pico, using the SoftI2C class instead of the hardware I2C class can avoid EIO errors that may be otherwise experienced, perhaps due to I2C clock stretching (see: [RP2: Hard I2C is intolerant of clock stretching](https://github.com/micropython/micropython/issues/8167)). 

## Version

This is a production version (v104).

## Usage

Here is an example of using the `DS1307` class.

first you will need to import the Pin and SoftI2C libraries to use the sensor
```
from machine import Pin, SoftI2C
import ds1307
```
once this is done you can define your `SoftI2C` object and define your `ds1307.DS1307` object
```
# uses SoftI2C class and pins for Raspberry Pi pico 
i2c0 = SoftI2C(scl=Pin(1), sda=Pin(0), freq=100000)
ds1307rtc = ds1307.DS1307(i2c0, 0x68)
```
now you can give the current time to the device.
```
# set time (year, month, day, hours. minutes, seconds, weekday: integer: 0-6 )
ds1307rtc.datetime = (2022, 12, 18, 18, 9, 17, 6)
```
you can access the current time accessing the `datetime` property.
```
current_time = ds1307rtc.datetime
```
the `datetimeRTC` property returns the current time in a format suitable for setting the Pico's internal real time clock (once the RTC module is imported).
```
from machine import RTC
machine.RTC().datetime(ds1307rtc.datetimeRTC)
```
finally, the disable oscillator property may be useful to stop the clock when not in use and reduce demand on the standby battery.

See also the example code provided in this repository.

## Features

The current time is set and access by means of properties. A 12 hour am/pm time stored in the DS1307 will be read as a 24 hour clock time. The time can only be set using 24-hour clock format. 

## Interfaces and tested displays 

The code includes an I2C interface. 

It has been tested with a Raspberry Pi Pico and an Adafruit DS1307 breakout. As the DS1307 works at 5V and the Pico uses 3.3V logic, an I2C-compatible level shifter breakout was also used. 
- [Adafruit DS1307](https://www.adafruit.com/product/264 "DS1307 Real Time Clock breakout board kit")
- [Adafruit 4-channel I2C-safe Bi-directional Logic Level Converter - BSS138](https://www.adafruit.com/product/757 "4-channel I2C-safe Bi-directional Logic Level Converter - BSS138") 

## Requirements

This code has been tested with MicroPython version 1.19.1.

## Datasheet

[DS1307.pdf at Analog industries](https://www.analog.com/media/en/technical-documentation/data-sheets/ds1307.pdf)

## Release notes

#### Version 104

Initial version
