## Purpose

The upward-facing camera is used to improve the accuracy of the placement by measuring the offset of the component on the nozzle and then applying that delta to the placement operation.

## Why not the default?

The default pipeline is reliant upon the disk above the part being colored green as it uses 'green-screen / chroma-key' techniques to reduce noise.  My nozzle and disk are black so my pipeline relies upon just the brightness of the object in the frame.

## Code

    <cv-stage class="org.openpnp.vision.pipeline.stages.ImageCapture" name="0" enabled="true" settle-first="true"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.ImageWriteDebug" name="13" enabled="true" prefix="bv_source_" suffix=".png"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.BlurGaussian" name="10" enabled="true" kernel-size="9"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.MaskCircle" name="4" enabled="true" diameter="300"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.Threshold" name="16" enabled="true" threshold="200" auto="false" invert="false"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.ConvertColor" name="3" enabled="true" conversion="Hsv2BgrFull"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.ConvertColor" name="6" enabled="true" conversion="Bgr2Gray"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.FindContours" name="5" enabled="true" retrieval-mode="List" approximation-method="None"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.FilterContours" name="9" enabled="true" contours-stage-name="5" min-area="50.0" max-area="900000.0"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.MaskCircle" name="11" enabled="true" diameter="0"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.DrawContours" name="7" enabled="true" contours-stage-name="9" thickness="2" index="-1"><color r="255" g="255" b="255" a="255"/></cv-stage>
    <cv-stage class="org.openpnp.vision.pipeline.stages.MinAreaRect" name="result" enabled="true" threshold-min="100" threshold-max="255"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.ImageRecall" name="14" enabled="true" image-stage-name="0"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.DrawRotatedRects" name="8" enabled="true" rotated-rects-stage-name="result" thickness="2"/>
    <cv-stage class="org.openpnp.vision.pipeline.stages.ImageWriteDebug" name="15" enabled="true" prefix="bv_result_" suffix=".png"/>

## Example Images from Pipeline
![1206 LED](https://raw.githubusercontent.com/redvers/opencvimages/master/bv_result_5027480662176760365.png)
![1206 Cap](https://raw.githubusercontent.com/redvers/opencvimages/master/bv_result_876226607992064061.png)
![1206 YC164](https://raw.githubusercontent.com/redvers/opencvimages/master/bv_result_5347306999264825735.png)
