# Native Driver (Obsolete)

**The information below pertains to the development of a native driver, which has turned out to be impossible. The information is obsolete but left here for reference.**

The stock firmware on the CHMT36VA turned out to be a dead end. There does not appear to be any command that can be used to rotate the nozzle. The best way forward seems to be to use [Matt Baker's Smoothie port](https://github.com/mattthebaker/Smoothieware-CHMT) which can be flashed on the stock main board.
see https://github.com/openpnp/openpnp/wiki/CharmHigh-CHMT36VA

# Source Code

See https://github.com/openpnp/openpnp/tree/feature/chmt36va/src/main/java/org/openpnp/machine/chmt36va for the latest source code.

# Testing

**WARNING** The code is currently in alpha testing. **IT MAY BREAK YOUR MACHINE**. Running the current code is only recommended for people with experience with OpenPnP and who are willing to potentially destroy your machine.

To be clear: **This code might permanently damage your machine, and your warranty is likely to be voided. I accept no responsibility for any damage you cause. Proceed at your own risk!**

To run the CHMT36VA driver in OpenPnP, follow these basic instructions:

1. Connect your CHMT36VA to your OpenPnP computer using the supplied USB cables. There are two - one for the camera, and one for the serial port.
2. Download and install the test branch of OpenPnP from http://openpnp.org/test-downloads/.
3. Start OpenPnP once, and then exit. This creates your .openpnp configuration directory. See https://github.com/openpnp/openpnp/wiki/FAQ#where-are-configuration-and-log-files-located to find the location. You'll need it for the next step.
4. Copy your CharmHigh supplied `LICENSE.smt` file to your configuration directory. Note, make sure to COPY and not move the file. Always keep a backup!
5. Start OpenPnP again and begin to work through the [[Setup and Calibration]] Guide. When you reach the point where you need to select a driver, choose the `CHMT36VADriver`. You will be prompted to restart.
6. Go to Machine Setup -> Driver -> CHMT36VADriver -> Communications.
7. Select your Port, Baud: 115200, Parity: None, Data Bits: Eight, Stop Bits: One and pretty Apply.
8. Click the green OpenPnP power button. The machine should reset and beep.
9. Click the Home button to home the machine.
10. Read the Warnings and Things to Try sections before doing anything else.

## Warnings

* Z and Rotation are not working yet.
* No machine extents or limits are enforced, you can easily crash the head of the machine and cause permanent damage. Be careful and cautious!
* Don't allow the drag pin to be extended for more than a few seconds at a time or it may overheat and be damaged.

## Things to Try

* Jog the machine in X/Y using the jog buttons.
* Add a [Top Camera](https://github.com/openpnp/openpnp/wiki/Setup-and-Calibration_Top-Camera-Setup) and perform camera calibration. Try camera jogging.
* Visit the Actuators panel and try the various Actuators. You can select different cameras, turn lights on and off, extend the drag pin, turn the pump on and off, turn on nozzle 1 and nozzle 2 vacuum, etc.

# Machine Overview

The CHMT36VA is a desktop pick and place machine. It has an internal controller but requires an external PC to run.

For feeders, the machine has 29 lanes of drag feeders: 22 8mm lanes, 4 12mm lanes, 2 16mm lanes, and 1 24mm lane, and a clutch driven cover tape peeler. There are also 14 single chip trays, and the software supports trays placed randomly around the machine.

There are two nozzles in the shared Z see-saw configuration that is now common on almost all of these types of machines. The nozzles each have a NEMA8 hollow shaft stepper and a permanently mounted nozzle tip holder. The nozzle tip holder takes standard Juki style nozzles, and they are retained by 4 ball bearings and a rubber band.

The vision system has an up looking and a down looking camera, both analog, at 640x480. As far as I can tell, the down looking camera is only used for manual positioning, it does not appear to be used for any type of vision operations. The up looking camera is used for the standard "bottom vision" operation.

The OEM software is Windows based, written with Qt, and is designed for a small touch screen interface. There are other CharmHigh desktop pick and places that use an on board tablet interface, and I suspect the same software runs on the tablet.

For vacuum and air, the machine has an internal vacuum pump and a small internal air compressor. The vacuum pump is used to pick components up, and the air compressor is used to evacuate the vacuum lines when the machine places a part.

## Axes

The X and Y axes are belt driven, with linear bearings on smooth rod. Power comes from NEMA23 steppers with optical encoders and the motion is closed loop. The steppers are powered at 36v and are quite fast. A drive shaft spreads the load of the Y axis to dual belts on either side, while the X axis has a single belt and is directly driven.

Homing is via two mechanical homing switches with rollers.

The Z axis uses a see-saw configuration with spring return. The motor is a NEMA11 (or 14?) and has an optical homing disk with a small slot in it. The disk interrupts the optical beam and the slot allows it through. The machine checks this whenever Z returns to center to ensure it's safe to move.

The C axes (nozzle rotation) are via NEMA 8 hollow shaft steppers with vacuum tubing connected directly to the back shaft. 

## Head

The head includes the two nozzles, a drag pin solenoid, vacuum sensors, vacuum solenoids, and a couple PCBs. The drag solenoid also has an optical interruptor style gate to ensure it's in the up position before moving. It appears to use a spring return made of a piece of rubber tubing, but I haven't been able to inspect it closely to be sure.

## Lighting

The gantry has a white LED strip across it's entire length that the software refers to as the "Work Light". This does a pretty good job of lighting the work area without creating intense reflections.

The up facing camera has a white LED ring light with 3mm LEDs bent at an angle.

## Cameras

There are two cameras, on up facing and one down facing. They are analog cameras and both feed into an off the shelf single capture card with a strange switching PCB soldered to the top of it. The result is that only one camera can send a signal to the host at a time. In general, the software keeps the up looking camera activated and only activates the down looking during user targeting.

The up looking camera appears to be a Sony Effio SN700, which is a cheap and common security camera. I have not yet determined what the down looking one is.

The capture card seems to be a clone of an EZCAP and is identified as "USB2.0 PC CAMERA" with VID 18EC, PID 5850.

# Communications

The machine has an RS-232 9 pin connector, and a USB B connector on the back. The RS-232 is used by the host to send and receive commands and status, and the USB B connector routes directly to the capture card.

## Protocol

The protocol is little endian binary over RS-232 serial. Communication is at 115200 baud, N81, no flow control. Most commands from the host result in a response from the machine, and the machine also sends some asynchronous responses.

The packet format is as follows:

Example packet: ebfbebfb110400ca0032002a0001012c00cbdbcbdb

| Offset     | Data       | Description                                               |
| ---------- | ---------- | --------------------------------------------------------- |
| 00         | ebfbebfb   | header                                                    |
| 04         | 11         | I call this packetType, but not completely sure on it yet |
| 05         | 0400       | payload size, 16 bit unsigned little endian               |
| 07         | ca         | encryption key / subkey info                              |
| 08         | 00         | unknown                                                   |
| 09         | 3200       | tableId, 16 bit unsigned little endian                    |
| 11         | 2a00 01 01 | payload (paramId, data type, field value)                 |
| length - 6 | 2c00       | crc16, 16 bit unsigned little endian                      |
| length - 4 | cbdbcbdb   | footer                                                    |

- All packets are encrypted using keys from the included LICENSE.smt file.
  - In LICENSE.smt, there is a 12 byte key at 1024, and a 1024 byte key at 4096.
  - The 12 byte key is "decrypted" by XOR 0xAA.
  - The 1024 byte key is encrypted by XORing with the decrypted 12 byte key with the index mod 12.
  - For details of the encryption see https://github.com/openpnp/openpnp/blob/feature/chmt36va/src/main/java/org/openpnp/machine/chmt36va/Protocol.java

- Commands are somewhat generic, and can be found in the software's smt.db sqlite3 database. See https://docs.google.com/spreadsheets/d/1mVowA6ZmbxwnvEy32Ap_YYBsWuUACl5wKZB5wjBqljY
