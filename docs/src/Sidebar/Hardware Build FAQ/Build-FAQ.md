This is a collection of frequently asked questions specific to designing, building, and assembling a machine. For questions more specific to the OpenPnP software or running the machine, see the main [[FAQ]].

## What Should I Build?

See http://openpnp.org/hardware/ for a number of options. 

The current recommendation for a build guideline is:

* [DIY Pick and Place](https://hackaday.io/project/9319-diy-pick-and-place) for frame and X/Y.
* [ICEpick Direct Drive Head](https://github.com/BETZtechnik/ICEpick--Direct-drive-pick-and-place-head/wiki) for the head.
* [Quick Change Juki Nozzle Holders](http://www.betztechnik.ca/store/p32/Quick_change_Juki_nozzle_holders-_NEMA_8_5mm_OD_hollow_shaft-_STOCK.html) for nozzle holders and nozzles.
* [Smoothieboard](http://smoothieware.org/getting-smoothieboard) for motion control. The [[Motion Controllers]] page also has additional information about this important component.
* [ELP Model USB100W03M, 720p USB Cameras](http://www.elpcctv.com/hd-720p-usb-cameras-c-85_87.html) which can be purchased on Amazon, AliExpress and eBay. Lenses are standard M12 mount and can be replaced to customize for your machine's geometry. See below for lens and positioning recommendations.
* Yamaha CL Feeders (available on Aliexpress and [Robotdigg](https://www.robotdigg.com/product/829/CL82-or-CL84-Feeder-4-OpenPnP)) if you want auto feeders. Make sure to get a [mounting block](https://www.robotdigg.com/product/1190/Pick-and-place-machine-Feeder-mounting-block), too. 

## What kind of PC will I need?

OpenPNP has been used on all manner of computing hardware.  OpenPNP is written in Java and makes use of OpenCV.  Hardware that runs Java and OpenCV well will generally run OpenPNP well.  on Dec 19 2019, Jason said, "an i5 w/ 8GB RAM would be a good minimum requirement."  He also added, "OpenCV performance is not critical at all. Everything else is orders of magnitude slower than OpenCV."  You can run OpenPNP on other things but this is a sensible recommendation for an easy machine to start with.

The one hardware constraint worth mentioning has to do with USB Cameras.  In order to use more than one USB camera, you need a machine with more than one USB bus.  On many machines, USB Ports will share a single USB bus.  This does not provide sufficient bandwidth for multiple simultaneous cameras to operate.  Please read the documentation on the [[OpenPnpCaptureCamera]] driver for more details.


## How Far Should Cameras Be Mounted From Objects

This is a hard to answer question since it's specific to the machine, the camera, the lens, the size of parts you want to place, etc. A good guideline is:

* Top Camera: 8mm lens, 100mm distance from board.
* Bottom Camera: 3.2mm or 3.6mm lens, 30-40mm distance from nozzle tip.

M12 lenses are cheap, so we recommend picking up a couple sizes to experiment with. 

The [Ikosi Vision Pick and Place Lens Calculator](http://www.ikosivision.com/index.php/pick-and-place-lens-calculator/) can help you choose appropriate lens sizes for your machine setup, and has some great tips on choosing a lens.

The [Vision Doctor optics basics page](https://www.vision-doctor.com/en/optical-basics.html) and [lens calculators](https://www.vision-doctor.com/en/optical-calculations.html) can help with this decision.

## Mounting Vacuum Hose to NEMA-8 Motors

Most home built systems use NEMA-8 stepper motors for nozzle rotation. To get vacuum to the nozzle on the motor shaft, there are three common options:

1. Slide the vacuum hose over the back shaft directly: This works fine, and is how many Chinese machines work. The downside to it is that you do not get 360 degree rotation and it puts a little extra load on the motor shaft. Since these motors don't have much torque to begin with, this may cause extra current and heating. But again, it works.

2. Use a rotary fitting coupler: Places like RobotDigg sell a rotary fitting that will mount to the back of the shaft and the hose. The coupler is airtight and puts very little strain on the motor.

3. Use a "cap": The OpenBuilds reference design includes a [3D printed cap](https://github.com/openpnp/openpnp-openbuilds/blob/1.0/Mechanical/3D%20Printed/STL/OpenPnP-OpenBuilds-C-Mount.stl) for the back of the motor. The cap accepts the vacuum hose and using an o-ring and a little silicone grease creates a very low friction, airtight fitting.

    A common question for the cap style is whether the bearings of the motors will leak vacuum, and the answer is: No, or not enough to matter. Additionally, you don't have to do anything fancy to the 3D print to make it airtight. Just print and go.