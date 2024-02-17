---
title: "From the Archives: The micro:bit and motors"
---

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Experimenting with controlling my <a href="https://twitter.com/codemyrobot?ref_src=twsrc%5Etfw">@codemyrobot</a> with a <a href="https://twitter.com/hashtag/microbit?src=hash&amp;ref_src=twsrc%5Etfw">#microbit</a> (on loan from <a href="https://twitter.com/KidsCoding?ref_src=twsrc%5Etfw">@KidsCoding</a> and <a href="https://twitter.com/afmcdnl?ref_src=twsrc%5Etfw">@afmcdnl</a>)! Excited for <a href="https://twitter.com/hashtag/LNDLCA?src=hash&amp;ref_src=twsrc%5Etfw">#LNDLCA</a> 2018 where I&#39;ll be working with educators to develop coding and computational thinking skills! (cc <a href="https://twitter.com/mshagerman?ref_src=twsrc%5Etfw">@mshagerman</a> <a href="https://twitter.com/Megan_C_K?ref_src=twsrc%5Etfw">@Megan_C_K</a> <a href="https://twitter.com/edukatik?ref_src=twsrc%5Etfw">@edukatik</a>) <a href="https://t.co/aYGmRuKPwH">pic.twitter.com/aYGmRuKPwH</a></p>&mdash; Jonathan (@jernwerber) <a href="https://twitter.com/jernwerber/status/1004767493716660224?ref_src=twsrc%5Etfw">June 7, 2018</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

_Another one from the archives, hence the date. Here I go into a bit more detail on a very common IC, the L9110 motor driver, and write some MicroPython that allows a micro:bit to function as a rudimentary controller for a robotics platform._

After a bit of tinkering, I made a short clip of a micro:bit being used to control a robot platform. One of the challenges of using the micro:bit for something like this is the limit on the amount of current (mA) the micro:bit can provide. In some cases, the voltage (max 3.3V) that it can provide is also an issue.

In this particular case, the platform (from _CodeMyRobot_) uses a separate chip (or integrated circuit/IC) to drive the two motors, one for each wheel. Usually, integrated circuits will have a model number written on them to identify them. In this case, the chip being used was an L9110. A quick Google search later, and I had found a description of the L9110, including how to control it[^1]. Once supplying it with power, two inputs could be used to control the direction of a motor:

| `Pin A` | `Pin B` | Output |
| ---- | ---- | ---- |
| `L` | `L` | Off |
| `H` | `L` | Forward |
| `L` | `H` | Backward |
| `H` | `H` | Off |

So, according to that table, if you wanted the motor to spin forward, you would set `Pin A` to `HIGH` and `Pin B` to `LOW`. On the micro:bit, if you're not using a breakout board, you have access to three pins along the bottom edge: `Pin 0`, `Pin 1`, and `Pin 2`. You also have a 3.3V power pin (which can either be an input or an output) and a ground pin.  We have to be the ones to tell the micro:bit which pins it should be using and how. In our case, we can say that `Pin 0` will correspond to `Pin A`, and `Pin 1` will correspond to `Pin B`. If we want the motor to run forward when, say, `Button A` is pressed, our MicroPython code might look something like:

```python
from microbit import *

while True:
    # Go forward
    if button_a.is_pressed(): 
        pin0.write_digital(1) # Pin A = High
        pin1.write_digital(0) # Pin B = Low
    
    # Stop
    else:
        pin0.write_digital(1) # Pin A = High
        pin1.write_digital(1) # Pin B = High
```

So, as long as Button A is pressed, the motor will run forward; as soon as it's not pressed, both Pin 0/A and Pin 1/B are set to "High" which, according to our table, means the motor shuts off. In the video above, both motors are using the same inputs, i.e. I'm sending the same signals to two L9110 chips, which make the motors attached to them behave the same way. This means, though, that I can only move forward or backward, because to turn, I need to have one motor running faster than the other, which means having them do different things. I can accomplish this through use of the third pin (Pin 2) and sharing one of the pins. Consider the following setup:

- we have our left motor as Motor A and our right motor as Motor B
- `Pin 0` is now `Pin A` for Motor A
- `Pin 1` is now `Pin A` for Motor B
- `Pin 2` is shared, and is `Pin B` for Motor A+B

Then, to get the micro:bit to put out the correct signals on its pins, we have to figure out what the desired outputs should be, as in below:

| Desired outputs | `Pin 0` | `Pin 1` | `Pin 2` |
| --------------- | ----- | ----- | ----- |
| Off | `L` | `L` | `L` |
| Forward (i.e. Motor A+B forward) | `H` | `H` | `L` |
| Backward (i.e. Motor A+B backward) | `L` | `L` | `H` |
| Left (i.e. Motor B only forward) | `L` | `H` | `L` |
| Right (i.e. Motor A only forward) | `H` | `L` | `L` |

The code below defines a few functions to make our lives easier if we wanted to reuse or combine any of these actions. It runs through a sequence of `forward()`, `back()`, `left()`, then `right()`, for 0.75 seconds each, with a 4 second `stop()` in between.

```python
from microbit import *

wait_amount = 750

motor_a_pin_a = pin0
motor_b_pin_a = pin1
motor_shared_pin = pin2

def forward():
    motor_a_pin_a.write_digital(1)
    motor_b_pin_a.write_digital(1)
    motor_shared_pin.write_digital(0)
    
def backward():
    motor_a_pin_a.write_digital(0)
    motor_b_pin_a.write_digital(0)
    motor_shared_pin.write_digital(1)
    
def left():
    motor_a_pin_a.write_digital(0)
    motor_b_pin_a.write_digital(1)
    motor_shared_pin.write_digital(0)
    
def right():
    motor_a_pin_a.write_digital(1)
    motor_b_pin_a.write_digital(0)
    motor_shared_pin.write_digital(0)

def stop():
    motor_a_pin_a.write_digital(0)
    motor_b_pin_a.write_digital(0)
    motor_shared_pin.write_digital(0)
    
while True:
    stop()
    display.show(Image.ASLEEP, wait=False)
    sleep(4000)
    
    forward()
    display.show(Image.ARROW_N, wait=False)
    sleep(wait_amount)
    
    backward()
    display.show(Image.ARROW_S, wait=False)
    sleep(wait_amount)
    
    left()
    display.show(Image.ARROW_W, wait=False)
    sleep(wait_amount)
    
    right()
    display.show(Image.ARROW_E, wait=False)
    sleep(wait_amount)
```

This would depend on your physical setup-- for example, when I plugged everything in the first time, everything was reversed. It was an easy switch--swapping the physical connections of Pins 0 and 1. You can see the video of the code running in this tweet:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/LNDLCA?src=hash&amp;ref_src=twsrc%5Etfw">#LNDLCA</a> in two short weeks! brushing up on my Python and micro:bit skills with a blog post about the motor drivers (<a href="https://t.co/e1tRu0HsyI">https://t.co/e1tRu0HsyI</a>) and a video (code included)! (cc @mshagerman <a href="https://twitter.com/afmcdnl?ref_src=twsrc%5Etfw">@afmcdnl</a> <a href="https://twitter.com/KidsCoding?ref_src=twsrc%5Etfw">@KidsCoding</a>) <a href="https://t.co/H2FdYnA9W0">pic.twitter.com/H2FdYnA9W0</a></p>&mdash; Jonathan (@jernwerber) <a href="https://twitter.com/jernwerber/status/1011658993708797953?ref_src=twsrc%5Etfw">June 26, 2018</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---

[^1]: One of the easiest ways to figure out what a device is capable of is to read (or at least search) the documentation. For example, how did I know how to "tell" the pins to be high or low? I checked the documentation for MicroPython, micro:bit, and how to use its pins: [http://microbit-micropython.readthedocs.io/en/latest/microbit_micropython_api.html#pins](http://microbit-micropython.readthedocs.io/en/latest/microbit_micropython_api.html#pins)