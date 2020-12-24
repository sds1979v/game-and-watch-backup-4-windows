# Game and Watch Backup and Restore tools 4 Windows

This repository contains pre-built tools for backing up & restoring the original Game and Watch firmware.

What you'll need:
- A Game & Watch in original state
- An ARM debug probe (Tested with J-Link and ST-Link compatible devices)
- Connections to the [debug port](https://twitter.com/ghidraninja/status/1326860677353512960) - testclips or soldered wires work well!
- A computer with Windows 10 and Powershell.


## Warnings & disclaimer

The tools in this repository will modify both the internal and the external flash of the Game and Watch.
While we tested the scripts to our best ability, we can not guarantee that there won't be failures that will leave your
Game & Watch damaged. Use these tools at your own risk. If you feel like you don't understand what you're doing it might be best to let someone with more experience help (and teach) you!
Feel free to join the dedicated [discord channel](https://discord.gg/rE2nHVAKvn) and ask any support questions in *#game-and-watch-support*.

## Read this first

This manual is for developers, and not yet suitable for end-users. The goal is to enable other developers to be able to contribute to Game & Watch development, and hopefully eventually get to a point where we can release end-user instructions :)

Please note that we recommend either a (full-size, not mini) J-Link/J-Link Edu, or an offical ST-Link. ST-Link clones have caused a lot of issues, please avoid them. Also please disconnect the battery before you continue.

## Connecting the debugger

When connecting the debugger ensure that at least SWDIO, SWDCLK and GND are connected. Do *not* under any circumstances connect 3.3V to the VDD connection. If your debug probe (for example ST-Link clones) does not have a VTREF connector, just leave VDD unconnected. Connecting 3.3V to VDD will likely destroy your SPI flash.

### Supported Debuggers

Please either use an official ST-Link (not one of the small USB stick clones) or a full-size J-Link. Others might work, a lot of them do not work with the 1.9V logic levels used on the Game and Watch.

Programmers we had a lot of trouble with: J-Link EDU Mini (does not work), cheap ST-Link clones.

## Powershell setup

Install the required tools:

Open Powershell as Administrator and launch:
```
Set-ExecutionPolicy Unrestricted
```
then
```
.\0_installtools.ps1
```

## Usage

The scripts are split into 5 parts:

- PS_1_sanity_check.ps1 - Performs sanity check and makes sure all required tools are available
- PS_2_backup_flash.ps1 - Backs up the contents of the SPI flash. Does not modify device contents.
- PS_3_backup_internal_flash.ps1 - Backs up the internal flash. To do this the contents of the SPI flash are modified. Your device will stop working until it's restored in step 5.
- PS_4_unlock_device.ps1 - This will disable the active read protection. This will erase the internal flash of the STM32.
- PS_5_restore.ps1 - This will restore the original firmware.

Just run these scripts *from the checked out directory* one after each other. All scripts are safe to be re-run in case of error.

Ensure that you keep your backup in a safe place so you can always recover. Don't ask us for flash dumps & co, we will not share them.

## What if something goes wrong

As long as your electrical connections are right and you didn't short/overvolt anything, chances are high that it's rescuable:

If a script fails and the device does not work after power-cycling, repeat the script. If it fails again, try to hold the power button of the device while executing the script.



## Sources for binaries

The binaries and firmware are based on:

- [flashloader](https://github.com/ghidraninja/game-and-watch-flashloader)
- [flashdumper](https://github.com/ghidraninja/game-and-watch-flashdumper)
- [game-and-watch-backup](https://github.com/ghidraninja/game-and-watch-backup)

All credits for the hard work go to stacksmashing and kbeckmann
Thank you guys!
