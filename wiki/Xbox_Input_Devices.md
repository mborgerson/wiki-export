---
title: Xbox Input Devices
permalink: wiki/Xbox_Input_Devices/
layout: wiki
---

XID Overview
------------

XIDs are USB devices.

### USB Adapters

The Xbox input devices are USB devices. As such, you can connect a
keyboard to the Xbox, or a gamepad to your PC. In fact, Linux already
has drivers for the gamepad. In order to preserve Xbox hardware, please
do not cut OEM Xbox cables to make an adapter. Decent adapters can be
acquired cheaply (~$10 USD ea. on 2017.04.30).

| Port (From) | Plug (To) | Link                                                   |
|-------------|-----------|--------------------------------------------------------|
| Xbox        | USB-A     | [Amazon](https://www.amazon.com/gp/product/B0169BOTDC) |
| USB-A       | Xbox      | [Amazon](https://www.amazon.com/gp/product/B00F52LQHO) |

### Wiring

Untested / unverified! Take this with a grain of salt.

| Pin | Typical cable color | Description                                     |
|-----|---------------------|-------------------------------------------------|
| 1   | Green               | USB D-                                          |
| 2   | White               | USB D+                                          |
| 3   | Black               | GND                                             |
| 4   | Red                 | VCC                                             |
| 5   | Yellow              | VBlank signal from video output (for Lightguns) |
||

### Protocol

#### Controller to Xbox

    typedef struct XIDGamepadReport {
        uint8_t bReportId;
        uint8_t bLength;
        <Data>
    }

#### Xbox to Controller

    typedef struct XIDGamepadOutputReport {
        uint8_t report_id; //FIXME: is this correct?
        uint8_t length;
        <Data>
    }

Standard Gamepads
-----------------

### USB Descriptors

Most standard gamepads share the same USB descriptor. Usually the only
difference will be the VID / PID. See
<https://github.com/xboxdrv/xboxdrv/blob/stable/src/xpad_device.cpp> for
a list of devices.

### Controller to Xbox

20 bytes

| Field          | Offset (Bytes) | Mask         | Notes             |
|----------------|----------------|--------------|-------------------|
| DPAD\_UP       | 2              | 1 &lt;&lt; 0 |                   |
| DPAD\_DOWN     | 2              | 1 &lt;&lt; 1 |                   |
| DPAD\_LEFT     | 2              | 1 &lt;&lt; 2 |                   |
| DPAD\_RIGHT    | 2              | 1 &lt;&lt; 3 |                   |
| START          | 2              | 1 &lt;&lt; 4 |                   |
| BACK           | 2              | 1 &lt;&lt; 5 |                   |
| LEFT\_THUMB    | 2              | 1 &lt;&lt; 6 |                   |
| RIGHT\_THUMB   | 2              | 1 &lt;&lt; 7 |                   |
| A              | 4              | 0xFF         | Button is analog  |
| B              | 5              | 0xFF         | Button is analog  |
| X              | 6              | 0xFF         | Button is analog  |
| Y              | 7              | 0xFF         | Button is analog  |
| BLACK          | 8              | 0xFF         | Button is analog  |
| WHITE          | 9              | 0xFF         | Button is analog  |
| LEFT\_TRIGGER  | 10             | 0xFF         | Trigger is analog |
| RIGHT\_TRIGGER | 11             | 0xFF         | Trigger is analog |
| sThumbLX       | 12             | 0xFFFF       |                   |
| sThumbLY       | 14             | 0xFFFF       |                   |
| sThumbRX       | 16             | 0xFFFF       |                   |
| sThumbRY       | 18             | 0xFFFF       |                   |

### Xbox to Controller

6 bytes

| Field                   | Offset (Bytes) | Mask   | Notes |
|-------------------------|----------------|--------|-------|
| Left actuator strength  | 2              | 0xFFFF |       |
| Right actuator strength | 4              | 0xFFFF |       |

Steering wheels
---------------

Light guns
----------

Steel Battalion Controller
--------------------------

### USB Descriptors

From
<http://www.yaronet.com/topics/154490-steel-battalion-controller-homemade#post-15>

    bcdUSB:             0x0110
    bDeviceClass:         0x00
    bDeviceSubClass:      0x00
    bDeviceProtocol:      0x00
    bMaxPacketSize0:      0x08 (8)
    idVendor:           0x0A7B
    idProduct:          0xD000
    bcdDevice:          0x0100
    iManufacturer:        0x00
    iProduct:             0x00
    iSerialNumber:        0x00
    bNumConfigurations:   0x01

    ConnectionStatus: DeviceConnected
    Current Config Value: 0x00
    Device Bus Speed:     Full
    Device Address:       0x03
    Open Pipes:              0

    Configuration Descriptor:
    wTotalLength:       0x0020
    bNumInterfaces:       0x01
    bConfigurationValue:  0x01
    iConfiguration:       0x00
    bmAttributes:         0x80 (Bus Powered )
    MaxPower:             0xFA (500 Ma)

    Interface Descriptor:
    bInterfaceNumber:     0x00
    bAlternateSetting:    0x00
    bNumEndpoints:        0x02
    bInterfaceClass:      0x58
    bInterfaceSubClass:   0x42
    bInterfaceProtocol:   0x00
    iInterface:           0x00

    Endpoint Descriptor:
    bEndpointAddress:     0x82
    Transfer Type:   Interrupt

### Controller to Xbox

26 bytes (208 bits)

| Field         | Bits                       | Notes                                                                                                                                                                                   |
|---------------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AimingX       | 72-79 (Maybe 72-87 ?!)     | Corresponds to the "Aiming Lever" joystick on the right. X Axis value.                                                                                                                  |
| AimingY       | 88-95 (Maybe 88-103 ?!)    | Corresponds to the "Aiming Lever" joystick on the right. Y Axis value.                                                                                                                  |
| RotationLever | 104-111 (Maybe 104-119 ?!) | Corresponds to the "Rotation Lever" joystick on the left.                                                                                                                               |
| SightChangeX  | 120-127 (Maybe 120-135 ?!) | Corresponds to the "Sight Change" analog stick on the "Rotation Lever" joystick. X Axis value.                                                                                          |
| SightChangeY  | 136-143 (Maybe 136-151 ?!) | Corresponds to the "Sight Change" analog stick on the "Rotation Lever" joystick. Y Axis value.                                                                                          |
| LeftPedal     | 152-159 (Maybe 152-167 ?!) | Corresponds to the left pedal on the pedal block                                                                                                                                        |
| MiddlePedal   | 168-175 (Maybe 168-192 ?!) | Corresponds to the middle pedal on the pedal block                                                                                                                                      |
| RightPedal    | 184-191                    | Corresponds to the right pedal on the pedal block                                                                                                                                       |
|               | 192-195                    |                                                                                                                                                                                         |
| TunerDial     | 196-199                    | Corresponds to the tuner dial position. The 9 o'clock postion is 0, and the 6 o'clock position is 12. The blank area between the 6 and 9 o'clock positions is 13, 14, and 15 clockwise. |
| GearLever     | 200-207                    | Corresponds to the gear lever on the left block.                                                                                                                                        |

    // Based on http://steelbattalionnet.codeplex.com/SourceControl/latest#SBC/SteelBattalionController.cs
    // See http://steelbattalionnet.codeplex.com/SourceControl/latest#SBC/lgpl-3.0.txt

      RightJoyMainWeapon       = BUTTON_MASK( 2, 0x01),
      RightJoyFire             = BUTTON_MASK( 2, 0x03), //FIXME: WTF?!
      RightJoyLockOn           = BUTTON_MASK( 2, 0x04),
      CockpitHatch             = BUTTON_MASK( 2, 0x10),
      Ignition                 = BUTTON_MASK( 2, 0x20),
      Start                    = BUTTON_MASK( 2, 0x40),
      Eject                    = BUTTON_MASK( 2, 0x08),
      MultiMonOpenClose        = BUTTON_MASK( 2, 0x80),
      MultiMonMapZoomInOut     = BUTTON_MASK( 3, 0x01),
      MultiMonModeSelect       = BUTTON_MASK( 3, 0x02),
      MultiMonSubMonitor       = BUTTON_MASK( 3, 0x04),
      MainMonZoomIn            = BUTTON_MASK( 3, 0x08),
      MainMonZoomOut           = BUTTON_MASK( 3, 0x10),
      FunctionFSS              = BUTTON_MASK( 3, 0x20),
      FunctionManipulator      = BUTTON_MASK( 3, 0x40),
      FunctionLineColorChange  = BUTTON_MASK( 3, 0x80),
      Washing                  = BUTTON_MASK( 4, 0x01),
      Extinguisher             = BUTTON_MASK( 4, 0x02),
      Chaff                    = BUTTON_MASK( 4, 0x04),
      FunctionTankDetach       = BUTTON_MASK( 4, 0x08),
      FunctionOverride         = BUTTON_MASK( 4, 0x10),
      FunctionNightScope       = BUTTON_MASK( 4, 0x20),
      FunctionF1               = BUTTON_MASK( 4, 0x40),
      FunctionF2               = BUTTON_MASK( 4, 0x80),
      FunctionF3               = BUTTON_MASK( 5, 0x01),
      WeaponConMain            = BUTTON_MASK( 5, 0x02),
      WeaponConSub             = BUTTON_MASK( 5, 0x04),
      WeaponConMagazine        = BUTTON_MASK( 5, 0x08),
      Comm1                    = BUTTON_MASK( 5, 0x10),
      Comm2                    = BUTTON_MASK( 5, 0x20),
      Comm3                    = BUTTON_MASK( 5, 0x40),
      Comm4                    = BUTTON_MASK( 5, 0x80),
      Comm5                    = BUTTON_MASK( 6, 0x01),
      LeftJoySightChange       = BUTTON_MASK( 6, 0x02),
      ToggleFilterControl      = BUTTON_MASK( 6, 0x04),
      ToggleOxygenSupply       = BUTTON_MASK( 6, 0x08),
      ToggleFuelFlowRate       = BUTTON_MASK( 6, 0x10),
      ToggleBuffreMaterial     = BUTTON_MASK( 6, 0x20),
      ToggleVTLocation         = BUTTON_MASK( 6, 0x40),

### Xbox to Controller

34 bytes

| Field                  | Offset (bytes) | Mask | Notes |
|------------------------|----------------|------|-------|
| EmergencyEject         | 2              | 0x0F |       |
| CockpitHatch           | 2              | 0xF0 |       |
| Ignition               | 3              | 0x0F |       |
| Start                  | 3              | 0xF0 |       |
| OpenClose              | 4              | 0x0F |       |
| MapZoomInOut           | 4              | 0xF0 |       |
| ModeSelect             | 5              | 0x0F |       |
| SubMonitorModeSelect   | 5              | 0xF0 |       |
| MainMonitorZoomIn      | 6              | 0x0F |       |
| MainMonitorZoomOut     | 6              | 0xF0 |       |
| ForecastShootingSystem | 7              | 0x0F |       |
| Manipulator            | 7              | 0xF0 |       |
| LineColorChange        | 8              | 0x0F |       |
| Washing                | 8              | 0xF0 |       |
| Extinguisher           | 9              | 0x0F |       |
| Chaff                  | 9              | 0xF0 |       |
| TankDetach             | 10             | 0x0F |       |
| Override               | 10             | 0xF0 |       |
| NightScope             | 11             | 0x0F |       |
| F1                     | 11             | 0xF0 |       |
| F2                     | 12             | 0x0F |       |
| F3                     | 12             | 0xF0 |       |
| MainWeaponControl      | 13             | 0x0F |       |
| SubWeaponControl       | 13             | 0xF0 |       |
| MagazineChange         | 14             | 0x0F |       |
| Comm1                  | 14             | 0xF0 |       |
| Comm2                  | 15             | 0x0F |       |
| Comm3                  | 15             | 0xF0 |       |
| Comm4                  | 16             | 0x0F |       |
| Comm5                  | 16             | 0xF0 |       |
|                        | 17             | 0x0F |       |
| GearR                  | 17             | 0xF0 |       |
| GearN                  | 18             | 0x0F |       |
| Gear1                  | 18             | 0xF0 |       |
| Gear2                  | 19             | 0x0F |       |
| Gear3                  | 19             | 0xF0 |       |
| Gear4                  | 20             | 0x0F |       |
| Gear5                  | 20             | 0xF0 |       |


