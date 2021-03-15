# Raspi Mouse Jiggler

This repository hosts a Raspberry Pi Pico mouse jiggler project. It is an
alternative to my [Arduino Mouse Jiggler] repository.

[Arduino Mouse Jiggler]: https://github.com/TomasHubelbauer/arduino-mouse-jiggler

While researching how to implement this, I've found that someone has already
done exactly what I intended: [Raspberry Pi Pico - DIY USB Mouse Jiggler].

[Raspberry Pi Pico - DIY USB Mouse Jiggler]: https://www.youtube.com/watch?v=MjCFJCfq8ko

The person also made his code available on GitHub: [novaspirit/PicoMouseJiggler]

[novaspirit/PicoMouseJiggler]: https://github.com/novaspirit/PicoMouseJiggler

I'll use this as a guide and just try to reproduce his lead then.

1. Press and hold the <kbd>BOOTSEL</kbd> button on the Pi Pico
2. Connect the Pico to the computer using a data+power USB cable
3. Find the RPI-RP2 mass storage device that should mount and release the button
4. Download CircuitPython UF2 https://circuitpython.org/board/raspberry_pi_pico
5. Wait for the RPI-RP2 drive to unmount itself and reconnect it to the computer
6. Wait for a new device to mount called CIRCUITPY
7. Go to https://github.com/adafruit/Adafruit_CircuitPython_HID and download it
8. Move `adafruit_hid` from the downloaded repository over to `CIRCUITPY/lib`
9. Open `CIRCUITPY/code.py` and edit it to have this content:

```python
import time
import usb_hid
from adafruit_hid.mouse import Mouse

mouse = Mouse(usb_hid.devices)
while True:
  mouse.move(x=10, y=10)
  time.sleep(0.25)
  mouse.move(x=10, y=-10)
  time.sleep(0.25)
  mouse.move(x=-10, y=-10)
  time.sleep(0.25)
  mouse.move(x=-10, y=10)
  time.sleep(0.25)
```

There seems to be an issue with macOS currently:

https://github.com/adafruit/Adafruit_CircuitPython_HID/issues/59

Everything works as expected on Windows.

## To-Do

### Figure out a way to solve the macOS issue
