# Scripting

OpenPnP has a built in scripting engine that allows you to add new functionality to the program without having to modify the source code.

Scripts get full access to the OpenPnP program and can read and modify any data in it. You can use scripts to do things like move the machine, reset feeders, print out board information, etc.

OpenPnP comes with several example scripts that are automatically installed. These can be used as starting points for your own scripts.

## Usage
Scripts placed in the `.openpnp/scripts` directory will be automatically added to the Scripts top level menu. Directories are respected and will be added as sub-menus so that you can organized your scripts as you see fit. OpenPnP will recognize scripts by their file extensions based on the scripting engines that are installed. By default scripts with the extension `.js` are loaded.

To run a script simply click it's name from the menu.

Scripts can also be run as part of the machine's motion by using a special type of actuator called a **ScriptActuator**. For more detail on this feature see here [ScriptActuators](https://github.com/openpnp/openpnp/wiki/Script-Actuators)

The Scripts menu also includes a Refresh Scripts item that will manually update the menu and a Open Scripts Directory item that will open the scripts directory using your computer's file manager.

## Ignoring Directories
If you place an empty file called `.ignore` in any subdirectory under the `scripts` directory then that directory will not be shown in the menu.

## Language Support
OpenPnP supports scripts written in JavaScript and Python out of the box, and it's easy to add support for other languages. In most cases to add a language it's just a matter of finding a JAR file that implements that language and adding it to your Java classpath. See below for specifics on languages.

### JavaScript
JavaScript support is included with OpenPnP, there is nothing you need to add. The engine is called Nashorn. Script files with the extension `.js` will be run by the Nashorn / JavaScript engine.

For more information about the integration between Java and JavaScript in scripts, see the following links. They are listed in suggested reading order:

- http://winterbe.com/posts/2014/04/05/java8-nashorn-tutorial/
- https://wiki.openjdk.java.net/display/Nashorn/Nashorn+extensions
- https://docs.oracle.com/javase/8/docs/technotes/guides/scripting/nashorn/toc.html
- http://www.oracle.com/technetwork/articles/java/jf14-nashorn-2126515.html

### Python
Python support is included with OpenPnP, there is nothing you need to add. The engine is called Jython. Script files with the extension `.py` will be run by the Jython engine.

More information about Jython can be found at http://www.jython.org/currentdocs.html and http://www.jython.org/jythonbook/en/1.0/JythonAndJavaIntegration.html.

### Beanshell
Beanshell support is included with OpenPnP. Script files with the extension '.bsh' are executed using Beanshell.

More information about it can be found at http://www.beanshell.org/manual/syntax.html#Basic_Syntax

## Global Variables
OpenPnP exposes several global variables to the scripting environment for use in your scripts. They are:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| config  | [org.openpnp.model.Configuration](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Configuration.html) | The current OpenPnP Configuration object. Provides access to Parts, Packages, Resources, etc. |
| machine | [org.openpnp.spi.Machine](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Machine.html) | The machine object declared in the configuration. Provides access to Nozzles, Cameras, Feeders, etc. This can be used to move the machine and perform machine operations. Note that the type of this object is dependent on how your machine is configured but it will typically be ReferenceMachine. |
| gui | [org.openpnp.gui.MainFrame](http://openpnp.github.io/openpnp/develop/org/openpnp/gui/MainFrame.html) | The top level window in the OpenPnP GUI. From here you can access any part of the GUI |
| scripting | [org.openpnp.scripting.Scripting](http://openpnp.github.io/openpnp/develop/org/openpnp/scripting/Scripting.html) | The OpenPnP scripting engine. Can be used to find information about scripts and the scripting environment. |

## Scripting Events

Scripting Events allow you to define scripts that will be run automatically by OpenPnP during certain events. These scripts should be placed in the `.openpnp/scripts/Events` directory. They should be named in accordance with the events described below (e.g. `Job.Placement.Complete.py`). Some Scripting Events include additional global variables. These are described below with each event.

## Scripting Event List

* [Camera.BeforeSettle](#CameraBeforeSettle): Called before the camera settling, preceding a capture.
* [Camera.BeforeCapture](#CameraBeforeCapture): Called before an image capture.
* [Camera.AfterCapture](#CameraAfterCapture): Called after an image capture.
* [Camera.AfterSettle](#CameraAfterSettle): Called after the camera settle time.
* [Camera.AfterPosition](#CameraAfterPosition): Called after moving the camera using the Position Camera icon.
* [Job.AfterDiscard](#JobAfterDiscard): Called after a part has been discarded and the nozzle has returned to safe z.
* [Job.BeforeDiscard](#JobBeforeDiscard): Called during a discard sequence before the nozzle has moved to the discard location.
* [Job.Finished](#JobFinished): Called when a job completes.
* [Job.Placement.BeforeAssembly](#JobPlacementBeforeAssembly): Called before the process of handling a placement starts.
* [Job.Placement.Complete](#JobPlacementComplete): Called after a placement is complete.
* [Job.Starting](#JobStarting): Called before the job starts.
* [Machine.AfterHoming](#MachineAfterHoming): Called after the machine is homed.
* [Nozzle.AfterPick](#NozzleAfterPick): Called after a nozzle has picked a part.
* [Nozzle.BeforePick](#NozzleBeforePick): Called before a nozzle picks a part.
* [Nozzle.AfterPlace](#NozzleAfterPlace): Called after a nozzle places a part.
* [Nozzle.BeforePlace](#NozzleBeforePlace): Called before a nozzle places a part.
* [NozzleCalibration.Starting](#NozzleCalibrationStarting): Called before nozzle calibration begins.
* [NozzleTip.BeforeLoad](#NozzleTipBeforeLoad): Called before a nozzle tip is loaded in a nozzle.
* [NozzleTip.Loaded](#NozzleTipLoaded): Called after a nozzle tip is loaded in a nozzle.
* [NozzleTip.BeforeUnload](#NozzleTipBeforeUnload): Called before a nozzle tip is unloaded from a nozzle.
* [NozzleTip.Unloaded](#NozzleTipUnloaded): Called after a nozzle tip is unloaded from a nozzle.
* [Startup](#Startup): Called after OpenPnP starts.
* [Vision.PartAlignment.After](#VisionPartAlignmentAfter): Called after part alignment / bottom vision is performed on a part.
* [Vision.PartAlignment.Before](#VisionPartAlignmentBefore): Called before part alignment / bottom vision is performed on a part.

## Scripting Event Details

### Startup

Called when system startup is complete.

Variables: None.

### Job.AfterDiscard

Called after a part has been discarded and the nozzle has returned to safe z.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle that held the discarded the part. |

### Job.BeforeDiscard

Called during a discard sequence before the nozzle has moved to the discard location.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle that holds the part to be discarded. |

### Job.Starting

Called when a job is starting up. The event is fired after the pre-checks for the job have completed, and the machine is ready to start processing the job.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| job  | [org.openpnp.model.Job](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Job.html) | The Job that is starting. |
| jobProcessor  | [org.openpnp.spi.JobProcessor](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/JobProcessor.html) | The JobProcessor responsible for running the Job. |

### Job.Finished

Called when a job has completed. The machine has parked (if configured) and all processing is finished.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| job  | [org.openpnp.model.Job](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Job.html) | The Job that has just finished. |
| jobProcessor  | [org.openpnp.spi.JobProcessor](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/JobProcessor.html) | The JobProcessor responsible for running the Job. |

### Job.Placement.Complete

Called after a placement has been completed, i.e. a part has been placed.

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| job  | [org.openpnp.model.Job](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Job.html) | The Job that has just finished. |
| jobProcessor  | [org.openpnp.spi.JobProcessor](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/JobProcessor.html) | The JobProcessor responsible for running the Job. |
| part  | [org.openpnp.model.Part](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Part.html) | The Part that has been placed. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle that placed the part. |
| placement  | [org.openpnp.model.Placement](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Placement.html) | The Placement that was completed. |
| placementLocation  | [org.openpnp.model.Location](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Location.html) | The Location where the part was placed. This includes offset corrections. |
| boardLocation  | [org.openpnp.model.BoardLocation](http://openpnp.github.io/openpnp/develop/org/openpnp/model/BoardLocation.html) | The BoardLocation that the given Placement was located in. |

Example for saving images of placed parts (for quality assurance):  
.openpnp/scripts/events/Job.Placement.Complete.js
```js
// Get a reference to the camera.
var camera = machine.defaultHead.defaultCamera;

// Move the camera to where the part was placed.
camera.moveTo(placementLocation);

// Settle the camera and capture an image.
var image = camera.settleAndCapture();

var t = new Date(); // get current time
var timeStr = t.toISOString(); // e.g. 2011-12-19T15:28:46.493Z

timeStr = timeStr.replace('T','_');
timeStr = timeStr.replace(':','-');
timeStr = timeStr.replace(':','-');
timeStr = timeStr.slice(0,-5); // remove milli seconds
// timeStr is now: 2011-12-19_15-28-46

var fileName = String('/home/user/.openpnp/vision/log/placements/' + timeStr + '_' + placement.getId() + '.png')

print('save placement image to ' + fileName);
// Write the image to a file based on the placement name.
var file = new java.io.File(fileName);
javax.imageio.ImageIO.write(image, "PNG", file);
```

### Camera.BeforeSettle

Called concurrently with the start of the settle timer before an image is captured from a camera. This is intended to be used to control lighting, mirrors, strobes, etc. Using Camera.BeforeSettle instead of Camera.BeforeCapture gives the lighting time to actually turn on and gives the camera more time to adjust to a new lighting condition.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which will be used to capture an image. |

Example:

.scripts/events/Camera.BeforeSettle.js
```js
/**
 * Controls lighting for the two cameras using two named actuators. The lights
 * for the up camera and down camera are turned on and off based on which camera
 * needs to capture.
 */
 var upCamLights = machine.getActuatorByName("UpCamLights");
 var downCamLights = machine.getActuatorByName("DownCamLights");

if (camera.looking == Packages.org.openpnp.spi.Camera.Looking.Up) {
	upCamLights.actuate(true);
	downCamLights.actuate(false);
}
else if (camera.looking == Packages.org.openpnp.spi.Camera.Looking.Down) {
	upCamLights.actuate(false);
	downCamLights.actuate(true);
}
```

### Camera.BeforeCapture

Called before an image is captured from a Camera. It is not guaranteed to be run before the captured frame is physically taken by the camera (due to buffering). In other words, if used for lighting, this may turn on the lights too late.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which will be used to capture an image. |


### Camera.AfterCapture

Called after an image is captured from a Camera. It is not recommended to be used for lighting.
Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which will be used to capture an image. |


### Camera.AfterSettle

Called after the camera is settled, and an image is captured. This is intended to be used to control lighting, mirrors, strobes, etc. Using Camera.AfterSettle instead of Camera.AfterCapture is the only correct way to control lighting, if you use Auto Settling. 

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which will be used to capture an image. |

Example:

.scripts/events/Camera.AfterSettle.js
```js
/**
 * Controls lighting for the two cameras using two named actuators. All
 * of the lights are turned off.
 */
 var upCamLights = machine.getActuatorByName("UpCamLights");
 var downCamLights = machine.getActuatorByName("DownCamLights");
upCamLights.actuate(false);
downCamLights.actuate(false);
```

### Camera.AfterPosition

Called after the camera is moved to a position using the ![Position Camera](https://rawgit.com/openpnp/openpnp/develop/src/main/resources/icons/position-camera.svg) icon. This is intended to be used to turn on lighting and/or adjust camera settings so the object the camera moves to can be seen.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which was moved. |

Example:

.scripts/events/Camera.AfterPosition.js
```js
/**
 * Turn on Down Camera Lights
 */
 var downCamLights = machine.getActuatorByName("DownCamLights");
downCamLights.actuate(true);
```

### Vision.PartAlignment.Before

Called before part alignment (bottom vision) takes place.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| part  | [org.openpnp.model.Part](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Part.html) | The Part being aligned. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The The Nozzle that the part is on. |

### Vision.PartAlignment.After

Called after part alignment (bottom vision) has completed.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| part  | [org.openpnp.model.Part](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Part.html) | The Part being aligned. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The The Nozzle that the part is on. |

### NozzleCalibration.Starting

Called before nozzle is calibrated. The other camera-events (beforeSette, .beforeCapture and .afterCapture) are fired while processing, too.

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle that is being calibrated. |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which will be used to capture an image. |

Example for adapting the camera exposure:  
.openpnp/scripts/events/NozzleCalibration.Starting.bsh
```js
//
//This is the script to change the value of the Bottom Camera on demand.
//I use it to increase the "sensitivity" of the camera while the nozzle calibration
//process then decrease it when calibration is done.
//Actuators in last section can be used to control the lightning for the calibration time.
//
 
import org.pmw.tinylog.Logger;
camExposure = org.openpnp.util.VisionUtils.getBottomVisionCamera().getExposure();
value = 4; // put here the value you want to set. Expos values are negative but use positive here.

prevExpValue = camExposure.getValue();
camExposure.setValue(-value);
//newExpValue = camExposure.getValue(); //commented to dont rummage in the camera settings memory more than need

//gui.setStatus(("BottomVision Exposure changed from: "+ prevExpValue) + (" to: " + newExpValue));
gui.setStatus(("BottomVision Exposure changed from: "+ prevExpValue) + (" to: -" + value));
//Logger.debug(("BottomVision Exposure changed from: "+ prevExpValue) + (" to: " + newExpValue));
Logger.debug(("BottomVision Exposure changed from: "+ prevExpValue) + (" to: -" + value));

actuator = machine.getActuatorByName("DownCamLights");	
actuator.actuate(false); //to turn downlooking lightning OFF
//actuator.actuate(true); //to turn downlooking lightning ON
```


### NozzleCalibration.Finished

Called after nozzle calibration finished.

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle that was calibrated. |
| camera  | [org.openpnp.spi.Camera](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Camera.html) | The Camera which will be used to capture an image. |

### NozzleTip.BeforeLoad

Called before a new NozzleTip is loaded.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| head  | [org.openpnp.spi.Head](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Head.html) | The Head the NozzleTip belongs to. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle the new NozzleTip was loaded in. |
| nozzleTip  | [org.openpnp.spi.NozzleTip](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/NozzleTip.html) | The NozzleTip that will be loaded. |

### NozzleTip.Loaded

Called after a new NozzleTip has been loaded.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| head  | [org.openpnp.spi.Head](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Head.html) | The Head the NozzleTip belongs to. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle the new NozzleTip was loaded in. |

### NozzleTip.BeforeUnload

Called before a NozzleTip is unloaded.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| head  | [org.openpnp.spi.Head](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Head.html) | The Head the NozzleTip belongs to. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle the new NozzleTip was loaded in. |
| nozzleTip  | [org.openpnp.spi.NozzleTip](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/NozzleTip.html) | The NozzleTip that will be unloaded. |

### NozzleTip.Unloaded

Called after a NozzleTip has been unloaded.

Variables:

| Name  | Type | Description |
| ------------- | ------------- | -------------- |
| head  | [org.openpnp.spi.Head](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Head.html) | The Head the NozzleTip belongs to. |
| nozzle  | [org.openpnp.spi.Nozzle](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/Nozzle.html) | The Nozzle the NozzleTip was unloaded from. |

## Examples Running System Commands from JavaScript

#### Call `firefox` from JavaScript:

```js
Runtime = Java.type("java.lang.Runtime").getRuntime();
Runtime.exec("firefox");
```

#### Print a system command (e.g. "ls -l") response:

```js
var imports = new JavaImporter(java.io, java.lang);
with (imports) {
  var p = Runtime.getRuntime().exec("ls -l");
  var stdInput = new BufferedReader(new InputStreamReader(p.getInputStream()));

  while ((s = stdInput.readLine()) != null)
    System.out.print(s);
}
```
