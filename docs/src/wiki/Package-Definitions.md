A place to put community sourced package definitions. You can export a package definition from OpenPnP by clicking the "Copy" button in the Packages panel.

When adding a package here, please use the following format:

    ## SOT-23
    ```
    <package id="SOT-23">
       <outline units="Millimeters"/>
       <footprint units="Millimeters" body-width="2.9" body-height="1.3">
          <pad name="1" x="-1.0" y="-1.1" width="0.8" height="0.8" rotation="0.0" roundness="0.0"/>
          <pad name="2" x="1.0" y="-1.1" width="0.8" height="0.8" rotation="0.0" roundness="0.0"/>
          <pad name="3" x="0.0" y="1.1" width="0.8" height="0.8" rotation="0.0" roundness="0.0"/>
       </footprint>
    </package>
    ```

To use one of these packages in your system just copy the definition to your clipboard and hit the "Paste" button in the Packages panel.

# Packages
## Resistors 0201-1206:
```
<package version="1.1" id="R0201" description="Resistor R0201">
  <footprint units="Millimeters" body-width="0.6" body-height="0.3">
    <pad name="1" x="-0.3" y="0.0" width="0.3" height="0.3" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="0.3" y="0.0" width="0.3" height="0.3" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="R0402" description="Resistor R0402">
  <footprint units="Millimeters" body-width="1.0" body-height="0.5">
    <pad name="1" x="-0.5" y="0.0" width="0.5" height="0.6" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="0.5" y="0.0" width="0.5" height="0.6" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="R0603" description="Resistor R0603">
  <footprint units="Millimeters" body-width="1.55" body-height="0.85">
    <pad name="1" x="-0.75" y="0.0" width="0.6" height="0.9" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="0.75" y="0.0" width="0.6" height="0.9" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="R0805" description="Resistor R0805">
  <footprint units="Millimeters" body-width="2.0" body-height="1.2">
    <pad name="1" x="-0.95" y="0.0" width="0.7" height="1.3" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="0.95" y="0.0" width="0.7" height="1.3" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="R1206" description="Resistor R1206">
  <footprint units="Millimeters" body-width="3.2" body-height="1.6">
    <pad name="1" x="-1.45" y="0.0" width="0.9" height="1.6" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="1.45" y="0.0" width="0.9" height="1.6" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
```

## Capacitors 0402-1206, A-E:
```
<package version="1.1" id="C0402" description="Capacitor 0402">
  <footprint units="Millimeters" body-width="1.0" body-height="0.5">
    <pad name="1" x="-0.6" y="0.0" width="0.9" height="0.7" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="0.6" y="0.0" width="0.9" height="0.7" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="C0603" description="Capacitor 0603">
  <footprint units="Millimeters" body-width="1.6" body-height="0.8">
    <pad name="1" x="-0.9" y="0.0" width="1.0" height="1.0" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="0.9" y="0.0" width="1.0" height="1.0" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="C0805" description="Capacitor 0805">
  <footprint units="Millimeters" body-width="1.6" body-height="1.25">
    <pad name="1" x="-1.0" y="0.0" width="1.2" height="1.5" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="1.0" y="0.0" width="1.2" height="1.5" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="C1206" description="Capacitor 1206">
  <footprint units="Millimeters" body-width="3.2" body-height="1.6">
    <pad name="1" x="-1.45" y="0.0" width="1.5" height="1.8" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="1.45" y="0.0" width="1.5" height="1.8" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="CA" description="Capacitor A">
  <footprint units="Millimeters" body-width="3.2" body-height="1.6">
    <pad name="1" x="-1.45" y="0.0" width="1.5" height="1.8" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="1.45" y="0.0" width="1.5" height="1.8" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="CB" description="Capacitor B">
  <footprint units="Millimeters" body-width="3.5" body-height="2.8">
    <pad name="1" x="-1.6" y="0.0" width="1.5" height="2.4" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="1.6" y="0.0" width="1.5" height="2.4" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="CC" description="Capacitor C">
  <footprint units="Millimeters" body-width="6.0" body-height="3.2">
    <pad name="1" x="-2.7" y="0.0" width="2.6" height="2.8" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="2.7" y="0.0" width="2.6" height="2.8" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
<package version="1.1" id="CD CE" description="Capacitor CD CE">
  <footprint units="Millimeters" body-width="7.3" body-height="4.3">
    <pad name="1" x="-3.35" y="0.0" width="2.6" height="3.0" rotation="0.0" roundness="0.0"/>
    <pad name="2" x="3.35" y="0.0" width="2.6" height="3.0" rotation="0.0" roundness="0.0"/>
  </footprint>
</package>
```

## SOT-23
```
<package id="SOT-23">
   <outline units="Millimeters"/>
   <footprint units="Millimeters" body-width="2.9" body-height="1.3">
      <pad name="1" x="-1.0" y="-1.1" width="0.8" height="0.8" rotation="0.0" roundness="0.0"/>
      <pad name="2" x="1.0" y="-1.1" width="0.8" height="0.8" rotation="0.0" roundness="0.0"/>
      <pad name="3" x="0.0" y="1.1" width="0.8" height="0.8" rotation="0.0" roundness="0.0"/>
   </footprint>
</package>
```

## SOIC-8
```
<package version="1.1" id="SOIC-8" description="SOIC-8 IC" pick-vacuum-level="0.0" place-blow-off-level="0.0">
   <footprint units="Millimeters" body-width="3.0" body-height="1.1">
      <pad name="1" x="-2.1496" y="0.975" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="2" x="-2.1496" y="0.325" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="3" x="-2.1496" y="-0.325" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="4" x="-2.1496" y="-0.975" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="5" x="2.1496" y="-0.975" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="6" x="2.1496" y="-0.325" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="7" x="2.1496" y="0.325" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
      <pad name="8" x="2.1496" y="0.975" width="1.4709" height="0.4815" rotation="0.0" roundness="0.0"/>
   </footprint>
   <compatible-nozzle-tip-ids class="java.util.ArrayList"/>
</package>
```
## SOP-16
```
<package version="1.1" id="SOP-16" pick-vacuum-level="0.0" place-blow-off-level="0.0">
   <footprint units="Millimeters" body-width="4.0" body-height="10.0">
      <pad name="1" x="-2.5" y="4.445" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="2" x="-2.5" y="3.175" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="3" x="-2.5" y="1.905" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="4" x="-2.5" y="0.635" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="5" x="-2.5" y="-0.635" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="6" x="-2.5" y="-1.905" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="7" x="-2.5" y="-3.175" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="8" x="-2.5" y="-4.445" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="9" x="2.5" y="-4.445" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="10" x="2.5" y="-3.175" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="11" x="2.5" y="-1.905" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="12" x="2.5" y="-0.635" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="13" x="2.5" y="0.635" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="14" x="2.5" y="1.905" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="15" x="2.5" y="3.175" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
      <pad name="16" x="2.5" y="4.445" width="1.2" height="0.6" rotation="0.0" roundness="0.0"/>
   </footprint>
   <compatible-nozzle-tip-ids class="java.util.ArrayList"/>
</package>
```
## TSSOP-20
```
<package version="1.1" id="TSSOP-20" pick-vacuum-level="0.0" place-blow-off-level="0.0">
   <footprint units="Millimeters" body-width="4.4" body-height="6.9">
      <pad name="1" x="-2.75" y="2.925" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="2" x="-2.75" y="2.275" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="3" x="-2.75" y="1.625" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="4" x="-2.75" y="0.975" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="5" x="-2.75" y="0.325" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="6" x="-2.75" y="-0.325" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="7" x="-2.75" y="-0.975" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="8" x="-2.75" y="-1.625" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="9" x="-2.75" y="-2.275" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="10" x="-2.75" y="-2.925" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="11" x="2.75" y="-2.925" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="12" x="2.75" y="-2.275" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="13" x="2.75" y="-1.625" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="14" x="2.75" y="-0.975" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="15" x="2.75" y="-0.325" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="16" x="2.75" y="0.325" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="17" x="2.75" y="0.975" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="18" x="2.75" y="1.625" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="19" x="2.75" y="2.275" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
      <pad name="20" x="2.75" y="2.925" width="1.0" height="0.3" rotation="0.0" roundness="0.0"/>
   </footprint>
   <compatible-nozzle-tip-ids class="java.util.ArrayList"/>
</package>
```