The motion controller is the interface between OpenPnP and your hardware. It translates OpenPnP's movements commands into electrical signals that control your motors.

# What is a Motion Controller?

The term motion controller can be a little confusing, and it means a few different things. People often use the phrase "motion controller" to mean the actual board that you install in your machine, or it can mean the software (or firmware) that runs on the board. It can even mean a completely computer based system like LinuxCNC.

When we say motion controller we just mean "whatever OpenPnP sends commands to to make your hardware move."

# Choosing a Motion Controller

Choosing a motion controller for your machine is a very important decision. The motion controller will determine how fast the machine moves, how many motors it can control, how many outputs are available for things like solenoids and pumps, etc.

In general, the most important decision is to pick a motion controller that supports as many motors as you need on your machine. A basic pick and place machine has 4 motors, or axes. They are X, Y, Z and C, which is also called Rotation. X and Y move the head around, Z moves the nozzle up and down and C rotates the nozzle. More complex machines may have multiple Z axes and multiple C axes. These are referred to as Z2, Z3, C2, etc.

Another important consideration is making sure that the motion controller has enough outputs to control your various peripherals. The most basic PnP will have 1 output; usually a nozzle vacuum solenoid. More complex machines may have additional solenoids for exhaust and blow off, switches for pumps and lights, solenoids for feeders, etc.

There are a *lot* of motion controllers available, from Open Source software that runs on an Arduino and a shield, to all in one systems like Smoothie, all the way up to closed loop, high power servo controllers. Most people find that something in the middle works well.

# Recommended Models

## Smoothie

Based on years of evidence, and lots and lots of different builds, we recommend a [Smoothie](http://smoothieware.org/) based board for most machines. Smoothie is an Open Source motion controller firmware that runs on a variety of affordable, all in one boards. It's easy to configure, well documented and works great with OpenPnP.

Some Smoothie based boards that are known to work with OpenPnP, and which you can buy online are:
* **Smoothieboard**: http://smoothieware.org/getting-smoothieboard
    The original. Buying this board helps support the creators of Smoothie. Available with up to 5 stepper drivers and 6 MOSFET outputs.
* **Rapid Star Board**: https://www.deltaprintr.com/product/rapid-star-board/
    Created by Deltaprintr, this board is a remix of the smoothie board but has features tailored specifically towards PNP machines and supports up to 6 stepper drivers with many additional features.
* **Cohesion3D Remix**: http://cohesion3d.com/cohesion3d-remix/
    Created by an OpenPnP forum member, this board is designed with PnP in mind and has up to 6 stepper drivers and 6 MOSFET outputs. This board is great for larger, more complex machines.
* **Azteeg**: http://www.panucatt.com/default.asp
    Panucatt Devices sells a series of Smoothie based boards called Azteeg. There is a review [here](https://makr.zone/choosing-a-motion-controller-the-panucatt-azteeg-x5-gt-32bit/455/).
* **Re-Arm**: http://www.panucatt.com/default.asp
    Panucatt Devices also sells the Re-Arm, which is a Smoothie based board in the form factor of an Arduino Mega. This allows it to be used with existing RAMPS 1.4 boards which are common in the 3D printer world. 

**⚠ WARNING ⚠** do not buy the illegitimate clones of the Smoothieboard that are typically offered in Chinese online-shops. These are known to violate Open Source licenses and brand names, they use inferior/sub-spec and counterfeit components, inadequate copper layers etc. They are known to fail with OpenPnP. **We will not provide support for these boards.** See the discussions [here](https://groups.google.com/g/openpnp/c/rdAXltRoSdc/m/lPNkWLX4BQAJ) and [here](https://groups.google.com/g/openpnp/c/4LswIzPOfpU/m/gopdUoiPAAAJ).   

See [this FAQ](http://smoothieware.org/troubleshooting#what-is-wrong-with-mks) for more information on why. 



### Using with a Peter's Head and Advanced Motion Control

There is a common style of pick and place often referred to as "Peter's Head". This style of head has one Z axis motor which uses belts or gears to drive two nozzles. Due to the complex homing operation required for this type of head, the Smoothie firmware had to be extended to support it. In addition, for use with new Advanced Motion Control features in OpenPnP, a special PnP Smoothie firmware version was developed and is now a requirement:

Please see the [Firmwares page](https://github.com/openpnp/openpnp/wiki/Motion-Controller-Firmwares#smoothieware).

## Duet + RepRapFirmware

After having been specifically extended to support OpenPnP very well, Duet 2 and Duet 3 and RepRapFirmware (starting from 3.3beta) are now also formidable platforms for OpenPnP. 

See the [Firmwares page](https://github.com/openpnp/openpnp/wiki/Motion-Controller-Firmwares#duet3d).

Those interested in using Duet 2 or Duet3 with OpenPnP, for help and support please contact dc42 on the OpenPnP forum at https://groups.google.com/forum/#!forum/openpnp or the Duet3D forum at https://forum.duet3d.com/.

## TinyG

[TinyG](http://synthetos.myshopify.com/products/tinyg) is another great Open Source motion control platform. It supports up to 6 axes and is one of the only ones to support S-curve acceleration. This makes its motion very smooth and can allow for faster accelerations without losing steps. The TinyG board only has 4 stepper drivers, but if that's all you need then it's an excellent choice.

Unfortunately, due to some strange quirks in TinyG, it's not a great solution for OpenPnP out of the box. Some problems have been resolved by creating a special PnP firmware version, [see the Firmwares page](https://github.com/openpnp/openpnp/wiki/Motion-Controller-Firmwares#tinyg). Using the standard firmware is no longer recommended, it requires some hacking that is not fully documented. For more information, see [TinyG Quirks](https://github.com/openpnp/openpnp/wiki/TinyG#quirks)

## Grbl

[Grbl](https://github.com/gnea/grbl) is an Open Source motion control system for the Arduino platform. Grbl is very easy to get up and running and can be considered the cheapest option, but it only supports 3 axes by default. This makes it not ideal for pick and place since it leaves you without an option to rotate the nozzle. There is a modification of Grbl available [here](https://github.com/openpnp/grbl) but it is out of date, unsupported and somewhat buggy. You can use it in a pinch if it's all you have, but it's not recommended.

## Aprinter

[Aprinter](https://github.com/ambrop72/aprinter) is a modern 3D printer firmware that may be useful for OpenPnP. I don't think of any machines using it yet, but the features list says all the right things and it may be worth looking into.

## Marlin and Other 3D Printer Firmwares

Every 3D printer is by definition at least a 4 axis machine and this makes 3D printer firmware tempting for pick and place motion control. The most popular of the bunch is [Marlin](https://github.com/MarlinFirmware/Marlin). Marlin can be used with OpenPnP but it has some inherent limitations based on it's focus on 3D printing. It can be difficult to get acceleration and maximum velocity set up correctly since these are often tied together on a 3D printer. In addition, configuration is complex because you have to remove a lot of the 3D printing functionality. 

It's not recommended to use 3D printer firmware with OpenPnP, but if you work hard enough it can be made to work.

# Drivers

In OpenPnP, the driver is the piece of the program that talks to the motion controller. The driver converts OpenPnP command to motion controller commands. In all of the cases above you should use the [[GcodeDriver]]. It is a flexible, well supported and well documented driver that can be used with almost any motion controller.

OpenPnP also contains a number of motion controller specific drivers such as GrblDriver, OpenBuildsDriver, MarlinDriver, etc. These should only be used if you are using a machine specifically designed for these drivers. In almost every case the GcodeDriver will be a better fit.

There is more information about setting up your driver in [[Setup and Calibration: Driver Setup]].