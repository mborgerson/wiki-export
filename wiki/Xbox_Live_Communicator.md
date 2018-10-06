---
title: Xbox Live Communicator
permalink: wiki/Xbox_Live_Communicator/
layout: wiki
---

The Xbox Live Communicator is the headset which is used for Xbox Live.
<img src="Xbox_Live_Communicator.png" title="fig:Headset / Xbox Live Communicator" alt="Headset / Xbox Live Communicator" width="200" />

Protocol
--------

### USB Descriptor

    Bus 003 Device 006: ID 045e:0283 Microsoft Corp. Xbox Communicator
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            0 
      bDeviceSubClass         0 
      bDeviceProtocol         0 
      bMaxPacketSize0         8
      idVendor           0x045e Microsoft Corp.
      idProduct          0x0283 Xbox Communicator
      bcdDevice            1.58
      iManufacturer           0 
      iProduct                0 
      iSerial                 0 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           45
        bNumInterfaces          2
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
          bNumEndpoints           1
          bInterfaceClass       120 
          bInterfaceSubClass      0 
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 9
            bDescriptorType         5
            bEndpointAddress     0x04  EP 4 OUT
            bmAttributes            5
              Transfer Type            Isochronous
              Synch Type               Asynchronous
              Usage Type               Data
            wMaxPacketSize     0x0030  1x 48 bytes
            bInterval               1
            bRefresh                0
            bSynchAddress           0
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        1
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass       120 
          bInterfaceSubClass      0 
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 9
            bDescriptorType         5
            bEndpointAddress     0x85  EP 5 IN
            bmAttributes            5
              Transfer Type            Isochronous
              Synch Type               Asynchronous
              Usage Type               Data
            wMaxPacketSize     0x0030  1x 48 bytes
            bInterval               1
            bRefresh                0
            bSynchAddress           0
    can't get debug descriptor: Resource temporarily unavailable
    Device Status:     0x0000
      (Bus Powered)

### Microphone

### Speaker

#### Links

-   [Code for accessing the communicator microphone and
    speaker](https://github.com/JayFoxRox/xbox-tools/tree/4bc808e187311010f850d7fbd9af4b76bed90727/communicator-tool)

