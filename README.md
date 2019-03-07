custom version of Opera, running a browser framework written in JS, that uses
# Mazda-IVI-Knowledgebase
Knowledge base repo for anyone trying to reverse engineer the code on the IVI

Contained within is everything notable I've learned while reverse engineering 
the code on the IVI for my 2016 Mazda MX-5 Miata. This also likely applies to 
other mazdas as well, and I suspect most other IVIs that were created by 
Johnson Controls Inc (JCI)

# Hardware

## Board 
i.MX6Q from freescale.

### Radios
Texas Instruments WiFi and bluetooth combo chip. Both share the same antenna, 
so when using BT, (the JCI software hammers the BT with a lot of noise assuming 
there's no need to play nicely with wifi) WiFi may be laggy and buggy. A 
temporary fix would be to disable BT while using WiFi. I've noticed that having
a BT device connected makes wifi work "better".

#### BT
seems to work well with HCI, but not when the BlueGo library is running.

### Serial Connection
Available at two unpopulated pins on the back of the device @ 115200 
(TODO: image)

### Display Hardware

### CAN

### GPIO

### NVRAM

### Flash

### Audio
I'm still working out exactly how all this works, any help would be appreciated!


# Software

## Bootloader

## Linux Kernel
Version 3.0.35 (someone wanna see if they can get the build scripts from mazda?)

## Busybox
I hear the busybox people are willing to assist as well.

## Userland
Userland and kernel becasue the whole of the init system is proprietary, and I 
want an intuitive layout for this document.

### Init
Init (PID 1) is handled by some NIH init_cmu thing written by (I assume) JCI. 
It sets up the enviroment for the next child processes, and then at some point 
calls `/usr/bin/autostart`

`autostart` does things, then calls the JCI "System Manager" (sm) `/jci/sm/sm`. 
Which is what starts up all the following services

### Daemons
#### logging system

#### usb_authd

#### usb_authd

### Non-GUI Client apps
There's a long list of processes that are started up. Each one is a shared
object library that's called by the sm service launcher `sm_svclauncher`

TODO dependency tree

#### sm
this is the main system manager for the system. It starts up all the child 
processes, with a dependency tree written in XML.

#### dmserver
Device Manager Server (I think)

#### bteca
I think it's the Bluetooth Emergency Collision Agent. It looks like it handles
the emergency call to 911 if it detects a crash.

It also appears to use text to speech to call 911, and "talk" to the operator
giving them prompts using DTMF like, talking to the driver, etc.

#### Audio manager
there's various audio management scripts in lua across the system. I've had no
luck in reverse engineering how they work (I really haven't tried more than 10m
either)

### GUI
The User Interface itself is Wayland with a NIH IVI wrapper. Some headers for
the IVI have been reverse engineered, and are available in HUDTDS (TODO link). 
With a fullscreen custom version of Opera, running a browser framework written
in JS, that uses websockets to interface with the native binaries that do all 
the actual work.

#### Wayland
##### VIV compositor
##### IVI 
The version of the IVI seems to be a NIH version of the In Vehicle Infotainment
surface manager wayland/weston. None of the open source code from GENIVI matches
the interface I found on the car, but it appears to be similar in some respects.

#### Splashplayer
NIH code that diplays SOMETHING while everyone waits for a the web browesr to 
start up.

#### Opera
Seems to interfaces with wayland directly, controlling what's on screen.

#### JCI's NIH browser framework
Yes, my car runs Javascript... what do you mean that's stupid?! Javascript was 
obviously made for cars!

