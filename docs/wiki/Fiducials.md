## About Fiducials

Fiducials, or fiduciary marks, are small identifiers designers place on PCBs specifically to help pick and place machines and other processes to visually align the board to a known reference point. A typical PCB that is designed for manufacture will have at least 3 fiducials and may have many more.

Fiducials are typically created by leaving a blank spot of copper on the PCB surrounded by a bare area called a keepout. This creates a bright, shiny mark on the PCB that is easy for computer vision algorithms to identify. The most common mark is a 1mm circle surrounded by a 2mm keepout.

## Fiducials in OpenPnP

OpenPnP can use 2 or 3 fiducials to detect the position of a PCB on the bed of the machine. This allows for automated, accurate, board locating either during job setup or during job run.

When 2 fiducials are detected OpenPnP can determine the position and rotation of the PCB. If 3 or more are detected, the best 3 will be used to determine the position, rotation, scale, and shear of the PCB. Using 3 or more will generally produce better results than just 2.

A fiducial in OpenPnP is defined by a package with a footprint that specifies what the fiducial looks like. For instance, a 1mm round fiducial is simply a footprint containing a 1x1mm pad with 100% roundness.

## Creating a Fiducial Package

You only need to perform this process once per type of fiducial you use.

1. Create or select an existing Package from the Packages tab.
2. On the right of the window you should see a Footprint tab. Select this and you will see the Footprint editor.
3. Set the Body Width and Body Length to 0, and set the Units to the units of your Fiducial.
4. Click the Add button ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/general-add.svg) to add a new Pad. A Fiducial will typically have just one pad.
5. Set the name of the Pad to anything you like. I typically just use "1".
6. Set the X and Y position of the Pad to 0, and set the Width and Length to the diameter of the Fiducial. If the Fidicual is round set the Roundness to 100%. For example, a 1mm round fiducial would be defined as X = 0, Y = 0, Width = 1, Length = 1, Roundness = 100%.
7. Go to the Parts tab and create a new Part to represent your fiducial. This is the part you will assign to placements to represent fiducials on boards. Set it's package to the fiducial package you created.

## Using Fiducials in Boards

Once you have defined your fiducial part, package and footprint you are ready to use it. You can assign that part to any placement in your board and set the placement type to Fiducial. This will tell OpenPnP to consider this placement for any fiducial operation it needs. You should set at least two placements as Fiducials.

For additional information on using your fiducials, please watch this video tutorial showing how to use fiducials: https://www.youtube.com/watch?v=xvmdvTroZj8

## Troubleshooting

If OpenPnP is not finding your fiducials, try the following:

1. Make sure you have set the Units Per Pixel values for your camera. A quick way to test this is to right click the camera view, turn on the Ruler Reticle and see if it matches a ruler placed on the machine.
2. Use solder wick to put a thin coating of solder on the fiducial. This will give it a matte finish that can more easily be seen by the machine vision, compared to the reflective finish that is sometimes present on pads straight from manufacture.
3. On HASL boards, try roughing up the fiducial with an eraser to remove the gloss.
4. Make sure that the fiducial is within a certain distance from where it is expected to be. Go to Machine Setup / Vision / Fiducial Locator and adjust **Max. Distance** if needed. This prevents from locking to something in the camera view which is recognized as fiducial, but really isn't.
5. Don't set a **body** width and length for fiducials, leave them 0.  You only need the pad definition.
6. Change the log level to DEBUG or TRACE. See https://github.com/openpnp/openpnp/wiki/FAQ#how-do-i-turn-on-debug-logging for more information.
7. Restart OpenPnP and try to run your fidicual check again.
8. After you run it there will be a new directory in your `.openpnp` [directory](https://github.com/openpnp/openpnp/wiki/FAQ#where-are-configuration-and-log-files-located) called `openpnp/org.openpnp.vision.pipeline.stages.ImageWriteDebug` and under that directory will be some images. The images show the process of trying to find the fiducials.

  If you understand computer vision a bit, take a look at the images and see if you can find any problems. Common problems are lighting and template size related. If it doesn't help, please post your images to [the OpenPnP mailing list](http://groups.google.com/group/openpnp) and someone will try to help.

