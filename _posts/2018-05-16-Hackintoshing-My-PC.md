---
layout: post
title: "Hackintoshing My PC"
author: "Mark Pinto"
categories: journal
tags: [documentation]
image: hackintosh.jpg
---

# About

For my first real post I will be talking about how I hackintoshed my computer. Hackintoshing is the process of installing macOS on non-Mac hardware. My PC hackintosh build is a build that I found while [reading through Lifehacker one day in 2011.](https://lifehacker.com/5815715/how-to-build-a-hackintosh-mini-for-less-than-600) I have built and maintained this hackintosh since that day; starting with Mac OS X Snow Leopard all the way to the current macOS at the time of this writing, macOS High Sierra. At first updating the OS on my hackintosh was easy to do as there were guides and people posting on forums on how to upgrade from Snow Leopard to Lion for this specific build but as the years went by there were fewer and fewer guides on how to upgrade this hackintosh build. In fact, I have searched for guides on how to upgrade this build to High Sierra but have found none so I had to upgrade without a guide. The purpose of this post is to document how I got macOS High Sierra running on this hackintosh build with the hopes that it will help someone else that still maintains this specific build.

# The Build

Even though the [Lifehacker article](https://lifehacker.com/5815715/how-to-build-a-hackintosh-mini-for-less-than-600) has the build listed I thought I should list it here anyway for documentation purposes.

* Gigabyte GA-H55N-USB3 Motherboard

* Intel Core i3 Processor i3-540 3.06GHz 4MB LGA1156 CPU

* ZOTAC nVidia GeForce GT240 512 MB DDR3 DVI/HDMI PCI-Express Video Card

* 2x2GB Corsair PC3-10666 1333Mhz Dual Chanel 240-pin DDR3 Desktop RAM

* Western Digital 1TB SATA III 7200 RPM 32MB Cache Desktop Hard Drive

* SilverStone SG05BB-450 ALL Black Plastic/SECC Mini-ITX Computer Case with SFX 450W 80+ Bronze Certified/Single +12V rail Power Supply

* Sony Optiarc 8X SATA DVD+/-RW Slim Drive

* 13-pin to Standard+Power Slimline SATA Cable

# Issues

One important thing I should go over are the issues I still encounter with this install of macOS.

1. From time to time the computer will not wake up from sleep. If you do encounter this problem you will have to force shutdown and restart the computer.

2. Whenever I am forced to shutdown the computer such as when the computer does not wake up from sleep as mentioned above, upon a forced shutdown and restart the computer will at times have its audio outputs disabled except for ones built into the display. In other words external speakers will be disabled. This has an easy fix through clover Configurator which I will expand upon further below but it is worth mentioning that you will need to apply this fix every time you encounter this issue, it is not a one time fix.

3. I have not been able to use the DVD drive to burn images onto a disc, it fails. I do not know if it can read media as I have not tested this.

My point is that this install is not 100% perfect. I can work with these issues but not everyone else is willing to stick with these issues and can be a deal breaker. Issue number one doesn't bother me because I always save my work before putting my computer to sleep and if I do have to force shutdown the computer due to it not waking form sleep, macOS reopens all my closed windows upon reboot. Number two is less of an issue to me due to it being an easy issue to fix and that it is a rare occurrence. If you are willing to work with these issues then continue reading ahead. I will of course update this post or even make a new post if I find a way to permanently fix the two issues.

# Requirements

You will need the following to follow this guide:

* This guide assumes that you have already installed macOS on it before whether it is Snow Leopard or any of the more recent versions. This means that you have your BIOS settings properly setup for hackintoshing.

* A USB flash drive (I used a 16GB one but 8GB should work too).

* You will need access to macOS in some way.



# Downloading the macOS High Sierra Update

First things first, one of the things that we need access to is a Mac. Now this can either be a legitimate Mac or a hackintosh; there is also an option to do this through a virtual machine (maybe I'll do a write up on that process one day). With access to Mac OS, you will now want to make use of the App Store to download the Mac OS High Sierra update. While getting the update through the App Store I ran into a problem that seems to be common with Sierra users which is an incomplete download of the update (5-20 MB download instead of 6GB). What I did to overcome this issue is to use the tool [macOS High Sierra Patcher.](http://dosdude1.com/highsierra/) Upon opening the tool click "Tools" on the menubar and then "Download macOS High Sierra...", you will then be asked if you want to proceed, click "Yes" and pick a location for the download (I picked the Applications folder).

![macOS High Sierra Patcher](/assets/img/2018-05-16-Hackintoshing-My-PC/hackintosh_high_sierra_patcher.jpg "macOS High Sierra Patcher in action")

# Setting up the USB

## Writing the installer to the USB

Now that you have downloaded the update, you need to format you USB. Open Disk Utility, click the USB then erase, set the name as "USB", the format to "Mac OS Extended (Journaled)" and the scheme as "GUID Partition Map" then click erase.

After formatting the USB enter the following command into the terminal to write the macOS image onto the USB.

sudo "/Applications/Install macOS High Sierra.app/Contents/Resources/createinstallmedia" --volume /Volumes/USB --applicationpath "/Applications/Install macOS High Sierra.app" --nointeraction

Great! Now you have macOS installation media, now to make it hackintosh compatible.

## Installing Clover on the USB

Now you will install Clover on the USB you just created. Run the Clover package and when asked where to install pick your USB then click "Customize Install" and check the following options:

* Install Clover in the ESP
* Under Bootloader
	* Install boot0af in MBR
* Under CloverEFI
	* CloverEFI 64-bits SATA

After checking these options click install and wait for the process to finish and then you have successfully installed Clover.

![Clover Install Package](/assets/img/2018-05-16-Hackintoshing-My-PC/hackintosh_clover_install.jpg "Installing Clover on the macOS install media")

## Installing Kexts to the EFI folder

Our next step is to put Kexts on the EFI folder of our USB. Navigate to the following directory on your install media EFI/CLOVER/kexts/Other and copy over the following files

[AppleALC.kext](https://github.com/vit9696/AppleALC/releases)

[FakeSMC.kext](http://www.mediafire.com/file/g6845k6ika0pd8n/FakeSMC-3.5.1-2018-02-28%2020.19.59.zip)

[Lilu.kext](https://github.com/vit9696/Lilu/releases)

[NvidiaGraphicsFixup.kext](https://github.com/lvs1974/NvidiaGraphicsFixup/releases)

[RealtekRTL8111.kext](https://bitbucket.org/RehabMan/os-x-realtek-network/downloads/)

[USBInjectAll.kext](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)

Congrats, your builds hardware should now be compatible with macOS

## Using Clover Configurator

You will now run Clover Configurator to tweak some final settings

Open Clover Configurator and click file then open and navigate to /Volumes/EFI/EFI/CLOVER/config.plist

Upon opening the file, change the following settings:

Under the ACPI tab:

Uncheck all of the DSDT fixes listed under "patches"

![ACPI Tab](/assets/img/2018-05-16-Hackintoshing-My-PC/hackintosh_acpi.jpg "All DSDT fixes are unchecked")

Under the SMBIOS tab:

For hardware select "iMac11,2 - Intel Core i3-540 @ 3.06 GHz"

Click the arrows and pick the following hardware configuration

![SMBIOS Tab](/assets/img/2018-05-16-Hackintoshing-My-PC/hackintosh_smbios.jpg "Selecting iMac11,2")

On the Serial field click generate

Open terminal and run the command uuidgen, this command will output a bunch of numbers and letters, copy it and paste it into the SmUUID field. Copy the data from the Board Serial field and switch to the RtVariables tab.

Under the RtVariables tab:

Paste the data from the Board Serial field from the SMBIOS tab into the MLB field.

Under the ROM field set it as UseMacAddr0

Under the BooterConfig field set it to 0x28

Under 


# Installation
