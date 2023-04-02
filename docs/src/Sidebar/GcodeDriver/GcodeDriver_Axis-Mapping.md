# Before You Start
___
**Note**: With newer Versions of OpenPnP 2.0, most of the following is obsolete. Use the [[Machine Axes]], [[Mapping Axes]] and [[Transformed Axes]] GUI based setup instead. 
___

Axis Mapping requires editing the `machine.xml` configuration file directly. Since most of OpenPnP can be configured through the GUI, this may be new to you. See https://github.com/openpnp/openpnp/wiki/FAQ#where-are-configuration-and-log-files-located for information about where to find the file.

Note that when editing `machine.xml` by hand, you should make sure OpenPnP is not running. When you exit OpenPnP it saves the file so your changes might be overwritten if you leave it running.

In this document, there is some terminology that may be new as well. Here are some terms that will help you understand:

* **Head Mountable**: In OpenPnP a Head Mountable is a general term for a device that can be mounted to a head. This includes nozzles, cameras, actuators, and dispensers.
* **ID** or **id**: Every Head Mountable has a unique ID that identifies it in the configuration file. For example, a Nozzle definition may look like this: `<nozzle class="org.openpnp.machine.reference.ReferenceNozzle" id="N1" name="N1">`. This Nozzle's ID is shown as `id="N1"`, so this Nozzle's ID is N1.
* **head-mountable-id**: Another name for ID. This tells you that this is a place where you can put the ID of a Head Mountable like a Nozzle or Camera.

# Defining Axes
___
**Note**: With newer Versions of OpenPnP 2.0, the following is obsolete. Use the [[Machine Axes]] GUI based setup instead. 
___

The first step in axis mapping is to define each axis that your motion controller can control. In general these will correspond to motors on your machine, but not always. Another way to think of it is to consider each individual part of the machine that can move.

For instance, a common machine might have an X axis, a Y axis, a Z motor that controls two Z axes for two nozzles and a C or Rotation motor for each Nozzle. This machine has 5 axes: X, Y, Z, C1, C2.

A basic axis definition looks like:
```
   <axis name="x" type="X" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>*</string>
      </head-mountable-ids>
   </axis>
```

# Mapping Axes to HeadMountables
___
**Note**: With newer Versions of OpenPnP 2.0, the following is obsolete. Use the [[Mapping Axes]] GUI based setup instead. 
___

In OpenPnP anything that can be attached to the head is called a HeadMountable. These are your Nozzles, PasteDispensers, Cameras and Actuators. Each of these has an ID.

In the example we defined above, the machine has two nozzles that we'll call N1 and N2. The two nozzles share a Z axis and each has it's own C axis.

Here is an example axis mapping for this machine setup. We'll break it down by section further below:

```         
 <axes class="java.util.ArrayList">
   <axis name="x" type="X" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>*</string>
      </head-mountable-ids>
   </axis>
   <axis name="y" type="Y" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>*</string>
      </head-mountable-ids>
   </axis>
   <axis name="z" type="Z" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>69edd567-2341234-495a-9b30-2fcbf5c9742f</string>
         <string>69edd567-df6c-495a-9b30-2fcbf5c9742f</string>
      </head-mountable-ids>
      <transform class="org.openpnp.machine.reference.driver.GcodeDriver$NegatingTransform">
         <negated-head-mountable-id>69edd567-2341234-495a-9b30-2fcbf5c9742f</negated-head-mountable-id>
      </transform>
   </axis>
   <axis name="c1" type="Rotation" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>69edd567-2341234-495a-9b30-2fcbf5c9742f</string>
      </head-mountable-ids>
      <pre-move-command>T0</pre-move-command>
   </axis>
   <axis name="c2" type="Rotation" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>69edd567-df6c-495a-9b30-2fcbf5c9742f</string>
      </head-mountable-ids>
      <pre-move-command>T1</pre-move-command>
   </axis>
</axes>
```

Now, section by section:

Define the X axis. Since it's head-mountable-ids list contains * it will map the X axis to all head mountables.
```         
   <axis name="x" type="X" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>*</string>
      </head-mountable-ids>
   </axis>
```

Define the Y axis. Like the X axis, it is mapped to all head mountables.
```
   <axis name="y" type="Y" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>*</string>
      </head-mountable-ids>
   </axis>
```

Define the Z axis. This is where it starts to get interesting. The Z axis lists two head mountables which are the IDs for nozzle 1 and nozzle 2. By not including * or the IDs of the cameras we declare that the camera does not move in Z.

In addition, since this machine uses a shared Z motor for two Z head mountables, we define a transform that inverts the coordinate for the second nozzle. All this means is that when one nozzle is up, the other is down.
```
   <axis name="z" type="Z" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>69edd567-2341234-495a-9b30-2fcbf5c9742f</string>
         <string>69edd567-df6c-495a-9b30-2fcbf5c9742f</string>
      </head-mountable-ids>
      <transform class="org.openpnp.machine.reference.driver.GcodeDriver$NegatingTransform">
         <negated-head-mountable-id>69edd567-2341234-495a-9b30-2fcbf5c9742f</negated-head-mountable-id>
      </transform>
   </axis>
```

Now we define the c1 axis, which is the rotational axis for the first nozzle. We include only the head mountable ID of the first nozzle, so this axis will only affect that nozzle. In addition, we have included a pre-move-command. This command is sent before this axis is moved. In this case we are sending a `T0` command which will signal the controller to control the first tool, or extruder.
```
   <axis name="c1" type="Rotation" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>69edd567-2341234-495a-9b30-2fcbf5c9742f</string>
      </head-mountable-ids>
      <pre-move-command>T0</pre-move-command>
   </axis>
```

Finally, we define the second rotational axis and include a `T1` pre-move-command. Note that we also use the head mountable ID for the second nozzle.
```
   <axis name="c2" type="Rotation" home-coordinate="0.0">
      <head-mountable-ids class="java.util.HashSet">
         <string>69edd567-df6c-495a-9b30-2fcbf5c9742f</string>
      </head-mountable-ids>
      <pre-move-command>T1</pre-move-command>
   </axis>
```
## Pre Move Command
___
**Note**: With newer Versions of OpenPnP 2.0 you should try to avoid pre-move commands. Many firmwares can work with proper **Axis Letters** (`A`, `B`, `C` axes instead of the "extruder" style `E` axis). See the [[Motion Controller Firmwares]] to check your controller's support and the [**Letter Variables** option in the driver](https://github.com/openpnp/openpnp/wiki/GcodeAsyncDriver#gcodedriver-new-settings). 
___
The `pre-move-command` shown in examples above supports a `Coordinate` variable which can be used to reset the axis before moving it. This is useful for motion controllers that do not support multiple extruder coordinates, for example. Usage looks like:

`<pre-move-command>T0G92E{Coordinate:%.4f}</pre-move-command>`

For more information, see:
* https://github.com/openpnp/openpnp/blob/develop/CHANGES.md#2017-10-21
* https://groups.google.com/d/msgid/openpnp/d7a88150-6e12-4532-b0fa-6971da25c557%40googlegroups.com

# Axis Transforms
___
**Note**: With newer Versions of OpenPnP 2.0, the following is obsolete. Use the [[Transformed Axes]] GUI based setup instead. 
___

## NegatingTransform

For machines with dual nozzles controlled by a single Z motor in a counterweight configuration. This works for rack and pinion drives and belt drives. It does not work for cam based drives. Commonly referred to as "Peter's Head".

Example:
```
<axis name="z" type="Z" home-coordinate="0.0">
  <head-mountable-ids class="java.util.HashSet">
     <string>N1</string>
     <string>N2</string>
  </head-mountable-ids>
  <transform class="org.openpnp.machine.reference.driver.GcodeDriver$NegatingTransform">
     <negated-head-mountable-id>N1</negated-head-mountable-id>
  </transform>
</axis>
```

The `negated-head-mountable-id` specifies which nozzle will have it's coordinate inverted. It's not important to understand the math. Just try one and if the nozzles work backwards, try the other.

## CamTransform

For machines with dual nozzles controlled by a single Z motor in a seesaw or rocker configuration where a cam is used to push down either the left or right nozzle.

Example:
```
<axis name="z" type="Z" home-coordinate="0.0">
   <head-mountable-ids class="java.util.HashSet">
      <string>N1</string>
      <string>N2</string>
   </head-mountable-ids>
   <transform class="org.openpnp.machine.reference.driver.GcodeDriver$CamTransform" cam-radius="24.0" cam-wheel-radius="9.5" cam-wheel-gap="2.0">
      <negated-head-mountable-id>N2</negated-head-mountable-id>
   </transform>
</axis>
```

It is important to set the cam-radius, cam-wheel-radius, and cam-wheel-gap settings correctly or behavior will be incorrect. See below for their meanings:

* **cam-radius**: This is the distance from the center of the motor shaft to the center of one of the wheels at the end of the cam.

* **cam-wheel-radius**: This is the radius of the wheels or bearings at the end of the cam, typically used to make contact with the Z1 or Z2 axis.

* **cam-wheel-gap**: The gap between the edge of the cam wheels to the top of the Z axes. This will often be 0 but if there is some space between the wheel and the top of the axis you should set this value to that measurement.

This image helps illustrate the various settings:

![screen shot 2017-02-09 at 6 53 07 pm](https://cloud.githubusercontent.com/assets/1182323/22809587/1b783f3e-eef9-11e6-8afe-fd45c434af34.png)

Note that after enabling the transform your nozzles will show a Z value that is the combination the cam values. To reset this back to 0 just set the Z head offset of each nozzle to the negative of the offset in Machine Setup -> Nozzles.