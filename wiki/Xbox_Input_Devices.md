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

    DPAD_UP           = FIELD_MASK(2, 1 << 0)
    DPAD_DOWN         = FIELD_MASK(2, 1 << 1)
    DPAD_LEFT         = FIELD_MASK(2, 1 << 2)
    DPAD_RIGHT        = FIELD_MASK(2, 1 << 3)
    START             = FIELD_MASK(2, 1 << 4)
    BACK              = FIELD_MASK(2, 1 << 5)
    LEFT_THUMB        = FIELD_MASK(2, 1 << 6)
    RIGHT_THUMB       = FIELD_MASK(2, 1 << 7)
    A                 = FIELD_MASK(4, 0xFF), // These buttons are analog!
    B                 = FIELD_MASK(5, 0xFF),
    X                 = FIELD_MASK(6, 0xFF),
    Y                 = FIELD_MASK(7, 0xFF),
    BLACK             = FIELD_MASK(8, 0xFF),
    WHITE             = FIELD_MASK(9, 0xFF),
    LEFT_TRIGGER      = FIELD_MASK(10, 0xFF),
    RIGHT_TRIGGER     = FIELD_MASK(11, 0xFF),
    sThumbLX          = FIELD_MASK(12, 0xFFFF),
    sThumbLY          = FIELD_MASK(14, 0xFFFF),
    sThumbRX          = FIELD_MASK(16, 0xFFFF),
    sThumbRY          = FIELD_MASK(18, 0xFFFF),

### Xbox to Controller

6 bytes

    left_actuator_strength  = FIELD_MASK(2, 0xFFFF),
    right_actuator_strength = FIELD_MASK(4, 0xFFFF),

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

    // Based on http://steelbattalionnet.codeplex.com/SourceControl/latest#SBC/SteelBattalionController.cs
    // See http://steelbattalionnet.codeplex.com/SourceControl/latest#SBC/lgpl-3.0.txt

    rawControlData = new Byte[26];

    typedef enum {

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

      AimingX                  = BUTTON_MASK( 9, 0xFF),  // Corresponds to the &quot;Aiming Lever&quot; joystick on the right.  X Axis value.

      AimingY                  = BUTTON_MASK(11, 0xFF), // Corresponds to the &quot;Aiming Lever&quot; joystick on the right.  Y Axis value.

      RotationLever            = BUTTON_MASK(13, 0xFF), // Corresponds to the &quot;Rotation Lever&quot; joystick on the left.

      SightChangeX             = BUTTON_MASK(15, 0xFF), // Corresponds to the &quot;Sight Change&quot; analog stick on the &quot;Rotation Lever&quot; joystick.  X Axis value.

      SightChangeY             = BUTTON_MASK(17, 0xFF) // Corresponds to the &quot;Sight Change&quot; analog stick on the &quot;Rotation Lever&quot; joystick.  Y Axis value.

      LeftPedal                = BUTTON_MASK(19, 0xFF), // Corresponds to the left pedal on the pedal block

      MiddlePedal              = BUTTON_MASK(21, 0xFF), // Corresponds to the middle pedal on the pedal block

      RightPedal               = BUTTON_MASK(23, 0xFF), // Corresponds to the right pedal on the pedal block
      TunerDial                = BUTTON_MASK(24, 0x0F), // Corresponds to the tuner dial position.  The 9 o'clock postion is 0, and the 6 o'clock position is 12.
                                                        // The blank area between the 6 and 9 o'clock positions is 13, 14, and 15 clockwise.
      GearLever                = BUTTON_MASK(25, 0xFF), // Corresponds to the gear lever on the left block.
    } ButtonEnum;

### Xbox to Controller

    rawLEDData = new Byte[34];

    typedef enum {

      EmergencyEject            = FIELD_MASK(2, 0x0F),
      CockpitHatch              = FIELD_MASK(2, 0xF0),
      Ignition                  = FIELD_MASK(3, 0x0F),
      Start                     = FIELD_MASK(3, 0xF0),
      OpenClose                 = FIELD_MASK(4, 0x0F),
      MapZoomInOut              = FIELD_MASK(4, 0xF0),
      ModeSelect                = FIELD_MASK(5, 0x0F),
      SubMonitorModeSelect      = FIELD_MASK(5, 0xF0),
      MainMonitorZoomIn         = FIELD_MASK(6, 0x0F),
      MainMonitorZoomOut        = FIELD_MASK(6, 0xF0),
      ForecastShootingSystem    = FIELD_MASK(7, 0x0F),
      Manipulator               = FIELD_MASK(7, 0xF0),
      LineColorChange           = FIELD_MASK(8, 0x0F),
      Washing                   = FIELD_MASK(8, 0xF0),
      Extinguisher              = FIELD_MASK(9, 0x0F),
      Chaff                     = FIELD_MASK(9, 0xF0),
      TankDetach                = FIELD_MASK(10, 0x0F),
      Override                  = FIELD_MASK(10, 0xF0),
      NightScope                = FIELD_MASK(11, 0x0F),
      F1                        = FIELD_MASK(11, 0xF0),
      F2                        = FIELD_MASK(12, 0x0F),
      F3                        = FIELD_MASK(12, 0xF0),
      MainWeaponControl         = FIELD_MASK(13, 0x0F),
      SubWeaponControl          = FIELD_MASK(13, 0xF0),
      MagazineChange            = FIELD_MASK(14, 0x0F),
      Comm1                     = FIELD_MASK(14, 0xF0),
      Comm2                     = FIELD_MASK(15, 0x0F),
      Comm3                     = FIELD_MASK(15, 0xF0),
      Comm4                     = FIELD_MASK(16, 0x0F),
      Comm5                     = FIELD_MASK(16, 0xF0),

      GearR                     = FIELD_MASK(17, 0xF0),
      GearN                     = FIELD_MASK(18, 0x0F),
      Gear1                     = FIELD_MASK(18, 0xF0),
      Gear2                     = FIELD_MASK(19, 0x0F),
      Gear3                     = FIELD_MASK(19, 0xF0),
      Gear4                     = FIELD_MASK(20, 0x0F),
      Gear5                     = FIELD_MASK(20, 0xF0),

    } LEDEnum;
