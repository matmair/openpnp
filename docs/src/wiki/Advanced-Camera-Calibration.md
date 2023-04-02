# Purpose
The purpose of advanced camera calibration is to:
1. Determine the head offset (or absolute location for bottom cameras) and orientation of the camera;
1. Determine the scaling of machine units to camera pixels, i.e., units per pixel and how it varies with machine Z;
1. Correct for imperfections in how the camera is mounted, i.e., rotation from ideal about the X, Y and Z axis; and,
1. Correct for lens imperfections, i.e., barrel/pincushion and tangential distortion.

![perpendicular-constant-xy-diff-z](https://user-images.githubusercontent.com/9963310/182214401-8bcf0d07-6fc5-4d22-b0e8-7472a6c4739f.png)

# How It Works
Advanced Camera Calibration works by creating a mathematical model of the camera that basically takes machine 3D coordinates and maps them to image pixel 2D coordinates. Calibration data is collected that consists of a large set of machine 3D coordinates and their corresponding image pixel 2D coordinates. The camera model is then "fitted" to the data such that the difference between the modelled image coordinates and the actual image coordinates is minimized. The camera model is then used to generate a remapping of image pixels from the physical camera to correct for the above mentioned imperfections in the camera. The camera model also includes information used to determine the head offset (or absolute location for bottom cameras) as well as information to determine the scaling of machine units to the camera pixels at any given Z coordinate. 

# Enabling/Disabling
Advanced camera calibration is enabled/disabled for a particular camera by selecting the camera in the Machine Setup tree, selecting the Experimental Calibration tab, and checking/clearing the check box "Enable experimental calibration to override old style image transforms and distortion correction" (see below). Other than the settings under the General Settings section, no other settings on this tab will have any effect on machine operation if this check box is cleared (all the settings prior to running Advance Camera Calibration will be restored):
![Experimental Calibration Tab](https://user-images.githubusercontent.com/50550971/142742856-25c775bd-4a1f-491c-aba2-52df06feb644.png)

# General Settings
![General Settings](https://user-images.githubusercontent.com/50550971/142742896-e4d0a26c-88ca-4f3b-a093-b431f3e6e8f1.png)

The settings here are simply copies of settings available on other tabs.  They are provided here as a convenience since they should be set to their desired values prior to running Advanced Calibration.  Any changes made here take effect immediately and will change the corresponding value on any other tabs on which they appear.

Note that **Cropped Width** and **Cropped Height** are applied to the raw camera images prior to any other corrections made by this calibration.  Therefore, calibration data is only collected over the cropped image and any corrections will only be valid over that portion of the raw image. 

# Calibration Setup
![Calibration Setup](https://user-images.githubusercontent.com/50550971/142742909-b9e190e7-5c66-4c94-8733-4fa8d992a253.png)

The **Primary Cal Z** and **Secondary Cal Z** settings determine the Z coordinate of the calibration test patterns that will be used to calibrate the camera.  For top cameras, these are set to the Z coordinate of the Primary and Secondary Calibration Fiducials respectively and are not editable here.  For bottom cameras, the **Primary Cal Z** is set to the Z coordinate where the nozzle tip is in best focus as determined by auto focus (not editable here) and the **Secondary Cal Z** is set by default to half of the primary setting. The operator should set this as high (more positive) as possible but still keeping the nozzle tip in reasonable focus. 

The **Radial Lines Per Cal Z** setting is used to control how many calibration data points are collected during the calibration process.  The calibration data is collected along lines that start at the center of the image and proceed out to the edge of the image.  This setting controls how many such lines are used.  To keep things symmetrical, this setting gets rounded up to the next multiple of four.  Reducing this number will reduce the duration of the collection sequence but at the risk of lower quality or even failed calibration. Increasing this number will increase the duration of the collection but may result in a higher quality calibration.

The **Start Calibration** button starts the calibration data collection process.  All settings on this tab above the button should be setup before starting calibration. During the calibration collection process, instructions/status will appear below the camera view guiding the operator through the process. A cancel button is also displayed allowing the operator to abort the calibration collection:
![Instructions/Status](https://user-images.githubusercontent.com/50550971/134715025-15d9b20d-ef11-4ad6-b3ec-34d5fdf1e4a5.png)

The **Detection Diameter** spinner sets the size of the fiducial/nozzle tip that is used.  During the calibration collection process, the operator will be instructed to set this at the appropriate times.

Once the calibration data has been collected and successfully processed, the **Apply Calibration** check box will automatically be selected thereby applying the calibration to the camera and enabling corrected images to be displayed in the Camera View.  Clearing this checkbox, results in raw uncorrected images being displayed in the Camera View.

The **Crop All Invalid Pixels <--> Show All Valid Pixels** slider at the bottom of this section allows the operator to control the cropping of invalid pixels along the edges of the calibrated camera images. Invalid pixels (usually displayed as black) on the edges of the image may result due to the image processing that compensates for errors in camera mounting and lens distortion. While mostly aesthetic, changing this setting does change the Units Per Pixel for the camera.  Setting the slider to the far right ensures all available pixels from the camera are displayed.  Setting the slider to the far left ensures that the edges of the image are cropped rectangularly and symmetrically about the reticle crosshairs so that no invalid pixels remain at the edges. Intermediate settings will produce results between the two extremes. See the animation below for a visualization of how this works:

![Slider Animation](https://user-images.githubusercontent.com/50550971/135167265-86140afa-a279-4c7c-8bbe-4fdcd8cdedfa.gif)

## Calibration Data Collection
Upon clicking the **Start Calibration** button, the calibration proceeds as follows:

### Top Cameras
1. The operator is requested to jog the camera so that the Primary Calibration Fiducial is centered.
1. The operator is requested to adjust the Detection Diameter spinner to achieve stable detections on the fiducial.
1. The camera is moved in a couple of small steps to obtain an estimate of the camera's units per pixel at the Z coordinate of the Primary Calibration Fiducial.
1. The camera is moved so that the Primary Calibration Fiducial is exactly centered.
1. The camera is moved in steps along radial lines so that the fiducial moves from the center of the image to the edge of the image. The radial lines are equally spaced angularly about the center but are visited in a random order. After a step on all of the lines is completed, the camera is moved to the next step out from the center and all the lines are again visited in random order.  This continues until all the lines are complete.
1. Steps 1 - 5 are repeated with the Secondary Calibration Fiducial.
1. The calibration data is processed, and if successful, the results are applied.
 
### Bottom Cameras
1. The operator is requested to load the smallest nozzle tip.
1. The operator is requested to jog the nozzle tip so that it is approximately centered in the camera view.
1. The nozzle tip is moved to the Primary Calibration Z and the operator is requested to verify that the nozzle tip is still centered.
1. The operator is requested to adjust the Detection Diameter spinner to achieve stable detections on the nozzle tip.
1. The nozzle tip is moved in a couple of small steps to obtain an estimate of the camera's units per pixel at the Primary Calibration Fiducial.
1. The nozzle tip is moved so that it is exactly centered.
1. The nozzle tip is moved in steps along radial lines so that it moves from the center of the image to the edge of the image. The radial lines are equally spaced angularly about the center but are visited in a random order. After a step on all of the lines is completed, the nozzle tip is moved to the next step out from the center and all the lines are again visited in random order.  This continues until all the lines are complete.
1. Steps 3 - 7 are repeated but at the Secondary Calibration Z
1. The calibration data is processed, and if successful, the results are applied.

# Results/Diagnostics
![Results/Diagnostics](https://user-images.githubusercontent.com/50550971/142743235-727d22b7-29f0-4c87-a18b-fc6ad4f25971.png)

The first item displayed here is the **Calibrated Head Offsets** (for top cameras) or the **Camera Location** (for bottom cameras). For top cameras, this is the horizontal offset compensation needed due to the optical axis of the physical camera not being perfectly vertical. This keeps coordinates captured with the top camera at the height of the primary calibration fiducial prior to this calibration consistent with coordinates captured after this calibration. For bottom cameras, this is the X-Y coordinates of the physical camera and Z is the Z coordinate determined by auto focus.

**Units Per Pixel** displays the computed units per pixel at default Z for this camera. Note that images are scaled such that the units per pixel is the same in both the X and Y directions.

**Estimated Locating Accuracy** displays an estimate of the expected accuracy of locations that are captured by using this camera. This includes not only inaccuracies due to uncompensated residual camera errors but also inaccuracies due to machine mechanics such as stepper motor non-linearities, mechanical backlash, etcetera.  As this is a statistical measure, it should be expected that about 63 percent of all measurements will be better than this and that 98 percent of all measurements will be better than double this value. See [Circular Error Probable](https://en.wikipedia.org/wiki/Circular_error_probable) for more information.

**Physical Field-Of-View** displays the estimated angular field-of-view for the physical camera. Note that width and height here refer to the camera's raw images without regard to how it may be mounted to the machine.

**Effective Field-Of-View** displays the estimated angular field-of-view for the camera after all corrections are applied.

**Camera Mounting Error** shows the estimated mounting errors of the camera as measured by the right hand rule about each machine axis. See [here](https://github.com/openpnp/openpnp/wiki/Advance-Camera-Calibration---Camera-Mounting-Errors#camera-mounting-errors) for how mounting errors effect the images as seen in the camera view pane.  

The **Select Cal Z For Plotting** spinner selects the data to be displayed in the plots below.

The **Show Outliers** checkbox enables data points that were judged to be bad measurements to be displayed in the plots.

The three plots at the bottom of this section all display the residual errors of the calibration data although in different ways. Residual error is defined as the difference between where the fitted model predicts where the detected fiducial/nozzle tip should have appeared in the image and where it actually appeared in the image. To be more meaningful to the operator, the residual errors are converted from differences in pixels to differences in machine units at the Z level at which the calibration data was collected.

The **Residual Errors In Collection Order** plot shows the individual X and Y components of residual error as a time ordered sequence. The residual errors should have zero mean and appear as random noise. If there are significant steps in the mean level or trends in the errors; depending on their magnitude, there may be a problem with the calibration. Some possible causes are: calibration fiducial/nozzle tip movement/slippage during the collection; camera or lens moving in its mount; motors missing steps; belt/cog slippage; thermal expansion/contraction; etcetera.

The **Residual Error X-Y Scatter Plot** shows the residual error for each collected data point as a dot on a scatter plot where the magnitude of the residual error is the distance of the dot from the origin. A green circle marks the approximate boundary at which points are considered to be outliers and are not used for determining the fit of the camera model. The residual errors should form a single circular cluster centered at (0, 0) and should appear randomly distributed. If two or more distinct clusters are present or the cluster is significantly non-circular; depending on the magnitude of the errors, there may be a problem with the calibration. Some possible causes are: bad vision detection of the calibration fiducial/nozzle tip, calibration fiducial/nozzle tip movement/slippage during the collection; camera or lens moving in its mount; under or over compensated backlash; motors missing steps; belt/cog slippage; etcetera.   

The **Residual Error Map** shows how the residual errors are distributed across the camera image. Dark blue areas have very low errors while dark red areas have the highest errors.  Note that the color range is always scaled so that zero error is the darkest blue and the maximum magnitude error is the darkest red. Therefore, this plot cannot be used to judge the magnitude of the error but only its distribution about the image. This distribution should look more or less random with no discernible patterns. If patterns such as rings or stripes are clearly visible and the residual errors observed in the other plots are larger than expected, the mathematical model of the camera does not fit very well with the physics of the camera and may indicate something is wrong with the camera and/or lens.

# Comparison of Uncalibrated to Calibrated Images
The animated gif below shows a piece of 2 millimeter graph paper face up on the machine table and imaged by the top camera. The camera's virtual Z axis was jogged to match the Z height of the graph paper and a Grid Reticle with 2 Units per Tick was selected. The gif alternates between uncorrected raw images and corrected images (note the state of the Apply Calibration checkbox):

![Before And After Calibration Image](https://user-images.githubusercontent.com/50550971/135157358-ad63e034-a3d5-4f7c-8605-800b5185cdc2.gif)
