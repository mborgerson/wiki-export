---
title: Xbox Input Devices
permalink: wiki/Xbox_Input_Devices/
layout: wiki
---

XID Overview
------------

XIDs are USB devices.

The hardware side is USB with a different plug while the software side
is USB without HID-descriptors. Technicly a XID is a USB-hub for the
Memory-Units and the XBL Communicator. The logical XID gamepad USB
device is internally connected to that hub.

### USB Adapters

The Xbox input devices are USB devices. As such, you can connect a
keyboard to the Xbox, or a gamepad to your PC. In fact, Linux already
has drivers for the gamepad. In order to preserve Xbox hardware, please
do not cut OEM Xbox cables to make an adapter. Decent adapters can be
acquired cheaply (~$10 USD ea. on 2017.04.30).

| Port (From) | Plug (To) | Link                                                   |
|-------------|-----------|--------------------------------------------------------|
| Xbox        | USB-A     | [Amazon](https://www.amazon.com/gp/product/B000RT2868) |
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

| Field | Offset (Bytes) | Mask   | Notes                             |
|-------|----------------|--------|-----------------------------------|
|       | 2              | 0x01   |                                   |
|       | 2              | 0x02   |                                   |
|       | 2              | 0x04   |                                   |
|       | 2              | 0x08   |                                   |
|       | 2              | 0x10   |                                   |
|       | 2              | 0x20   |                                   |
|       | 2              | 0x40   |                                   |
|       | 2              | 0x80   |                                   |
|       | 4              | 0xFF   | Button is analog                  |
|       | 5              | 0xFF   | Button is analog                  |
|       | 6              | 0xFF   | Button is analog                  |
|       | 7              | 0xFF   | Button is analog                  |
|       | 8              | 0xFF   | Button is analog                  |
|       | 9              | 0xFF   | Button is analog                  |
|       | 10             | 0xFF   | Trigger is analog                 |
|       | 11             | 0xFF   | Trigger is analog                 |
|       | 12             | 0xFFFF | Negative = Left; Positive = Right |
|       | 14             | 0xFFFF | Negative = Down; Positive = Up    |
|       | 16             | 0xFFFF | Negative = Left; Positive = Right |
|       | 18             | 0xFFFF | Negative = Down; Positive = Up    |

### Xbox to Controller

6 bytes

| Field                   | Offset (Bytes) | Mask   | Notes |
|-------------------------|----------------|--------|-------|
| Left actuator strength  | 2              | 0xFFFF |       |
| Right actuator strength | 4              | 0xFFFF |       |

The Microsoft Controller S will not react to packets which don't have a
value of 6 in the `length` field of the header. The Fanatec Speedster 3
ForceShock will still react to those. Further testing is necessary with
other gamepads.

Steering wheels
---------------

### MadCatz Wheel

### Fanatec Speedster 3 ForceShock

#### Pedals

The Pedals are *not* a USB device.

Note that the cable going to the pedals is also *not* a USB port despite
using the Xbox controller breakaway plug. Likewise, plugging the pedals
to a PC / Xbox won't provide a USB / XID (it is detected as garbage):

    new full-speed USB device number 14 using xhci_hcd
    device descriptor read/64, error -71
    device descriptor read/64, error -71
    new full-speed USB device number 15 using xhci_hcd
    device descriptor read/64, error -71
    device descriptor read/64, error -71
    new full-speed USB device number 16 using xhci_hcd
    Device not responding to setup address.
    Device not responding to setup address.
    device not accepting address 16, error -71
    new full-speed USB device number 17 using xhci_hcd
    Device not responding to setup address.
    Device not responding to setup address.
    device not accepting address 17, error -71
    unable to enumerate USB device

#### Internal HUB

##### USB Descriptors

Power not connected, pedals not connected, not in Tuning mode:

    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            9 Hub
      bDeviceSubClass         0 
      bDeviceProtocol         0 Full speed (or root) hub
      bMaxPacketSize0         8
      idVendor           0x3767 
      idProduct          0x0102 
      bcdDevice            0.01
      iManufacturer           0 
      iProduct                1 End
      iSerial                 0 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           25
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0xa0
          (Bus Powered)
          Remote Wakeup
        MaxPower               64mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         9 Hub
          bInterfaceSubClass      0 
          bInterfaceProtocol      0 Full speed (or root) hub
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0001  1x 1 bytes
            bInterval             255
    Hub Descriptor:
      bLength               9
      bDescriptorType      41
      nNbrPorts             3
      wHubCharacteristic 0x000d
        Per-port power switching
        Compound device
        Per-port overcurrent protection
      bPwrOn2PwrGood       50 * 2 milli seconds
      bHubContrCurrent     64 milli Ampere
      DeviceRemovable    0x02
      PortPwrCtrlMask    0xff
     Hub Port Status:
       Port 1: 0000.0103 power enable connect
       Port 2: 0000.0100 power
       Port 3: 0000.0100 power
    can't get debug descriptor: Resource temporarily unavailable
    Device Status:     0x0003
      Self Powered
      Remote Wakeup Enabled

#### Steering wheel (and Pedals)

Always connected to port 1 of the internal HUB

##### USB Descriptors

    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            0 
      bDeviceSubClass         0 
      bDeviceProtocol         0 
      bMaxPacketSize0         8
      idVendor           0x3767 
      idProduct          0x0101 
      bcdDevice            2.80
      iManufacturer           0 
      iProduct                0 
      iSerial                 0 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           32
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0x80
          (Bus Powered)
        MaxPower              100mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        88 Xbox
          bInterfaceSubClass     66 Controller
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x82  EP 2 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0020  1x 32 bytes
            bInterval               4
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x02  EP 2 OUT
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0020  1x 32 bytes
            bInterval               4
    can't get debug descriptor: Resource temporarily unavailable
    Device Status:     0x0000
      (Bus Powered)

Light guns
----------

### EMS TopGun II

*This is an unlicensed / unofficial Xbox accessory.*

The website for this product can be found at
<http://www.hkems.com/product/xbox/EMSTopGun2.htm>

The gun presents itself as a standard Xbox gamepad. It uses a different
USB descriptor for Xbox (X) and the other mode (P). There is no internal
hub in this device.

| EMS TopGun II    | Xbox Gamepad | Notes                                        |
|------------------|--------------|----------------------------------------------|
| Stick            |              |                                              |
| Trigger          |              | Digital only, either 0 or 255                |
| Grip             |              |
| A                |              |
| B                |              |
| START            |              |                                              |
| SE/BA            |              |                                              |
| Aim Left / Right |              | Absolute position using the full stick range |
| Aim Up / Down    |              |

There is no right thumbstick, thumbstick presses, black/white button or
trigger buttons (All of those read constant zeros).

##### Turbo Mode

-   Turbo mode 0 keeps pressed while trigger is held
-   Turbo mode 1 toggles rapidly while trigger is held
-   Turbo mode 2 toggles rapidly and once in a while while trigger is
    held

##### Force Feedback

The upper part of the gun is moveable and should push back to simulate
recoil (possibly hurting your thumb while you are using the stick). I
could not get the force feedback working, but I'm sure I've had it
working in the past on PC.

#### USB Descriptors

This is the descriptor in the Xbox mode (X).

    Bus 003 Device 016: ID 0b9a:016b  
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            0 
      bDeviceSubClass         0 
      bDeviceProtocol         0 
      bMaxPacketSize0        64
      idVendor           0x0b9a 
      idProduct          0x016b 
      bcdDevice            4.57
      iManufacturer           1 EMS̖E
      iProduct                2 EMS TopGun
      iSerial                 0 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           32
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0x80
          (Bus Powered)
        MaxPower              100mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        88 Xbox
          bInterfaceSubClass     66 Controller
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0040  1x 64 bytes
            bInterval               4
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x02  EP 2 OUT
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0040  1x 64 bytes
            bInterval               8
    can't get debug descriptor: Resource temporarily unavailable
    Device Status:     0x0000
      (Bus Powered)

### Joytech Sharp Shooter

*This is an unlicensed / unofficial Xbox accessory.*

The third party light gun from Joytech reports itself as 2 devices and
mentions pattent [US6287198](http://www.google.com/patents/US6287198) it
came with a detachable viewfinder scope without any magnification. a red
dot apears in the viewfinder, its a reflection of a red led, powered by
the gun over usb.

model numer: JS-901D

| Joytech Sharp Shooter | Xbox Gamepad | Notes                         |
|-----------------------|--------------|-------------------------------|
| Stick                 |              |                               |
| Trigger               |              | Digital only, either 0 or 255 |
| B (Left side)         |              |
| B (Right side)        |
| B (Magazine button)   |
| x                     |              |
| y                     |              |
| START                 |              |                               |
| BACK                  |              |                               |
| Aim Left / Right      |              |                               |
| Aim Up / Down         |              |                               |

There is no right thumbstick, thumbstick presses, black/white button or
trigger buttons

##### Fire/Reload Mode

-   Normal mode does nothing, normal operation
-   Auto reload mode toggles rapidly to rappidly reload
-   Auto fire+reload mode toggles  + rapidly

#### USB Descriptors


    Bus 003 Device 025: ID 1292:3006 Innomedia 
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            0 
      bDeviceSubClass         0 
      bDeviceProtocol         0 
      bMaxPacketSize0         8
      idVendor           0x1292 Innomedia
      idProduct          0x3006 
      bcdDevice            1.50
      iManufacturer           0 
      iProduct                0 
      iSerial                 0 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           32
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          1 (error)
        bmAttributes         0x80
          (Bus Powered)
        MaxPower              100mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        88 Xbox
          bInterfaceSubClass     66 Controller
          bInterfaceProtocol      0 
          iInterface              2 (error)
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0020  1x 32 bytes
            bInterval               4
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x02  EP 2 OUT
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0020  1x 32 bytes
            bInterval               4

    Bus 003 Device 024: ID 1292:3006 Innomedia 
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            9 Hub
      bDeviceSubClass         0 
      bDeviceProtocol         0 Full speed (or root) hub
      bMaxPacketSize0         8
      idVendor           0x1292 Innomedia
      idProduct          0x3006 
      bcdDevice            1.50
      iManufacturer           1 (c) 2004 R0R3 Inc.
      iProduct                2 US Patent 6,287,198
      iSerial                 0 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           25
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          4 (c) R0R3 Devices Inc. US Patent 6,287,19Ē
        bmAttributes         0xa0
          (Bus Powered)
          Remote Wakeup
        MaxPower               64mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         9 Hub
          bInterfaceSubClass      0 
          bInterfaceProtocol      0 Full speed (or root) hub
          iInterface              5 (error)
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0001  1x 1 bytes
            bInterval             255
    Hub Descriptor:
      bLength               9
      bDescriptorType      41
      nNbrPorts             3
      wHubCharacteristic 0x000d
        Per-port power switching
        Compound device
        Per-port overcurrent protection
      bPwrOn2PwrGood       32 * 2 milli seconds
      bHubContrCurrent     64 milli Ampere
      DeviceRemovable    0x02
      PortPwrCtrlMask    0x0e
     Hub Port Status:
       Port 1: 0000.0103 power enable connect
       Port 2: 0000.0100 power
       Port 3: 0100.0100 power
    Device Status:     0x0000
      (Bus Powered)

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

From
<http://steelbattalionnet.codeplex.com/SourceControl/latest#SBC/SteelBattalionController.cs>

26 bytes

| Field                   | Offset (Bytes) | Mask                             | Notes                                                                                                                                           |
|-------------------------|----------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| RightJoyMainWeapon      | 2              | 0x01                             |                                                                                                                                                 |
| RightJoyFire            | 2              | 0x03                             | FIXME: WTF?! Mask might be bad?                                                                                                                 |
| RightJoyLockOn          | 2              | 0x04                             |                                                                                                                                                 |
| Eject                   | 2              | 0x08                             |                                                                                                                                                 |
| CockpitHatch            | 2              | 0x10                             |                                                                                                                                                 |
| Ignition                | 2              | 0x20                             |                                                                                                                                                 |
| Start                   | 2              | 0x40                             |                                                                                                                                                 |
| MultiMonOpenClose       | 2              | 0x80                             |                                                                                                                                                 |
| MultiMonMapZoomInOut    | 3              | 0x01                             |                                                                                                                                                 |
| MultiMonModeSelect      | 3              | 0x02                             |                                                                                                                                                 |
| MultiMonSubMonitor      | 3              | 0x04                             |                                                                                                                                                 |
| MainMonZoomIn           | 3              | 0x08                             |                                                                                                                                                 |
| MainMonZoomOut          | 3              | 0x10                             |                                                                                                                                                 |
| FunctionFSS             | 3              | 0x20                             |                                                                                                                                                 |
| FunctionManipulator     | 3              | 0x40                             |                                                                                                                                                 |
| FunctionLineColorChange | 3              | 0x80                             |                                                                                                                                                 |
| Washing                 | 4              | 0x01                             |                                                                                                                                                 |
| Extinguisher            | 4              | 0x02                             |                                                                                                                                                 |
| Chaff                   | 4              | 0x04                             |                                                                                                                                                 |
| FunctionTankDetach      | 4              | 0x08                             |                                                                                                                                                 |
| FunctionOverride        | 4              | 0x10                             |                                                                                                                                                 |
| FunctionNightScope      | 4              | 0x20                             |                                                                                                                                                 |
| FunctionF1              | 4              | 0x40                             |                                                                                                                                                 |
| FunctionF2              | 4              | 0x80                             |                                                                                                                                                 |
| FunctionF3              | 5              | 0x01                             |                                                                                                                                                 |
| WeaponConMain           | 5              | 0x02                             |                                                                                                                                                 |
| WeaponConSub            | 5              | 0x04                             |                                                                                                                                                 |
| WeaponConMagazine       | 5              | 0x08                             |                                                                                                                                                 |
| Comm1                   | 5              | 0x10                             |                                                                                                                                                 |
| Comm2                   | 5              | 0x20                             |                                                                                                                                                 |
| Comm3                   | 5              | 0x40                             |                                                                                                                                                 |
| Comm4                   | 5              | 0x80                             |                                                                                                                                                 |
| Comm5                   | 6              | 0x01                             |                                                                                                                                                 |
| LeftJoySightChange      | 6              | 0x02                             |                                                                                                                                                 |
| ToggleFilterControl     | 6              | 0x04                             |                                                                                                                                                 |
| ToggleOxygenSupply      | 6              | 0x08                             |                                                                                                                                                 |
| ToggleFuelFlowRate      | 6              | 0x10                             |                                                                                                                                                 |
| ToggleBuffreMaterial    | 6              | 0x20                             |                                                                                                                                                 |
| ToggleVTLocation        | 6              | 0x40                             |                                                                                                                                                 |
|                         | 6              | 0x80                             |                                                                                                                                                 |
|                         | 7              | 0xFF                             |                                                                                                                                                 |
|                         | 8              | 0xFF                             |                                                                                                                                                 |
| AimingX                 | 9              | 0xFF, maybe 0xFFFF at offset 8?  | “Aiming Lever” joystick on the right. X Axis value.                                                                                             |
| AimingY                 | 11             | 0xFF, maybe 0xFFFF at offset 10? | “Aiming Lever” joystick on the right. Y Axis value.                                                                                             |
| RotationLever           | 13             | 0xFF, maybe 0xFFFF at offset 12? | “Rotation Lever” joystick on the left.                                                                                                          |
| SightChangeX            | 15             | 0xFF, maybe 0xFFFF at offset 14? | “Sight Change” analog stick on the “Rotation Lever” joystick. X Axis value.                                                                     |
| SightChangeY            | 17             | 0xFF, maybe 0xFFFF at offset 16? | “Sight Change” analog stick on the “Rotation Lever” joystick. Y Axis value.                                                                     |
| LeftPedal               | 19             | 0xFF, maybe 0xFFFF at offset 18? |                                                                                                                                                 |
| MiddlePedal             | 21             | 0xFF, maybe 0xFFFF at offset 20? |                                                                                                                                                 |
| RightPedal              | 23             | 0xFF, maybe 0xFFFF at offset 22? |                                                                                                                                                 |
| TunerDial               | 24             | 0x0F                             | The 9 o'clock postion is 0, and the 6 o'clock position is 12. The blank area between the 6 and 9 o'clock positions is 13, 14, and 15 clockwise. |
|                         | 24             | 0xF0                             |                                                                                                                                                 |
| GearLever               | 25             | 0xFF                             | The gear lever on the left block.                                                                                                               |

### Xbox to Controller

From
<http://steelbattalionnet.codeplex.com/SourceControl/latest#SBC/SteelBattalionController.cs>

34 bytes

| Field                  | Offset (Bytes) | Mask | Notes |
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

Related links
-------------

[XID emulation in
XQEMU](https://github.com/xqemu/xqemu/blob/xbox/hw/xbox/xid.c)
