## What is it?

The ReferenceLinearTransformAxis is a multi-input axis transformation. It can take the coordinates of several input axes and create one transformed output coordinate. 

In OpenPnP the typical use case is to compensate for non-squareness of the machine, i.e. when the axes are mechanically not perfectly 90° to each other. On this page we will mostly just explore this problem.

However, non-squareness compensation is just one possibility out of many. Multiple ReferenceLinearTransformAxis can be combined to create a full [Affine Transformation](https://en.wikipedia.org/wiki/Affine_transformation). Some pointers [at the end](#other-uses).

## ReferenceLinearTransformAxis

### Create an Axis

Create an axis in the Machine Setup tab.

![Create Linear Transform Axis](https://user-images.githubusercontent.com/9963310/95999610-5a3e7e80-0e36-11eb-8503-a12c31ec4313.png)

### Properties

![ReferenceLinearTransformAxis](https://user-images.githubusercontent.com/9963310/96001507-64fa1300-0e38-11eb-8336-0e7e7c1220b6.png)

**Type** and **Name** are the same as in the [[ReferenceControllerAxis|Machine-Axes#properties]]. 

In this case, **Type** determines which type of axis is output. 

### Linear Transformation

We are defining this transformation as a **forward transformation** here, as it happens mechanically, from the coordinates on the mechanical axes to the true geometric coordinates in space. 

Of course, OpenPnP will internally derive the _reverse_ transformation, from a desired true geometry coordinate, _back_ to the coordinates of the mechanical axes. But you don't need to know anything about that. For the math guys: yes, it does create and invert the [augmented Affine Transformation Matrix](https://en.wikipedia.org/wiki/Transformation_matrix#Affine_transformations). 

**X**, **Y**, **Z** and **Rotation** input axes can be selected.

Each of the input coordinates taken from the input axes are then multiplied by a certain **Factor**. For the principal axis (the one corresponding to the **Type**), the **Factor** is often `1`. 

All these terms are then summed up. 

Finally, an **Offset** is added.

**Compensation?** tells OpenPnP if this is a compensation transformation i.e. repairing a mechanical imperfection rather than applying a genuinely wanted transformation. The switch can help OpenPnP with certain optimizations and for proper simulation. 

## Use Case : Non-Squareness Compensation

Even after having tried to mechanically square a machine (see the [[Machine Squaring Guide]]) it is hardly _ever_ perfect. Other machines cannot be mechanically squared, due to technical or safety constraints. A mild non-squareness is irrelevant for the Pick&Place use case, as it is not very demanding, and OpenPnP has various built-in compensation capabilities. However, inexpensive (DIY) machines are prone to exhibit noticeable non-squareness that may matter practically. So instead of buying much more expensive hardware, let's use a piece of clever software and some brains to compensate. The following illustration shows the problem and steps to solve it: 

![Non-Squareness-Compensation](https://user-images.githubusercontent.com/9963310/96003787-e488e180-0e3a-11eb-8732-0c4d06ca2b33.png)

Using a trusted precision square or a large millimeter paper, you should be able to detect the non-squareness using the camera crosshairs. The following recipe works with millimeter paper:

1. Align the camera crosshairs with the horizontal line of the millimeter paper.
2. Move X as far as the line goes and carefully align the horizontal crosshair with the line, trying to keep the already aligned starting point pinned. It may need multiple passes back and forth.
3. Go to a vertical line of the millimeter paper.
4. Move in Y as far as the paper goes, stop at a known Y distance. 
5. Determine the offset in X (red arrow in the illustration).
6. Divide the offset by the distance in Y.
7. Enter the result in the Y Input Factor.

This is just one example, fixing the X/Y non-squareness using a compensation in X. If you prefer, you could also compensate in Y. If your machine table is uneven, compensate in Z. Any axis can be transformed, even in combination. 

**IMPORTANT:** The created axis must now be used **instead of the physical axis** in the [[Axis Mapping|Mapping Axes]].

## Other uses

### Rotate the Machine Table

Just to show off what it can do, the following would rotate your X-Y plane by -45° around Z :-)

![Rotate X](https://user-images.githubusercontent.com/9963310/96012041-fe7af200-0e43-11eb-8ffc-dc99d541938c.png)
![Rotate Y](https://user-images.githubusercontent.com/9963310/96011777-a8a64a00-0e43-11eb-8ba3-16e3e6682347.png)

### Advanced Applications

Advanced multi-axis Affine transformations are beyond the scope of this page. Use external tools to compute the **Factors** and **Offsets**. 

![Affine Matrix Effects](https://user-images.githubusercontent.com/9963310/96009379-e0f85900-0e40-11eb-9694-b890f19a6e02.png)

> Effect of applying various 2D affine transformation matrices on a unit square. 

From the Wikipedia, [Affine transformations](https://en.wikipedia.org/wiki/Transformation_matrix#Affine_transformations).



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
