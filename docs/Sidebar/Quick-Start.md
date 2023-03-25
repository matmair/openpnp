Introduction
============

This Quick Start guide will guide you through installing OpenPnP, seeing the major components of the interface and running a sample job in the OpenPnP simulator. This will allow you to quickly understand how OpenPnP works and give you a foundation to begin hooking it up to your own machine.

Installation
============

Download
--------

Visit http://openpnp.org/downloads to find out how to download the latest snapshot or release of OpenPnP.

Install and Run
---------------

If you are using one of the binary installers from the website, just run the installer and follow the instructions. After installation you can run OpenPnP from your operating system's applications list, i.e. Start Menu, Applications folder, etc.

If you are using an archive version of OpenPnP, unzip the software into a directory of your choosing. Typically this would be the same place you keep your other applications. Inside the folder you unzipped OpenPnP to there is an `openpnp.sh` and `openpnp.bat` script. These should work for Windows, Mac and Linux. For Mac and Linux, run `openpnp.sh` and for Windows run `openpnp.bat`. After a short wait you should see the OpenPnP Main Window.

A Quick Tour
============

User Interface
--------------
This image shows the major components of the user interface. We'll reference the names of these components throughout the rest of the guide, so take a moment to get familiar with them.

![screen shot 2017-04-14 at 6 15 55 pm](https://cloud.githubusercontent.com/assets/1182323/25058445/1c1ce4f8-213f-11e7-86f7-f30aea7d7425.png)

Moving Around
-------------
OpenPnP is set up out of the box so that you can use it right away; you don't even a need to connect a machine!

When you start OpenPnP for the first time you will see a simulated pick and place table in the camera view. Try following along with the items below to get a feel for how OpenPnP works:

1. Press the green power button <img src="https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/power_button_on.svg" height="18"> to start the virtual "machine".
2. Use the jog buttons ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/arrow-left.svg) ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/arrow-down.svg) ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/arrow-right.svg) ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/arrow-up.svg) ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/rotate-clockwise.svg) ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/rotate-counterclockwise.svg) in the jog controls to move the camera around. You can change the distance each click moves by changing the value of the Distance slider.
3. Visit each of the tabs along the top of the window to see how Jobs, Parts, Packages, Feeders and the Machine is configured. Right now it's best not to change anything.

About The Demo
--------------
If you'd like to get right to seeing OpenPnP in action you can skip this section and refer back to it later.

The demo configuration you see when you first start OpenPnP includes simulated cameras, feeders, nozzles, and everything else that makes an OpenPnP machine. In particular, what you see in the camera view is a small window into a much larger virtual pick and place machine. The full machine looks like this:

![](https://raw.githubusercontent.com/openpnp/openpnp/develop/src/main/resources/samples/pnp-test/pnp-test.png)

The machine includes 8 boards in different orientations, 9 strip feeders labeled Upper Strip 1 - 4, and Lower Strip 1 - 4, and a red rectangle showing the location of the simulated bottom vision camera.

Your First Job
==============

Now that you've seen the user interface a bit, it's time to try running a pick and place job. Follow along with the instructions below:

1. Select the Job tab at the top of the main OpenPnP window.
2. From the File menu, select Open Job.
3. Using your computer's file dialog, find the `samples` directory that came with OpenPnP. On Windows it's in your `Documents\OpenPnP` directory. On Mac and Unix it is in the same directory you installed OpenPnP into.
4. In the `samples` directory, find the `pnp-test` directory and open the `pnp-test.job.xml` file inside it.
5. You'll see the job has loaded and there are now boards and placements listed. You can browse the boards and placements to see what the job will be doing.
6. If you haven't already, press the green power button <img src="https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/power_button_on.svg" height="18"> to start the machine.
6. Press the green play button ![](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/control-start.svg) to start the job and the camera will start moving.

OpenPnP will now simulate a full pick and place job. It will use computer vision to align the boards using fiducials, find parts in virtual feeders, and then place the parts on virtual boards. You can follow along by watching the camera view.

When the job is complete, congratulations! You've run a job in OpenPnP! The next step is to dive into the [[User Manual]] and start learning how to hook OpenPnP up to a real machine.

What's Next
===========

Next you should start reading the [[User Manual]] to get a better feel for the more advanced features of OpenPnP, and to learn how start integrating the software with your machine.

If you don't have a machine yet, visit http://openpnp.org/hardware/ to see some options for building or buying a machine.

If you want to dive into the code, have a look at the [[Developers Guide]] to start hacking.

For help or just conversation, join [the discussion group](http://groups.google.com/group/openpnp), or come chat with us on Freenode IRC at [#openpnp](http://webchat.freenode.net/?channels=openpnp). If you don’t have an IRC client, you can use [this web based one](http://webchat.freenode.net/?channels=openpnp).