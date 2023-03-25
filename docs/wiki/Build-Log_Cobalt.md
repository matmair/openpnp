Cobalt is the name of the first OpenPnP machine. It's a work in progress and this document will chronicle it's design and build.

## April 21, 2013

Another good day for OpenPnP. The Y gantry is complete and is moving under power. I've decided to use a hunk of 80/20 25-5050 for the Y gantry, which I don't have at the moment, so for the time being I am using a piece of 2550. Gotta order more 80/20.

Today I built two new parts. They are the gantry adapter plates that couple the 80/20 gantry to the Y axis linear bearings. Easy parts. They are just 1/2" aluminum bar stock with 8 holes drilled and 4 holes countersunk.

Goals for this coming week are to finish up the design of the X axis and start building the parts for it. There's less parts in the X axis, and they are less complex, so my hope is that by the start of next weekend I'll have the X axis mostly built and ready to start working on the X to head interface. Then things get interesting!

Here's a video showing the Y gantry running a short job in OpenPnP: http://www.youtube.com/watch?v=6J69JoVtv-o

![](images/IMG_0947.png)

## April 20, 2013

Lots of progress today. Got the frame built from 80/20 and squared up very nicely. Built four 100x50mm joining plates for 80/20 on the mill, which went really quick. Used a DTI to get the first Y rail parallel to the 80/20 within a mil and then used the DTI on the first rail to do the same to the second rail. Mounted the lead screw, bearings and Y stepper back up and the base is looking pretty nice.

I figured out a way to quickly bolt a hunk of 80/20 to service as a temporary gantry so I can test the single sided drive. Tomorrow I'll cut a piece from my stock, bolt it up and test. If that does the trick I have two more plates to make to bolt the gantry to the Y axis and then I'll start on the X axis bearing blocks.

Also did quite a bit of design work today, fleshing out a lot of the next steps.

![](images/2013-04-21-00.png)
![](images/2013-04-21-01.png)

## April 19, 2013

This is the first entry in this build log, so I will detail where things stand now and then future updates will just talk about progress.

Cobalt is the name I gave to a hardware design I have been working on for quite some time. Unfortunately, the design turned out to be flawed and difficult to machine, so I have basically abandoned it. Now I am focusing on getting *any* machine up and running for Maker Faire. What I am working on now will eventually become the new Cobalt.

The old design used linear bearings and shafts and these turned out to be a lot tricker than I thought they would be. It also used a single sided drive on the Y axis. Those two things combined made for a very inaccurate machine.

For the time being, I am using some nice linear rails I got on eBay a long time ago for Y. X will still be linear bearings since the drive will be symmetric.

Last weekend I made two bearing blocks for the Y lead screw, the Y stepper mount and the Y lead screw nut coupler. These components, plus the linear rails, lead screw, lead screw nut, various bits of hardware and a TinyG resulted in a very accurate and fast half of Y axis. 

Here's a short video showing the half Y axis test: http://www.youtube.com/watch?v=LVPcsuYy4oA

This is running at 250mm per second and in a later test I measured it's repeatability to > 0.001" with a dial test indicator.

This weekend's goals are:

* Build the base frame out of 80/20.
* Get both rails that will make up Y mounted and parallel.
* Figure out a way to couple a hunk of 80/20 to the rails to make a gantry and test if single sided drive will work. If not, order a second Y screw and cut out two more baring blocks.
* Design the X axis in SketchUp and, if there is time, start cutting the parts for it.

By the end of the weekend I'd really like to have the Y axis fully built and running if possible and at least have a start on X.