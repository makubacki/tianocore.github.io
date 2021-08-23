Optimize cursor motion sequences; support Linux/UNIX standard (xterm/konsole/gnome-terminal/etc.) key codes and line-drawing characters (currently one must set their terminal emulator to use code page 437 for correct line drawing.)

* Difficulty: Medium
* Language: C
* Mentor: 
* Suggested by: bjjohnson, [@nate-desimone](https://github.com/nate-desimone)

# Background
The Terminal driver is located at https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Universal/Console/TerminalDxe

The most prevalent use case for the terminal driver is to display the BIOS setup menu on headless server systems using a PC style serial port connected to a laptop via null modem. This allows administrators to adjust BIOS settings on rack mounted systems without needing to connect a monitor and keyboard.

# Details
Historically, the BIOS setup menu would be rendered using the IBM PC VGA text mode, which encoded text using code page 437 (CP437). This was important for box-drawing characters, such as ┌ , ─ , and ┐ , which VGA text mode encodes as 0xDA, 0xC4, and 0xBF respectively. However, most terminal emulators assume text to be encoded in UTF-8. Unicode defines these box drawing characters as 0x250C, 0x2500, and 0x2510. In UTF-8 encoding, these characters translate into 3 byte sequences of (0xE2, 0x94, 0x8C), (0xE2, 0x94, 0x80), and (0xE2, 0x94, 0x90) respectively. The VGA encodings of these box characters will end up generating errors if one attempts to decode them as strict UTF-8, though most terminals assume that the intended characters to be drawn are Ú, Ä, and ¿, which have the Unicode character codes 0xDA, 0xC4, and 0xBF. The end result is the BIOS setup menu looks like this:

```
ÚÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄ¿
³                               Device Manager                                 ³
ÀÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÙ

   Devices List                                          List all the Driver
> Driver Health Manager                                 Health instances to
> RAM Disk Configuration                                manage
> OVMF Platform Configuration
> iSCSI Configuration
> Network Device List


   Press ESC to exit.


ÚÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄ¿
³                                                                              ³
³ ^v=Move Highlight       <Enter>=Select Entry      Esc=Exit                   ³
ÀÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÙ
```

Instead of like this:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                               Device Manager                                 │
└──────────────────────────────────────────────────────────────────────────────┘

   Devices List                                          List all the Driver
> Driver Health Manager                                 Health instances to
> RAM Disk Configuration                                manage
> OVMF Platform Configuration
> iSCSI Configuration
> Network Device List


   Press ESC to exit.


┌──────────────────────────────────────────────────────────────────────────────┐
│                                                                              │
│ ^v=Move Highlight       <Enter>=Select Entry      Esc=Exit                   │
└──────────────────────────────────────────────────────────────────────────────┘
```

The terminal driver has fully supported both the legacy CP437 encoding and the UTF-8 encoding for more than 10 years now. Which mode to use is part of the configuration settings given to the terminal driver at start up. However, those configuration settings need to come from somewhere. For example, OVMF has the following page in its setup menu for configuring the serial terminal:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                             Set COM Attributes                               │
└──────────────────────────────────────────────────────────────────────────────┘

   Set COM Baud Rate          <115200>                   Set COM Baud Rate
   Set COM Data Bits          <8>
   Set COM Parity             <None>
   Set COM Stop Bits          <One>
   Set COM Terminal Type      <PC_ANSI>
   Set COM Flow Control       <None>

   Commit Changes and Exit
   Discard Changes and Exit


┌──────────────────────────────────────────────────────────────────────────────┐
│                         F9=Reset to Defaults      F10=Save                   │
│ ^v=Move Highlight       <Enter>=Select Entry      Esc=Exit                   │
└──────────────────────────────────────────────────────────────────────────────┘
```

The default terminal type is PC_ANSI, which uses CP437. In this day and age that is probably not the right default anymore, though one could argue whether PC_ANSI being the default is a "bug". Here is the list of supported terminal types:

UEFI Spec Defined:
* PC_ANSI
* VT_100
* VT_100_PLUS
* VT_UTF8

EDK II Extensions:
* TTY_TERM
* LINUX
* XTERM_R6
* VT_400
* SCO

Currently all of these terminal types except for VT_UTF8 output the box drawing characters using CP437, this is a bug. The VT-100 did not use CP437 for box drawing characters. On the VT-100, the terminal driver should output the escape sequence "ESC ( 0" to switch the character set from ASCII to "DEC Special Graphics". Then, while in DEC Special Graphics mode, ┌ , ─ , and ┐ , are encoded as 0x6C, 0x71, and 0x6B respectively. After outputting the box drawing characters, the terminal driver should switch back to ASCII using the escape sequence "ESC ( B". Implementing this will introduce the interesting problem of optimizing display performance by limiting the number of character mode switches.

For most the modes listed above, the VT-100 method should be used for drawing box characters (and other characters in the DEC special graphics character set). DEC special graphics has been around for such a long time that most terminal emulators support it. However the very popular [PuTTY](https://www.putty.org) application is an exception to this, and since 2016 XTerm and most other Linux terminal emulators have enabled UTF-8 by default. So, UTF-8 it should be the preferred method of outputting characters outside the initial 0-127 basic ASCII codes for most modern terminal emulators. For older terminal emulators from the 2000's era, DEC Special Graphics should be attempted first with UTF-8 as a fallback if the character can't be encoded by either basic ASCII or DEC special graphics. VT_UTF8 should exclusively output UTF-8. PC_ANSI mode should exclusively output CP437. VT_100 should output data that would be render-able on a actual [DEC VT100](https://en.wikipedia.org/wiki/VT100) and as such should replace any character outside of basic ASCII or DEC Special Graphics with "?".

## Expected Behavior

| Terminal Type | DEC Special Graphics Allowed | Simple Box Drawing (┌ , ─ , ┐ , etc.) | Advanced Box Drawing ( ╔ , ═ , ╗ , etc.) |  UTF-8 Fallback |
|-------------|--------------------|----------------------|------------------------------------------|----|
| PC_ANSI     | :x:                | CP437                | CP437 | :x: (If character is not in CP437, output "?") |
| VT_100      | :x: | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not in basic ASCII or DEC Special Graphics, output "?") |
| VT_100_PLUS | :heavy_check_mark: | DEC Special Graphics | Convert to Simple Box Drawing and use DEC Special Graphics | :x: (If character is not in basic ASCII or DEC Special Graphics, output "?") |
| VT_UTF8     | :x:                | UTF-8                | UTF-8 | :heavy_check_mark: |
| TTY_TERM    | :x:                | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not in basic ASCII, output "?") |
| LINUX       | :x:                | UTF-8                | UTF-8 | :heavy_check_mark: |
| XTERM_R6    | :heavy_check_mark: | DEC Special Graphics | UTF-8 | :heavy_check_mark: |
| VT_400      | :heavy_check_mark: | DEC Special Graphics | Convert to Simple Box Drawing and use DEC Special Graphics | :x: (If character is not in basic ASCII or DEC Special Graphics, output "?") |
| SCO         | :heavy_check_mark: | DEC Special Graphics | UTF-8 | :heavy_check_mark: |

## Current Behavior

| Terminal Type | Simple Box Drawing (┌ , ─ , ┐ , etc.) | Advanced Box Drawing ( ╔ , ═ , ╗ , etc.) |  UTF-8 Fallback |
|-------------|-------|-------|------------------------------------------|
| PC_ANSI     | CP437 | CP437 | :x: (If character is not basic ASCII or box drawing, output "?") |
| VT_100      | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |
| VT_100_PLUS | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |
| VT_UTF8     | UTF-8 | UTF-8 | :heavy_check_mark: |
| TTY_TERM    | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |
| LINUX       | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |
| XTERM_R6    | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |
| VT_400      | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |
| SCO         | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | Convert to poor man's ASCII (+ , - , &#124; , \ , / ) | :x: (If character is not basic ASCII or box drawing, output "?") |

Now, here is the second bug. That BIOS setup menu page that OVMF has for configuring the serial port has a field for setting the terminal type. But, changing the value in that field doesn't actually change the configuration data that is sent to the terminal driver. So the terminal driver always ends up using PC_ANSI mode even if the user changes that setting. This isn’t a bug in the terminal driver really, it’s a bug in OVMF's setup menu implementation. But it does create the appearance of a problem in the terminal driver and should be fixed as part of this GSoC project. This should be fixed in both he OVMF implementation and the MinPlatform implementation.

# Development Environment
Building: This project should support all edk2 supported OSes and toolchains.

# Links for More Information
* https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Universal/Console/TerminalDxe
* https://github.com/tianocore/edk2/tree/master/OvmfPkg
* https://en.wikipedia.org/wiki/Box-drawing_character
* https://en.wikipedia.org/wiki/UTF-8
* https://en.wikipedia.org/wiki/Code_page_437
* https://en.wikipedia.org/wiki/DEC_Special_Graphics

* https://en.wikipedia.org/wiki/VT100
* https://www.putty.org

# Further Discussion
Interested parties are welcome to discuss this project on [edk2-devel](https://edk2.groups.io/g/devel).

# See Also
[[Tasks]]