## What is it?

In short, the ReferencePushPullFeeder can use both electronic and/or elaborate 5-point mechanical motion to advance the tape. Many feeder designs requiring electronic or mechanical actuation (automatic, lever, knob, push, drag, etc. pp.) are supported. The actuation can be independent of the pick motion and aside from electronical actuation, it can perform a "hook and pull" articulation (levers, drag feeding, etc.), therefore it supports 2mm pitch tapes (e.g. with 0402/0201 parts) where the tape is only advanced on every second feed. The feeder features OCR/QR-Code part label recognition, optimized vision calibration, elaborate auto-learning, one-click auto-setup, OCR/QR-Code based feeders-in-a-row discovery and more.

![Example Feeder in Action](https://user-images.githubusercontent.com/9963310/94424926-54387480-018b-11eb-96b1-963e69c14e50.gif)

While this feeder software solution was developed side-by-side with the all-3D-printed feeder [featured here](https://makr.zone/new-all-3d-printed-tapereel-feeder/399/), it was an important design goal to create the software side as universal as possible, to be used for all kinds of electronic and mechanical feeders. Your design can be completely different!  

## Features in more Detail

* Generic implementation for a feeder that can feed parts by performing electronic and/or push and pull (and other) motions using a head-mounted Actuator.
* Uses a semantic vision concept based on sprocket holes and knowledge of the EIA 481 standards.
* Employs a simple OCR/QR-Code vision stage that can identify the part from a label on the feeder.
* OCR/QR-Code based automatic feeder slot swapping, part changing or even new feeder creation with auto-cloned settings.
* Automatic and learning "only when needed" calibration for feeders that are mounted with large tolerances (+/-2mm).
* Any part pitch, any feed pitch, any tape width.
* 2mm pitch (0402/0201) support i.e. the feeder will only advance the tape on every second feed. 
* Multiplier: For speed, you can use a Multi-feed for any pitch, i.e. a feeder can quickly actuate multiple times, then only pick on the next feeds.
* Easy to clone & sync settings between feeders, by package or by (new) Tape&Reel Specification.
* One click setup from the second feeder.
* Handles geometric transformations for all the relevant settings (allows cloning settings from a “west” to a “south” feeder, for instance).

## Video

There is a video documenting the most important setup steps. You can watch this to get going and get a first impression. However, some steps aren't covered so you should come back to this page to fill in the missing details. Also note that the 3D-printed feeder featured in the video is just one example standing in for a wide variety of possible feeder hardware designs, including purely electronic ones. Any feeder requiring any type of electronic and/or mechanical motion actuation (lever, knob, push, drag, etc. pp.) is supported. 

https://youtu.be/5QcJ2ziIJ14

## Reuse of Settings

The settings described in the following sections are numerous and it may sound like hard work to setup a single feeder. However most of these settings are fine to be left on the defaults, and the few exceptions must only be visited once, for the first feeder of a certain tape specification. The settings will subsequently be automatically cloned to the next feeder. See [[Clone Setting|ReferencePushPullFeeder#clone-settings]]. 

Subsequent feeders may be setup with as little as one click!

## General Settings 

![General Settings](https://user-images.githubusercontent.com/9963310/94363615-af9f2f80-00c3-11eb-9688-5385838f3a6a.png)

The General Settings are as for every other feeder. However, because we will be using OCR to detect the right part, we just leave it alone now. 

## Locations

![Locations](https://user-images.githubusercontent.com/9963310/94363743-c09c7080-00c4-11eb-96ff-474db04ceb34.png)

The Locations define the orientation and location of the feeder _or rather of the tape_ in the feeder. Because it is much easier for computer vision to precisely see the sprocket holes than the parts in the tape's pockets, the sprocket holes will be used for precise tape location calibration. Given tape width and pitch settings, the ReferencePushPullFeeder can then use EIA 481 standardized tape geometry to deduce precise pick locations from the sprocket holes. 

Normally, you let computer vision get the locations automatically for you (see section [Locations Auto-Setup](#locations-auto-setup) below). When this is not possible e.g. for transparent tapes or when the camera can't reach the feeder, you can set the locations up using the standard capture buttons. Read the tooltips on the field labels to learn which location is which. 

For computer vision to work reliably, it is recommended to have a contrasting background behind the tape's sprocket holes. For best robustness use a vivid (i.e. high saturation) color, like the "green screen" that is used in movie special effects. The color-screening method works across a large range of brightnesses, even in difficult lighting/shadow situations. The ReferencePushPullFeeder comes with a default vision pipeline that should work out of the box for a green-yello-orange-red range of screening colors. However, the vision process is very flexible and also works with any custom sprocket hole detection method and many pipeline result types. So if your machine has a monochrome camera or non-color background, don't give up yet. In this case you need to edit the vision pipeline, see the [[Vision|ReferencePushPullFeeder#vision--calibration]] section below. 

The following assumes you have a working pipeline. You can press the **Preview Vision Features** button to check the pipeline first.  

![](https://user-images.githubusercontent.com/9963310/94364578-a36aa080-00ca-11eb-9ce5-a0160b7462c4.png)

## Locations Auto-Setup

**Important Note**: Auto-Setup is really only a first-step tool. Do not use it once the feeder is already set up, hand-tuned settings may be overwritten! To avoid mishaps, a warning message box will ask you to confirm.
___

For a very quick setup, just move your camera center roughly over the pick location and press the Auto-Setup button. If this is a 2mm pitch tape, choose the pick location closest to the tape reel, i.e. closest to where it is feeding from.

![Auto-Setup](https://user-images.githubusercontent.com/9963310/94364461-aadd7a00-00c9-11eb-908c-ae259b719d84.png)

The Auto-Setup will set up all your locations automatically, using "smart" computer vision, taking the camera location i.e. the future pick location into consideration, while applying EIA-481 industry standard tape geometry constraints. It will try the different types of computer vison until it finds the sprocket holes (see the **Vision Type** setting [futher below](#vision--edit-pipeline)). If you already have other ReferencePushPullFeeders defined, this will also clone some setting over from them (more about that later). 


The **Normalize?** option will normalize the pick location(s) to nominal coordinates relative to the vision calibrated sprocket holes according to the EIA 481 standard. This means it does not matter how precisely you capture the Pick Location i.e. the center of the tape pocket in Auto-Setup (or manually). Your manual capture need only be within ±1mm of the true pick location. If you want to override the standard, switch **Normalize?** off.

The **Snap to Axis?** option will align the vision calibrated sprocket hole locations to the nearest axis, i.e. either to X or Y (within ±10°). This means you are trusting the mechanical squareness of your machine, feeder mount and tape guide more than any relative rotation of the sprocket holes obtained from computer vision.

## Tape Settings

![Tape Settings](https://user-images.githubusercontent.com/9963310/94364897-00ffec80-00cd-11eb-8d18-91e98d523656.png)

In the Tape Settings you can set the right **Part Pitch**. 

The **Feed Pitch** is the mechanical tape transport per feeder actuation. If the Part Pitch is larger than that, OpenPnP will automatically actuate the feeder multiple times. If the Part Pitch is less than that, OpenPnP will automatically skip actuation and instead iterate between the pick locations. 

The **Rotation in Tape** setting must be interpreted relative to the tape's orientation, regardless of how the feeder/tape is oriented on the machine. 

Proceed as follows:

1. Look at the **neutral** upright orientation of the part package/footprint as drawn inside your E-CAD **library**.
1. ⚠ Double-check you are in the **library**, do **not** look at the part in the project PCB, this is **not neutral!**  
1. Note how pin 1, polarity, cathode etc. are oriented. 
   This is your 0° for the part. 

   ![library part](https://user-images.githubusercontent.com/9963310/173001959-d0b8e036-c73d-4e39-99ec-589f6b16d32c.png)

1. Look at the tape so that the sprocket holes are on top. 
   This is your 0° tape orientation (EIA-481 industry standard). 
1. Determine how the part is rotated inside the tape pocket, relative from its upright orientation in (1). Positive rotation goes counter-clockwise, negative clockwise.
   This is the **Rotation in Tape**.

   ![Rotation in Tape](https://user-images.githubusercontent.com/9963310/173055769-d776d177-b013-498e-8371-d631e43f1bb4.png)

1. Our example has a 90° clockwise rotation from its upright orientation in (1), so the correct **Rotation in Tape** is -90°. 
 
The **Multiplier** allows you to actuate the feeder multiple times to feed more parts per serving, as a speed optimization. This may reduce the feed time per part because the actuator is already at the right place and/or engaged in the mechanics. 

The **Discard Parts** function voids any remaining parts that were produced by prior feed actuations. This can be the case in 2mm pitch tapes (0201 or 0402 parts), where one feed produces two parts, and/or if you used a **Multiplier** greater than one. These parts might have fallen out of the tape in the meantime, or you don't want them picked for some reason. Discarding will just disregard these parts and prompt a new feed actuation on the next feed. If parts are still physically in their pockets, they will be transported away with the spent tape. 

## Vision

### Vision / Calibration

![Vision Calibration](https://user-images.githubusercontent.com/9963310/94429454-97e2ac80-0192-11eb-9121-1634f343fb44.png)

The **Calibration Trigger** determines when the feeder location is calibrated using computer vision. 

![Calibration Trigger](https://user-images.githubusercontent.com/9963310/94426384-c90cae00-018d-11eb-9052-9e5d74b60cc7.png)

* **None**: No calibration takes place. Use this setting for feeders/tapes, where sprocket hole recognition is impossible e.g. with a transparent tape or when the feeder is outside the camera reach.
* **OnFirstUse**: Calibration is performed on first use once after machine homing. You can press **Reset Statistics** to invalidate the current calibration. 
* **OnEachTapeFeed**: Calibration is performed on first use and after each tape feed. Use this setting when the tape transport is somewhat imprecise or if the parts are very small. 
* **UntilConfident**: Starts out like **OnEachTapeFeed**, but based on statistics over the accuracy of the tape transport, this will automatically skip further calibrations, once statistical confidence is reached. You can press **Reset Statistics** to invalidate the current calibration and recorded statistics. 

Calibration will also be invalidated whenever you change crucial feeder settings, such as Locations or when you press **Reset Feed Count**.

The **Precision wanted** is the tolerance in pick location that can be tolerated for this feeder/part. Only used with the **UntilConfident** setting. This should be no larger than about 1/4 of your part's contact size or pitch. 

The **Precision Average**, **Precision Confidence Limit** and **Calibration Count** are the statistical indicators showing the precision of your feeder's tape transport, i.e. the repeatability of the pick locations deduced from the sprocket holes after tape transport. You can press **Reset Statistics** to start a fresh statistics e.g. after re-mounting a feeder or after changing feeder settings that might impact precision. 

### Vision / OCR

![grafik](https://user-images.githubusercontent.com/9963310/94429541-bfd21000-0192-11eb-9f29-2770279c87c6.png)

The **OCR Wrong Part Action** determines what should happen if the OCR detects a different part than set in the General Settings: 

![grafik](https://user-images.githubusercontent.com/9963310/94429376-80a3bf00-0192-11eb-9df1-4ae0362a9aab.png)

* **None**: Use this setting if you don't want to use OCR. 
* **SwapFeeders**: If a wrong part is detected but the right part is selected in a different ReferencePushPullFeeder, the locations of the two feeders are swapped. The swapped-in feeder will be enabled. This will happen, if you unload/reload/rearrange your feeders on the machine.
* **SwapOrCreate**: Works like **SwapFeeders**, but if no other feeder with the right part is found, a new one will be created and swapped-in at the current feeder's location. The current feeder is then disabled in turn (they are now sitting at the same location and only one must be enabled). The new feeder will have its settings cloned from a template (see [[Clone Setting|ReferencePushPullFeeder#clone-settings]]).
* **ChangePart**: The part in the current feeder is changed. This will only work correctly, if the tape settings etc. remain the same between the parts i.e. if you restrict any reloading/rearranging to groups of feeders with the same settings. 
* **ChangePartAndClone**: The part in the current feeder is changed but settings are cloned from a template feeder (see [[Clone Setting|ReferencePushPullFeeder#clone-settings]]). 

The **Stop after wrong part?** option will stop any process (e.g. pause the Job) after a wrong part was detected and the chosen action performed. The user can then review the changes before resuming. 

The **Check on Job Start?** option will perform calibration and OCR detection on all enabled ReferencePushPullFeeders, before the Job is started so any changed parts can be resolved up front. A Travelling Salesman optimization will be used to visit all the feeders along an optimal path. 

The **OCR Font Name** and **OCR Font Size [pt]** settings determine the font used to print the label on the feeder. Make sure to have the font installed on the machine that runs OpenPnP. NOTE: the currently selected font will appear in the selection box regardless of wheter it is installed on the system or not (it will then appear at the top). If in doubt, use a different application to make sure. Users have reported that under Windows fonts need to be installed **system-wide** rather than just for the logged-in user in order to work in Java/OpenPnP. 

The **Setup OCR Region** button can be used to define the area of the Camera View that is taken for OCR. The instructions displayed underneath the camera view will guide you through the steps. You will be asked to define the region by clicking on the corners of a rectangle. If the label is rotated, make sure to select the corners in the sense of the text orientation. Also make sure to allow for enough space around the characters, i.e. the whole character box must be within the region for a character to be successfully detected. OCR happens together with location calibration, so the extracted area might yet be inaccurate, again add a generous margin when defining the region.  

![OCR Region](https://user-images.githubusercontent.com/9963310/94434266-c912ab00-0199-11eb-8947-dcb5cd0f71d0.png)

Use the **Part by OCR** button to perform the OCR action manually. 

Use the **All Feeder OCR** button to perform the OCR action on all the feeders manually. Again a Travelling Salesman optimization will be used to visit all the feeders along an optimal path. 

**NOTE**: the feeder will automatically and dynamically generate the minimal **OCR alphabet** from all the part IDs definied in OpenPnP. Characters not occuring anywhere in part IDs will not be recognized. Therefore you must make sure that parts are imported from the E-CAD (or manually defined) before testing OCR. 

### Vision / Edit Pipeline

Press the **Edit Pipeline** button to edit the pipeline, if needed. Pipeline editing should **not** normally be necessary. If your feeder does not work without editing the pipeline, please report it on the [OpenPnP discussion list](http://groups.google.com/group/openpnp). 

Select a **Vision Type** and press **Reset Pipeline** to assign one of the two stock pipelines. Usually, **Auto-Setup** should have automatically selected the right **Vision Type** for you.

- **ColorKeyed**: works when the floor under your sprocket holes has a vivid color (typically for 3D printed feeders). Color-keying is a very robust way to detect the holes, as it does not care much about varying brightnesses, shadows etc. However, it does not work for transparent plastic tapes, obviously.
- **CircularSymmetry**: works by detecting the all-around circular shape of the sprocket holes. They must be mostly plain within and around. Use this type for feeders that are not colored, or for transparent plastic tape. This may be slightly less robust than the color-keyed type.

![Edit, Vision Type, Reset](https://user-images.githubusercontent.com/9963310/180604557-1aa6e94d-1b90-4562-85e5-a15435210061.png)


**NOTE**: when editing the Pipeline, be aware that many stage properties are controlled by the feeder i.e. the ones visible in the stages will be ineffective. The AffineWarp is fully controlled by the OCR region set up in the feeder. Likewise the OCR Font etc. are controlled by the corresponding feeder settings, the OCR Alphabet is generated dynamically. 

![Editing Pipeline](https://user-images.githubusercontent.com/9963310/79022084-7870ac80-7b7d-11ea-9d7e-83efb3004551.png)

## Clone Settings

![Clone Settings](https://user-images.githubusercontent.com/9963310/94479451-1bbd8880-01d5-11eb-9f1a-a2f5488e72ea.png)

Setting up a ReferencePushPullFeeder may involve many settings and it is clear that we want to re-use these settings comfortably. We want OpenPnP to clone settings from one feeder definition to the next, either automatically, when creating a new feeder, or when we specifically want to revise the settings of a whole group of feeders. 

Not all feeders/tapes are the same, therefore OpenPnP need to know how to treat them as a group with common settings. By default the group is defined by the **packages** of the parts that are selected in the feeders. If multiple packages can share the same feeder settings because their tapes are equal in terms of pitch etc. you can set a **Tape Specification** on the Packages Tab:

![Tape Specification](https://user-images.githubusercontent.com/9963310/94481324-d3539a00-01d7-11eb-9d5a-b83d28ddf577.png)

This is just a textual tag, grouping packages with the same Tape Specification together. The example above just uses a common way to describe the relevant properties of the tapes, like for example `width x pitch`. The pattern is completely up to the user, it should include all properties relevant for the feeder settings, usually for the feed geometry and in special cases for vision (e.g. a different tag for transparent tapes). 

Once we have defined the grouping, we must designate one feeder in each group as the template, where all the settings are managed. This is done using the **Use this one as Template?** option. 

The **Template** info will show you which feeder clones to which. Either actively...

![grafik](https://user-images.githubusercontent.com/9963310/94482242-44478180-01d9-11eb-9c87-420df8e3faa2.png)

... or passively...

![grafik](https://user-images.githubusercontent.com/9963310/94482353-6a6d2180-01d9-11eb-9638-657a4ff12ecf.png)

If you want to change the settings of a whole group of feeders, you can edit the template and then use the **Clone to Feeders** button. The various option checkboxes will determine which settings are cloned over. 

![Clone to Feeders](https://user-images.githubusercontent.com/9963310/94482663-f0896800-01d9-11eb-852c-73b10873dc64.png)

A similar function is available on the feeders that are **not** templates. It can be used to update just one feeder from the template. 

## Feeder Actuator

As a preparation for the next step, we need to create an Actuator on the machine Head. This Actuator will be used to mechanically execute the feeding motion and it is usually attached to a part of the Nozzle mechanics, so it will be able to move up and down in Z but does not rotate with it (this is just one obvious design choice but others are of course possible). The Actuator can have its own Offsets, but it is recommended to use the same Offsets as the Nozzle it is attached to, so the motion can be easily aligned with the pick location. 

![Actuator](https://user-images.githubusercontent.com/9963310/94699702-e7fc7300-033a-11eb-81e7-3897932c2621.png)

### Actuator Axis Mapping

If the Actuator moves in Z, make sure to map it to the Z axis. In current versions of OpenPnP map the axes by drop-down, right on the Actuator: 

![Axis Mapping Preview](https://user-images.githubusercontent.com/9963310/94711842-f3ef3180-0348-11eb-8412-66a50f14ae33.png)

In earlier versions of OpenPnP this requires the following steps:

1. Exit OpenPnP
2. Find and open the `machine.xml` (as [described here](https://github.com/openpnp/openpnp/wiki/FAQ#where-are-configuration-and-log-files-located))
3. Search for the name of the Actuator you created. 
4. Copy the `id` from there.
  
  ![Actuator ID](https://user-images.githubusercontent.com/9963310/94701105-8c32e980-033c-11eb-8b77-62cd0dd1f5cc.png)

5. Search for `<axis` with `type="Z"`.
6. Add the Actuator's `id` as a new `<string>` entry in the `<head-mountable-ids>`:

  ![Axis Z](https://user-images.githubusercontent.com/9963310/94701415-eaf86300-033c-11eb-9624-eb4ebab70ea9.png)

7. Save the machine.xml
8. Restart OpenPnP.
9. Select the Actuator in the Machine Controls.
10. Test the mapping by jogging up and down in Z.


### Actuator Boolean Command

The Actuator may also be assigned a Boolean command. It is switched ON after having moved to the Start Location and before performing the actual actuation motion. It is switched OFF again when the motion has finished. 

See the [Actuators page](https://github.com/openpnp/openpnp/wiki/Setup-and-Calibration_Actuators#assigning-commands) for more information on how to assign the command.

## Push-Pull Motion

Go back to the feeder and the Push-Pull Motion tab.

![Push-Pull Motion](https://user-images.githubusercontent.com/9963310/94712971-7a584300-034a-11eb-97be-f710354d3a62.png)

### Assigning Actuators

The **Actuator** can now be selected in the Push-Pull Settings. This is the motion actuator, press Apply to make sure, the capture buttons are wired to it. The dot symbols (as shown above) should now appear, instead of the nozzle symbols. 

A second **Peel Off Actuator** may be selected optionally. This one switches an assigned Boolean command ON once the End Location is reached. It will be switched OFF, when the motion is complete. 

### The Push-Pull Motion

As the name somewhat implies, the Push-Pull Motion is executed forward and backward along the given five Locations. However, the forward and backward motions need not be completely identical, you can skip Locations one way or the other. There are three columns of checkboxes where you can switch Locations on and off. The left column (↓) is going forward, the right column (↑) is going backward. 

A so-called multi-feed is needed, when the part pitch is larger than the feed pitch, i.e. when the tape needs to be advanced multiple times to go to the next part. It is also needed, if you chose to use a **Multiplier** greater than 1 (see [[Tape Settings|ReferencePushPullFeeder#tape-settings]] above). In these cases multiple actuations of the feeder are preformed one immediately after the other. Often, there is no point in completely disengaging the actuator from the feeder mechanics, instead you only need to repeat a central part of the actuation motion. Use the middle column (↑↓) switches to set this up. A Location is only included in the multi-feed forward/backward motion, if both the middle column (↑↓) switch and the corresponding right (↑) or left (↓) switch are ON, respectively. 

### Motion Path / Locations

You can now enter or capture the up to five Locations using the common OpenPnP capture buttons. 

The **Speed** fields can be used to control the speed factor of the motion between the Locations. The left column (↓) controls the forward going speed, the right column (↑) the backward going speed. 

All these Locations are handled like the Pick and Sprocket Hole Locations discussed in the [[Locations|ReferencePushPullFeeder#locations]] section above i.e. they are automatically vision calibrated and automatically transformed if a feeder is moved around on the machine e.g. by OCR detection or if a new feeder is created from a template. This even supports rotation, so if a "west" feeder is later loaded at a "south" slot, this is not a problem. 

### Apply Vision Calibration, Not?

Sometimes it is better to _not_ calibrate a coordinate axis of the push-pull motion. This is the case when the motion directly transports the tape such as in drag feeding. In this case it is better to _disable_ **Vision Calibrate?** for the coordinate going into the direction of transport. Otherwise, the drag might be offset more and more, as errors accumulate. 

Note, if you _disable_ **Vision Calibrate?**, the transport motion will be applied "blindly" in the said coordinate, but this is likely OK, as the feeder will allow for some tolerances. For example, the drag needle is typically smaller in diameter than the sprocket hole it engages with. Make sure to make the insertion point go into the center of any wiggle room. 

![Vision Calibrate](https://user-images.githubusercontent.com/9963310/183599853-fb1adc39-c3c9-4bb9-847f-28f98283be8c.png)


### Coordinated Peeling

Cover tape peeling can be done using motion that is coordinated with the push-pull articulation, as needed for drag feeders etc. This allows for an elaborate and coordinated motion pattern. Furthermore, it performs the peeling motion _in parallel_ with the other feeding articulation, potentially saving time. 

#### Peeler Axis

Create a rotation axis for the peeler. Make sure to _disable_ the **Limit to Range** and **Wrap Around** options, as we want continuous wind-up of the peeled-off cover tape:

![Rotation Axis](https://user-images.githubusercontent.com/9963310/158563357-84e92d16-36d0-4ecb-a56a-63e8c6f3701e.png)

Assign the created axis to the push-pull actuator:

![Assign to Actuator](https://user-images.githubusercontent.com/9963310/158563736-3c94a813-d7ef-43c6-bf0e-9a9ce76aa34d.png)

#### Driver G-code for the new Peeler Axis

Do not forget to use [[Issues and Solutions]] to generate new G-code fragments and regular expressions for your drivers, i.e. to include the new axis in them. Be sure to press **Find Issues & Solutions** freshly, after having defined and assigned the new axis in the **Machine Setup**. 

#### Configure Coordinated Peeling
On the `ReferencePushPullFeeder` use the new **Rotation** column to configure the peeling coordinates. 

For a typical configuration, the **Additive** switch needs to be enabled, so the peeling motion is _additive/relative_ to the starting point. The starting point is the peeler axis coordinate, right before the feed actuation. If **Additive** is enabled, you cannot use the same way-points back and forth, obviously (it would go back), you need to define the feed articulation using the forward-going way-points only, i.e., be sure to disable all the ↑ column check-boxes. If the five way-points are not enough, please report to the [discussion group](http://groups.google.com/group/openpnp). 

Some feeders need multi-actuation, for instance if the part pitch in the tape is larger than the maximum tape feed that can be done by the mechanical motion. To enable multi-actuation together with additive rotation, you can enable the ↑↓ column check-boxes. But you still should **not** enable the ↑ column ones.

The **Reset** button can be used to reset the starting point to the current peeler axis coordinate. After pressing it, you can use the capture and positioning buttons to get and test the _additive/relative_ locations of the push-pull motion, _including_ the peel.

![Push-pull motion with Rotation](https://user-images.githubusercontent.com/9963310/159045634-fd7ff079-0e69-4fde-b36d-1bfbddca30bd.png)

## The next Feeder

All this was hard work. Now comes the fun part. If your feeders are 

* the same design,
* arranged in a row, 
* have an OCR label, 
* identifying a part with known tape specification or package, 

you can just press the green **Plus One** button:

![Plus One](https://user-images.githubusercontent.com/9963310/94719565-a3310600-0353-11eb-8f1f-52ff2ae0ca68.png)

OpenPnP will then automatically detect the next feeder and fully set it up with cloned settings and transformed locations. **This is litterally a One-Click-Setup**. 

If this is only the second feeder in a row, some guesswork is in play. It will try to find the next one on the right side of the current feeder (going counter-clockwise around the machine edge) and it will use a standard tape width + 8mm grid (that's the [all-3D-printed feeder's](https://makr.zone/new-all-3d-printed-tapereel-feeder/399/) design spec). If this doesn't work, move the camera to the next feeder's pick location and press **Auto-Setup**. Done. 

If this is the third or any later feeder in a row, it will have learned the row grid and direction and One-Click-Setup should always work. 

Even if the part has an unknown tape specification/package, some settings will be cloned from the best match. These will only have to be adjusted where there are differences. Because the ReferencePushPullFeeder is driven by sprocket hole vision and the semantics of EIA 481, a new package/tape specification is often configured by simply adjusting the **Part Pitch**, everything else remains the same or is automatically recalculated under the hood. 

Enjoy!
