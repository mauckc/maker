
## Resources
* [NeoTrellis](https://learn.adafruit.com/adafruit-neotrellis?view=all)

* [adafruit NeoTrellis M4 with Enclocsure and Buttons Pack](https://www.adafruit.com/product/4020)
* [PCB-and-Enclosure Eagle CAD files](https://github.com/adafruit/Adafruit-NeoTrellis-M4-PCB-and-Enclosure)

## Requirements
Have circuitpython installed using
```shell
pip3 install adafruit-blinka
```

### Device libraries you either have NeoTrellis 4x4 or NeoTrellis m4
Install the NeoTrellis library from adafruit
```shell
pip3 install adafruit-circuitpython-neotrellis
```
or 

Install the Trellis m4 library from adafruit
```shell
pip3 install adafruit-circuitpython-trellism4
```

##  Usage Example

### NeoTrellis 4x4

```python
import time
 
from board import SCL, SDA
import busio
from adafruit_neotrellis.neotrellis import NeoTrellis
 
#create the i2c object for the trellis
i2c_bus = busio.I2C(SCL, SDA)
 
#create the trellis
trellis = NeoTrellis(i2c_bus)
 
#some color definitions
OFF = (0, 0, 0)
RED = (255, 0, 0)
YELLOW = (255, 150, 0)
GREEN = (0, 255, 0)
CYAN = (0, 255, 255)
BLUE = (0, 0, 255)
PURPLE = (180, 0, 255)
 
#this will be called when button events are received
def blink(event):
    #turn the LED on when a rising edge is detected
    if event.edge == NeoTrellis.EDGE_RISING:
        trellis.pixels[event.number] = CYAN
    #turn the LED off when a rising edge is detected
    elif event.edge == NeoTrellis.EDGE_FALLING:
        trellis.pixels[event.number] = OFF
 
for i in range(16):
    #activate rising edge events on all keys
    trellis.activate_key(i, NeoTrellis.EDGE_RISING)
    #activate falling edge events on all keys
    trellis.activate_key(i, NeoTrellis.EDGE_FALLING)
    #set all keys to trigger the blink callback
    trellis.callbacks[i] = blink
 
    #cycle the LEDs on startup
    trellis.pixels[i] = PURPLE
    time.sleep(.05)
 
for i in range(16):
    trellis.pixels[i] = OFF
    time.sleep(.05)
 
while True:
    #call the sync function call any triggered callbacks
    trellis.sync()
    #the trellis can only be read every 17 millisecons or so
    time.sleep(.02)
```

### Trellis m4 
This example prints out the coordinates of a button each time it is pressed and released:
```python3
import time
import adafruit_trellism4

trellis = adafruit_trellism4.TrellisM4Express()

current_press = set()
while True:
    pressed = set(trellis.pressed_keys)
    for press in pressed - current_press:
        print("Pressed:", press)
    for release in current_press - pressed:
        print("Released:", release)
    time.sleep(0.08)
    current_press = pressed
```

