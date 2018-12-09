# i2c device

## Requirements

```shell
pip3 install RPI.GPIO
```
circuit python for busio and board modules
micropython included
```shell
pip3 install adafruit-blinka
```
VL6180X specific part library module for circuit python
```shell
pip install adafruit-circuitpython-vl6180x
```

The computer will install a few different libraries such as adafruit-pureio (our ioctl-only i2c library), spidev (for SPI interfacing), Adafruit-GPIO (for detecting your board) and of course adafruit-blinka


## Test CircuitPython installation
create a new file named blinkatest.py

```python
import board
import digitalio
import busio

print("Hello blinka!")

# Try to great a Digital input
pin = digitalio.DigitalInOut(board.D4)
print("Digital IO ok!")

# Try to create an I2C device
i2c = busio.I2C(board.SCL, board.SDA)
print("I2C ok!")

# Try to create an SPI device
spi = busio.SPI(board.SCLK, board.MOSI, board.MISO)
print("SPI ok!")

print("done!")
```



## i2c device 

```python3
# Demo of reading the range and lux from the VL6180x distance sensor and
# printing it every second.
# Author: Tony DiCola
import time
 
import board
import busio
 
import adafruit_vl6180x
 
 
# Create I2C bus.
i2c = busio.I2C(board.SCL, board.SDA)
 
# Create sensor instance.
sensor = adafruit_vl6180x.VL6180X(i2c)
 
# Main loop prints the range and lux every second:
while True:
    # Read the range in millimeters and print it.
    range_mm = sensor.range
    print('Range: {0}mm'.format(range_mm))
    # Read the light, note this requires specifying a gain value:
    # - adafruit_vl6180x.ALS_GAIN_1 = 1x
    # - adafruit_vl6180x.ALS_GAIN_1_25 = 1.25x
    # - adafruit_vl6180x.ALS_GAIN_1_67 = 1.67x
    # - adafruit_vl6180x.ALS_GAIN_2_5 = 2.5x
    # - adafruit_vl6180x.ALS_GAIN_5 = 5x
    # - adafruit_vl6180x.ALS_GAIN_10 = 10x
    # - adafruit_vl6180x.ALS_GAIN_20 = 20x
    # - adafruit_vl6180x.ALS_GAIN_40 = 40x
    light_lux = sensor.read_lux(adafruit_vl6180x.ALS_GAIN_1)
    print('Light (1x gain): {0}lux'.format(light_lux))
    # Delay for a second.
    time.sleep(1.0)
```
## Resources
* [adafruit](https://learn.adafruit.com/circuitpython-on-raspberrypi-linux/installing-circuitpython-on-raspberry-pi)
* https://learn.adafruit.com/adafruit-vl6180x-time-of-flight-micro-lidar-distance-sensor-breakout/python-circuitpython
