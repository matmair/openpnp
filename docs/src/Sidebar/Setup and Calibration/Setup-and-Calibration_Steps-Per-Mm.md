## Mechanical Machine Adjustments

Now that you can move the machine around and you have a camera it is the best moment to make sure your machine is mechanically adjusted, belts properly tensioned, etc. You will soon calibrate the machine and capture all sorts of measurements from it, so it is important that the frame of reference will not change afterwards. Also have a look at the [[Machine Squaring Guide]].

## Steps Per Mm

Once the mechanical side is finalized, it's a good time to check that your controller is moving the right amount when you tell it to. This is often part of your "steps per mm" configuration in most controllers. The way that you set this will be dependent on your controller, and you should check the instructions for the controller for more information. This setting is usually not configured from within OpenPnP, but it is possible to set this value for some controllers (for example, using an M92 command with the GcodeDriver as part of the homing command process).

## Testing Steps Per Mm

To test that your steps per mm settings are correct:

1. Place a [ruler](http://amzn.to/2642K3R) on the bed of the machine, along the X axis, so that it is visible to the camera. The ruler should be at least 100mm in length. The closer it is to the length of your axes the better your accuracy will be.
2. Jog the camera so that it is focused on the edge of the ruler.
3. Jog the camera back and forth along the length of the ruler and adjust the ruler on the bed until the camera crosshair tracks the edge perfectly.
4. Align the camera crosshair with one of the millimeter lines on the left of the ruler.
5. Jog 100mm to the right.
6. Check if the crosshairs now line up with the millimeter marking 100mm from where you started. If they do not, you will need to adjust your steps per mm setting. If the crosshairs went past 100mm then your steps per mm setting is too high. If they did not get all the way to the 100mm marking then the setting is too low.
7. When your X axis is adjusted correctly, perform the same test with the Y axis by turning the ruler 90 degrees.


***

| Previous Step                 | Jump To                 | Next Step                                   |
| ----------------------------- | ----------------------- | ------------------------------------------- |
| [Top Camera Setup](https://github.com/openpnp/openpnp/wiki/Setup-and-Calibration_Top-Camera-Setup) | [Table of Contents](https://github.com/openpnp/openpnp/wiki/Setup-and-Calibration) | [Nozzle Setup](https://github.com/openpnp/openpnp/wiki/Setup-and-Calibration_Nozzle-Setup) |