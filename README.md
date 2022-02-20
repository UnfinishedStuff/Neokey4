# Neokey4
Documentation for using Adafruit's NeoKey 4 over I2C

Adafruit's [Neokey 1x4](https://www.adafruit.com/product/4980) is a neat little pad with 4 mechanical keyswitches, each with their own Neopixel to light it up.  Unfortunately, the default example is a bit pants.  It doesn't use the interrupt pin, so the latency between pressing a button and getting the LED to light can be quite high.  This is documentation on tests for manually setting this up to use the interrupt, as well as manually setting up everything else.

The current state is that it is pretty rough and incomplete.

The keypad and neopixel controls seem to be from separate modules
```python
from adafruit_seesaw import keypad, neopixel
```

For the ESP32-S2 QTPY, the Stemma QT socket is NOT connected to the same I2C bus as the I2C pins.  These need to be manually set up, rather than using the in-built I2C function which defaults to the standard I2C pins.
```python
from adafruit_bus_device.i2c_device import I2CDevice
i2c = busio.I2C(board.SCL1, board.SDA1)
```

The whole board is controlled by a SeeSaw chip, so that needs initialised.
```python
ss = keypad.Keypad(i2c, addr=0x30)
```

According to the Neokey 1x4 guide, the NeoPixels are connected to Pin 3.  There are also 4 of them.  Create an object called 'pixel' to control these

```python
pixelPin = 3
numPixels = 4
pixel = neopixel.NeoPixel(ss, pixelPin, numPixels, pixel_order=neopixel.GRB)
```

To fill all of the pixels, use the 'fill' command:
```python
# Set all pixels to 50% Blue
pixel.fill((0,0,128))
```

Set pixel 0 to 50% red:
```python
pixel.__setitem__(0,(128,0,0))

```
