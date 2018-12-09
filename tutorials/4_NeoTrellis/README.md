

## Requirements
Have circuitpython installed using
```shell
pip3 install adafruit-blinka
```

### Device library
Install the NeoTrellis library from adafruit
```shell
pip3 install adafruit-circuitpython-trellism4
```

## Usage Example
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
