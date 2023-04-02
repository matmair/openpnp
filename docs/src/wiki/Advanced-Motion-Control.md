## What is it?
Advanced Motion Control is just a summary label for a new feature-set that aims at improving OpenPnP Motion Control. Better Motion Control should improve the precision and sometimes the speed of operations through various optimizations.

Because Motion Control is such a fundamental function inside OpenPnP, the feature-set has touched many parts in a Machine Setup: Axes, Drivers, Nozzles, Cameras, Actuators and the new Motion Planners. To put it all together, this page must also touch and connect many of these topics. 

## Feature List
Advanced Motion Control aims to improve these areas:

* Simpler all-GUI setup of [[Machine-Axes]], [[Transformed-Axes]] and their [[assignment to Nozzles, Cameras etc.|Mapping-Axes]] (no more `machine.xml` hacking).
* Making features such as [[Axis Mapping|Mapping-Axes]], [[Backlash Compensation]], [[Visual Homing]], [[Non-Squareness Compensation|Linear-Transformed-Axes#use-case--non-squareness-compensation]] etc. available for all types of drivers (formerly just the GcodeDriver).
* Allowing multiple drivers of mixed types. 
* Simpler and more unified G-code configuration of axes in (multiple) drivers. 
* Better control of speed factors: properly controls the _average_ speed, including acceleration/deceleration phases. A move at 50% takes exactly twice as long, regardles of how short or long the move is. Formerly, just the maximum feed-rate was limited, often having no effect, as it was never reached on short moves. 
* Operations at reduced speed factor perform much gentler due to reduced acceleration (and jerk). Improves handling delicate parts, nozzle tip changing, [[mechanical feeder operation|ReferencePushPullFeeder]], etc.
* Per axis [[feed-rate, acceleration (and optionally jerk) limits|Machine-Axes#kinematic-settings--rate-limits]]. Most importantly, this allows separate control of (nozzle) rotation rate limits for higher angular speeds.
* Added jerk control to ...
* ... reduce vibrations, therefore reduce needed camera settling times but also improve pick and place accuracy. 
* ... prevent any slipping of parts on the nozzle (for cheap DIY vacuum systems).
* ... allow for higher peak acceleration without stalling steppers, improving the speed of long moves.
* Paralellized operation and asynchronous communication between OpenPnP and (multiple) controllers to improve throughput and reduce delays.
* (Experimental) Motion Blending, to improve speed.
* Graphical diagnostics for Motion Planning as a basis for _fact based_ machine optimization. 

On one hand, adding jerk control does reduce the motion speed of the machine per se. On the other hand, due to reduced vibrations, some of that loss can be regained through shorter [[Camera Settling]] times and it turns out (somewhat unexpectedly) through much improved pick accuracy, needing fewer bottom vision alignment passes or even allowing the elimination of bottom vision altogether for some parts (e.g. small passives). Other features clearly improve the speed, with the most promising improvment (Motion Blending) still being experimental. 

![AdvancedMotionAnimation](https://user-images.githubusercontent.com/9963310/95627544-ab3c2480-0a7c-11eb-8d36-d6921ecf7423.gif)

Whether the machine will be slower or faster in the end, probably depends on the machine, the controller, the parts etc. but it seems that accuracy and reliability of operations will benefit for sure. 

## Optional and Step-by-Step

It is one design goal of the Advanced Motion Control feature set to be _optional_ in OpenPnP and to provide a continuous experience for those who don't want to use it. Almost all the setting of a previous OpenPnP machine configuration should automatically be migrated to the new version. The machine should work the same way as before. 

Some of the new features are visible in the GUI, such as [[Machine Axes]] / [[Axis Mapping|Mapping-Axes]] that was formerly done by hacking the `machine.xml` file. Some features have moved to different parts of the GUI such as [[Backlash-Compensation]]. But all the really "advanced" features are initially inactive or even hidden to keep it simple. This guide aims to document these features so you can enable them step-by-step. 

Note: This guide also acts as a repository for instructions that have completely changed from the previous ways. Some parts may later be incorporated into existing Wiki pages to replace or complement existing instructions. 

## Migration from a previous Version 

OpenPnP should migrate all but the most exotic machine setups automatically from previous OpenPnP 2.0 versions. After the migration, the machine should work mostly as before. But this also means, you're missing out on most of the new features. Because the new advanced features can be a bit daunting to configure, a new [[Issues and Solutions]] system has been created. You can use it to automatically setup most of the new features. The system will take your special machine configuration fully into consideration. 

[[Issues and Solutions]] can't extract _everything_ from a previous OpenPnP configuration. Some things need to be set manually, by the user. One important example are G-code axis letters (like `X`, `Y`, `Z`, `A`, `B`, `C`). You need to obtain them from your controller configuration and enter them into OpenPnP on the corresponding [[ReferenceControllerAxis|Machine-Axes#referencecontrolleraxis]]. OpenPnP can then use these to automatically propose an advanced G-code configuration for you. See the [[Axis Letter property|Machine-Axes#controller-settings]] for specific information. 

Other such things will be pointed out by [[Issues and Solutions]]. 

____

**First Stop**: Check out the [[Issues and Solutions]] page. 
____

**Important:** Only if the [[Issues and Solutions]] system does not help you, should you continue here. To complement the automatic system, the following background information details the most important needed changes to prepare for Advanced Motion Control features:

1. If you use more than four axes on one controller: check out if you can use it **without** pre-move commands (the `T` letter commands used to switch between extruder `E` axes). [[Smoothieware, Duet3D, Bill's fork of Marlin 2.0|Motion-Controller-Firmwares]] and possibly others can be used with proper axis letters `A`, `B`, `C` instead. 
**Warning**: if this is not the case, only a limited number of advanced features will be available. **This guide assumes you do not have pre-move commands, so if you continue you are on your own!** 

2. Upgrade your [[controller firmware|Motion-Controller-Firmwares]] if necessary. 

3. Go to each of your GcodeDrivers, enable **Letter Variables?** and disable **Pre-Move Commands?** (other settings will be explained later).

   ![GcodeDriver Migration](https://user-images.githubusercontent.com/9963310/96035272-1746d000-0e63-11eb-8ff8-94f3a0c7a67d.png)

4. The Axes are now in the GUI (formerly a proprietary part of the GcodeDriver). This guide assumes that you checked them out and read about the setup as needed for your machine. See the [[Machine Axes]], [[Transformed Axes]], [[Linear Transformed Axes]] pages plus the parts about [[Mapping Axes]] and [[Backlash-Compensation]].

5. Make sure you have assigned the correct **Axis Letter** to each controller axis as described in the [[Controller Settings|Machine-Axes#controller-settings]].

6. Go to each of the GcodeDrivers and create a **Default** `MOVE_TO_COMMAND` that moves **all** the axes of your controller **at once**, using the **Axis Letters** as the variable names. 

    ![Gcode Editing](https://user-images.githubusercontent.com/9963310/96037872-abfefd00-0e66-11eb-9639-46ba5dfa13fb.png)

    Add the acceleration command in front. Make sure to move any G-code letter inside the curly brackets (including the `F` letter, formerly outside). Remove any `Backlash` variables and extra commands if present (best to start fresh, if you had them):

    `{Acceleration:M204 S%.2f} G1 {X:X%.4f} {Y:Y%.4f} {Z:Z%.4f} {A:A%.4f} {B:B%.4f} {FeedRate:F%.2f} ; move to target`

    **NOTE**: the example commands shown here are for Smoothieware. They might differ on other controllers. 

7. Remove any `MOVE_TO_COMMAND`s from the other Head Mountables. They are no longer needed.

8. Create a **Default** `SET_GLOBAL_OFFSETS_COMMAND`, again for all the axes of that controller at once and using the **Axis Letters** as the variable names:

    `G92 {X:X%.4f} {Y:Y%.4f} {Z:Z%.4f} {A:A%.4f} {B:B%.4f} ; reset coordinates in the controller` 

9. Delete any `POST_VISION_HOME_COMMAND`.

10. Create a `GET_POSITION_COMMAND`. The command must report all axes (for Smoothieware, you must use [my special firmware](https://makr.zone/smoothieware-new-firmware-for-pnp/500/)).

    `M114 ; get position`

11. Create or change the `POSITION_REPORT_REGEX`, again for all the axes of that controller at once and using the **Axis Letters** as the regex group names:

    `^.*X:(?<X>-?\d+\.\d+) Y:(?<Y>-?\d+\.\d+) Z:(?<Z>-?\d+\.\d+) A:(?<A>-?\d+\.\d+) B:(?<B>-?\d+\.\d+).*`

12. Test the machine. Jog around a bit.

## Next step

Create or migrate to the [[GcodeAsyncDriver]].

___

## Advanced Motion Control Topics

### Motion Control
- [[Advanced Motion Control]]
- [[GcodeAsyncDriver]]
- [[Motion Planner]]
- [[Visual Homing]]
- [[Motion Controller Firmwares]]

### Machine Axes
- [[Machine Axes]]
- [[Backlash-Compensation]]
- [[Transformed Axes]]
- [[Linear Transformed Axes]]
- [[Mapping Axes]] 
- [[Axis Interlock Actuator]]

### General
- [[Issues and Solutions]]