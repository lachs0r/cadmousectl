Configuration Tool for 3Dconnexion CadMouse
-------------------------------------------

Since 3Dconnexion does not have any open-source tool to change firmware
settings for their CadMouse device, I have reverse engineered its configuration
protocol by looking at USB captures from a Windows VM running its settings tool.

Compilation
===========

This needs libudev.

``cc cadmousectl.c -o cadmousectl $(pkg-config --cflags --libs libudev)``

Usage
=====

This tool needs write access to the USB device, so either run as root or set
up access through udev rules. The mouse loses its settings each time it is
disconnected, so you probably want to set up udev rules to run this tool
anyway.

The file ``99-cadmouse.rules`` contains an example udev rule that
automatically runs cadmousectl whenever a CadMouse is plugged in.

cadmousectl [-[lprsS] value]

+--------+---------------------------------------------------------+
| Option | Effect                                                  |
+========+=========================================================+
| -l     | Enable (non-zero) or disable (zero) lift-off detection. |
+--------+---------------------------------------------------------+
| -p     | Set polling rate (125, 250, 500, 1000).                 |
+--------+---------------------------------------------------------+
| -r     | Remap buttons. Format is real_button:assigned_button.   |
|        |                                                         |
|        | Values for real_button:                                 |
|        |     left, right, middle, wheel, forward, backward, rm   |
|        |                                                         |
|        | Values for assigned_button:                             |
|        |     left, right, middle, backward, forward, rm, extra   |
|        |                                                         |
|        | .. note::                                               |
|        |     The extra button was discovered by accident.        |
|        |     Using this, you can assign an additional button to  |
|        |     the wheel click. It will have id 11 on X11.         |
+--------+---------------------------------------------------------+
| -s     | Set speed (1-164).                                      |
|        | Cannot be used with -d                                  |
+--------+---------------------------------------------------------+
| -d     | Set speed in DPI (50-8200).                             |
|        | Cannot be used with -s
+--------+---------------------------------------------------------+
| -S     | Set Smart Scroll mode. There are two additional modes   |
|        | which the Windows GUI does not expose.                  |
|        |                                                         |
|        | 0                                                       |
|        |     off                                                 |
|        | 1                                                       |
|        |     normal                                              |
|        | 2                                                       |
|        |     slow                                                |
|        | 3                                                       |
|        |     accelerated scrolling                               |
|        |                                                         |
|        | Accelerated scrolling mode is different from the other  |
|        | modes in that it does not simulate a flywheel but       |
|        | instead sends more scroll wheel clicks the faster you   |
|        | scroll.                                                 |
+--------+---------------------------------------------------------+

License
=======

This software is available under the terms of the ISC license as it appears
in each source file.
