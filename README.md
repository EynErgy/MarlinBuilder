# V1 Engineering Preconfigured Marlin

This repository provides a preconfigured version of Marlin that is optimized for The
V1Engineering.com machines. Special branch for BigtreeTech SKR 1.4 and 1.4 Turbo

![skr_1p4](https://github.com/EynErgy/MarlinBuilder/workflows/skr_1p4/badge.svg)
![skr_1p4t](https://github.com/EynErgy/MarlinBuilder/workflows/skr_1p4t/badge.svg)

## Getting started

This is currently a work in progress. If you are just looking to download firmwares, this probably
isn't the right place to be.

## Development/Understanding these scripts.

Start with the .github/workflows/builds.yml file. That tells github when to start a build, and which
steps to take to complete a particular build.

These are the main steps:
 - Check out a fresh copy of the MarlinFirmware.
 - Call "config-for-machine", which will change the Configuration.h, Configuration_adv.h, etc. for
     that particular machine.
     - This calls a particular machine setup file, like src/configs/V1CNC_Rambo_Dual
     - This calls specific groups of configs, such as configuring it for a CNC, rambo specific
         choices, and accessories such as an LCD and dual endstops.
 - Call platformio to verify the build works (test) and produce a firmware file.
 - Collect the Marlin folder and zip it up. Also collect the firmware file.

To really take a deep look at how Marlin gets set up for a particular build configuration, take a
look at src/configs/V1CNC_Rambo_Dual, and then find each file it uses to configure Marlin, such as
src/configs/boards/rambo.

The configs are broken into different categories:
 - common:
     - v1-base-config: which changes some names to V1, and sets the Bootscreen.h.
     - cnc, 3dp, zxy: Run one of these to set the machine type.
 - boards:
     - Each board type has a file here for configuring the build for that particular board.
     - This can get a little messy, because a titan aero extruder has different steps/mm on a mini
         rambo than on a ramps.
 - accessories:
     - These are board and machine type "independent" features. For example, dual endstops, or
         aero-extruder

If you want to change any of the configurations, make the appropriate changes to the configs/
scripts, push to a new branch, and then let github actions build a .zip for you. Download the zip,
and look at the Configuration files it produced. You should then also do a runtime test by flashing
the board with the firmware file, or flash it through arduino.

Don't forget to set the version number in src/core/version. I don't have a good way to do that
automatically.
