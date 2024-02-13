---
title: "From the Archives: Troubleshooting a y-axis issue on the Flashforge Dreamer"
---

_Another one from the archives, hence the date. This one might actually still be relevant, assuming that I can find and reupload the datasheet that was previously linked._

I was having an issue with the y-axis in my 3D printer. It had suddenly started producing intense low frequency vibrations when running:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Qt6f9YaMT4M" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

__That... doesn't sound good.__

It was only happening along the y-axis, which helped to narrow down the problem. Part of the challenge, though, was that I couldn't find the manufacturer's datasheet for this stepper motor, the 17HD4063-05N, and that a much more common issue, that exhibits similar symptoms, was all that I could find when trying to narrow down the problem. I could rule out the common issue, a broken cable harness, by testing the continuity of the harness with my multimeter and I could rule out a bad driver by swapping drivers. So, I reached out to Flashforge support, and my support contact, Tang, found a datasheet for the 17HD4063-**06****N which I've linked to here: 

> File attachment missing-- needs to be found & reuploaded

The datasheet specifies a phase resistance of 5.75 ohms ± 10% at 20C. You can use a multimeter to measure the resistance of each phase, and in doing so, I found that I had one phase at about 6.6 ohms and the other around 3.5 ohms at room temperature. Resistance can vary by temperature, but we would expect the two phases to have roughly the same resistance. Based on this, I ordered a replacement stepper motor from Flashforge and, upon swapping out the part, found my problem resolved.

<iframe width="560" height="315" src="https://www.youtube.com/embed/y7dNhFqIzQs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Troubleshooting checklist for y-axis vibration issues on Flashforge Dreamer

__Symptom: Increased low frequency vibration in one axis. No/minimal skipped steps.__

1. Check unpowered action of axis by moving extruder slowly by hand. If smooth, check motor cables for continuity issues; check motor driver by swapping with known working driver.
1. Disengage motor from drive pulley and confirm smooth action of axis. If not smooth, lubricate linear bearings and re-check axis action.
1. Rotate disconnected motor shaft by hand. Rotation action should be smooth, with barely perceptible cogging and with minimal vibration perceptible through motor casing. If not smooth, rotation action should be characterized by: 
     - extremely difficult to turn/a "locked" feeling: indicates residual magnetism in the windings. Briefly short all leads to release residual magnetism, then repeat step 3.
     - easy to turn, but with a grinding or "chunky" feeling and with low frequency vibrations perceptible through motor casing. Possible indication of bad stepper motor.
1. With multimeter, check motor for winding shorts: measure resistance of each phase by connecting to pins 1 and 3, then 4 and 6. Datasheet reports phase resistance should be 5.75 ohms ± 10% at 20C. Higher temperatures may result in higher resistance. Phase resistance for the windings should be similar. Large difference between measured resistance and expected value or between measured phase resistances possibly indicate motor failure, especially if at least one measured resistance value is abnormally low.
