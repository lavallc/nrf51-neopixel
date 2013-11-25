nrf51-neopixel
==============

This is a library for using WS2812(B) NeoPixels with a Nordic Semiconductor NRF51822 Bluetooth Low Energy ARM SoC.  

Developed by Lava, LLC.

[Adafruit NeoPixel Strip](http://www.adafruit.com/products/1138)

[Nordic Semiconductor 51822](http://www.nordicsemi.com/eng/Products/Bluetooth-R-low-energy/nRF51822)

[Lava](http://lava.io)

##Example code:##
```
neopixel_strip_t m_strip;
uint8_t dig_pin_num = 6;
uint8_t leds_per_strip = 24;
uint8_t error;
uint8_t led_to_enable = 10;
uint8_t red = 255;
uint8_t green = 0;
uint8_t blue = 159;

neopixel_init(&m_strip, dig_pin_num, leds_per_strip);
neopixel_clear(&m_strip);
error = neopixel_set_color_and_show(&m_strip, led_to_enable, red, green, blue);
if (error) {
	//led_to_enable was not within number leds_per_strip
}
//clear and remove strip
neopixel_clear(&m_strip);
neopixel_destroy(&m_strip);
```
 
##For use with Nordic S110 SoftDevice:##

Include in main.c

```
#include "ble_radio_notification.h"
```

Setup the radio notification with a callback handler(see nrf_soc.h: NRF_RADIO_NOTIFICATION_DISTANCES and NRF_APP_PRIORITIES)

```
ble_radio_notification_init(NRF_APP_PRIORITY_xxx,
							NRF_RADIO_NOTIFICATION_DISTANCE_xxx,
							your_radio_callback_handler);
```

Create the callback handler

```
void your_radio_callback_handler(bool radio_active)
{
	if (radio_active == false)
	{
		neopixel_show(&strip1);
		neopixel_show(&strip2);
		//...etc
	}
}
```

Write to LEDs

```
uint8_t neopixel_set_color(...);
```

__Do NOT use neopixel_set_color_and_show(...) with BLE. The LEDs should only be written to in the radio notification callback.__


##The MIT License (MIT)##
```
Copyright (c) 2013 lava

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
