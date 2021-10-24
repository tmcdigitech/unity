---
title: CircuitPython
weight: 1
---

Let's get something happening on your Gemma.

All of these examples are complete programs, so make sure you copy them into Mu (or whatever editor you're using)
and run them as you go through this page.

## Flash the built-in LED
The first thing you try to get working on any embedded system is flashing a light. It's amazing how much information
you can convey with just a light if you get a little creative!

```python {linenos=table}
import board
import digitalio
import time

led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = True
    time.sleep(0.5)
    led.value = False
    time.sleep(0.5)

```

Lines 1-3 import various libraries that we will use. All of your Gemma programs will start with one or more of these import lines.

Line 5 gives us a reference to the pin that the built-in LED (the red one) is connected to. We store that reference in a [variable]({{< ref "/glossary/variable" >}}) called `led`.
Line 6 tells the Gemma that we want to use the pin we've called `led` as an output (to drive the red LED).

Lines 8-12 are an infinite loop. We turn the LED on, wait for half a second, turn it off, and wait another half second.
Then we do it all again, and again, and so on.

{{< hint info >}}
**Check your units**

Note that `time.sleep()` takes values in seconds, rather than milliseconds.
{{< /hint >}}

If you've written this in Mu, it should look like this:

![first program in Mu](https://cdn-learn.adafruit.com/assets/assets/000/105/704/medium800/circuitpython_WtCP_Mu_blink_example.png?1634763853)

## Flash the built-in RGB LED
Now we've got something happening, let's explore the built-in RGB LED. This model of LED is a Dotstar.
The LEDs on the strip we'll be using are a model called Neopixel.

```python {linenos=table}
import board
import adafruit_dotstar as dotstar
import time

dot = dotstar.DotStar(board.APA102_SCK, board.APA102_MOSI, 1, brightness=0.2)

while True:
    dot[0] = (255, 255, 0)
    time.sleep(0.5)
    dot[0] = (0, 0, 255)
    time.sleep(0.5)
```

Notice that we on line 5 we call the dotstar `dot`, but to use it we say `dot[0]`. That's because
Dotstars and Neopixels are designed to be chained together on a strip with common wires,
and you need a way to talk to each one individually even though they share a signal wire. Since
most counting in computers begins at zero, and there is just the one built-in dotstar, it is
called `dot[0]`. The `[]` notation is to do with lists, which you used when programming the micro:bit.