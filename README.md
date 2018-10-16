# Dell XPS Enhancements

This is a comprehensive list of enhancements that aim to create the optimal Dell XPS 13/15 experience. Enhancements have been exclusively tested on a [Dell XPS 13 9370](http://www.dell.com/en-us/shop/dell-laptops/new-xps-13/spd/xps-13-9370-laptop), and should work on other configurations unless otherwise noted. Changes or additions are welcome in the form of pull requests.

* [Audio](#audio)
  * [Notes Before You Begin](#notes-before-you-begin)
  * [Installation Instructions](#installation-instructions)
* [Killer Wireless](#killer-wireless)
* [Intel Graphics Drivers](#intel-graphics-drivers)
* [Bloatware and Driver Maintenance](#bloatware-and-driver-maintenance)
* [Function Key Behavior](#function-key-behavior)
* [Applications Key](#applications-key)
* [Notebook Sleep FN Key shortcut](#notebook-sleep-fn-key-shortcut)
* [Save Your Windows Product Key](#save-your-windows-product-key)
* [Linux Tweaks](#linux-tweaks]
* [Advanced Performance Tweaks](#advanced-performance-tweaks)
  * [Power Plans](#power-plans)
  * [Undervolting and Speed Shift](#undervolting-and-speed-shift)
  * [Dell Power Manager](#dell-power-manager)
* [Miscellaneous Tweaks](#miscellaneous-tweaks)

## Audio

The Waves MaxxAudio software, which is part of the Dell Realtek drivers and is packaged with the Dell factory image causes numerous problems:

* Even with all effects disabled, post-processing still occurs.
* Connecting a device to the headphone jack will nag you with a pop-up asking you to pick what device you just plugged in (headphone, headset or microphone). Instructing the software not to ask again is inconsistent and will often be reset on reboot.
* Using the Generic Microsoft High Definition Audio drivers will produce crackling and popping under certain conditions.
* Waves MaxxAudio is known to push audio beyond acceptable volumes, which can cause physical speaker damage to your laptop.

Luckily, a user has created a copy of the Dell Realtek drivers without Waves MaxxAudio, which removes post-PROCESSING and the GUI pop-up. This is confirmed to work on Dell XPS 13 93XX and Dell XPS 9550/9560. I'm running it on the latest Dell XPS 13 9370 with success. It may work on other Dell laptops that ship with Waves MaxxAudio; if you are aware of models where this driver works, please post to [/r/dell](https://www.reddit.com/r/dell/) and submit a pull request so I can update the guide.

[Kevin Shroff's Modded Realtek Audio Drivers](https://github.com/kevinshroff/KSMRD-Modded-Realtek-Audio-Drivers/) | [Download](https://github.com/distantorigin/XPS-Enhancements/blob/master/Audio/KSMRD-v3.0.2.zip) | [Official Releases Page](https://github.com/kevinshroff/KSMRD-Modded-Realtek-Audio-Drivers/releases/)

### Notes Before You Begin

Waves MaxxAudio is not the same as the Dell Audio Application. These are separate drivers and you should not run this if your laptop came with the Dell Audio Application instead of Waves MaxxAudio.

I've included a copy of the latest driver available on Kevin's page in this repository for mirroring purposes, and the download link currently uses this version. I'll attempt to keep it as up-to-date as I can, but the official releases page may have more recent versions.

Finally, for a brief stint, the instructions here were replaced with steps that encouraged users to use the generic Microsoft High Definition Audio drivers on Windows 10 1809 and later. I've since redacted these and advise reverting the changes; I was informed that the modified Realtek drivers are still being maintained. Steps below will assist you in this process:
1. Access the device manager by right-clicking the "Start" button or by pressing Windows Key + X and selecting "Device Manager"
2. Under "Sound, video and game controllers", right-click High Definition Audio Device and select "Update Driver."
3. Click "Browse my computer for driver software," and then select "Let me pick from a list of available drivers on my computer."
4. Select the latest iteration of Realtek Audio and click next.
5. Allow the installation to finish. When you're prompted to restart, choose yes.
6. Proceed to the official instructions below once your machine reboots.

### Installation Instructions

Steps are included below for mirroring purposes. All credit should be given to [Kevin Shroff](http://paypal.me/kevinshroff) and the above linked thread should be used as an official reference, as it is much more comprehensive.

1. Disable Secure Boot: Secure Boot must be disabled in the BIOS/UEFI of your computer. This is required to install the driver, as this modded driver is not signed. After driver installation Secure Boot will be re-enabled in later steps. For most Dell XPS machines, UEFI settings can be accessed by repeatedly pressing F2 after turning on your computer.
2. Run Command Prompt with Administrator privileges, then run the following commands, pressing enter after each:

    ```powershell
    bcdedit.exe -set loadoptions DISABLE_INTEGRITY_CHECKS
    bcdedit.exe -set TESTSIGNING ON
    ```

3. Reboot
4. Uninstall "Realtek High Definition Audio" from Control Panel, and uninstall "Realtek Audio" from Device Manager - do not reboot!
5. Launch Disk Cleanup, select the drive for your Windows installation if necessary, click "Clean up system files" and check "Device driver packages." Click OK and allow Disk Cleanup some time to purge the old driver files.
*In the event that you do not have this option in Disk Cleanup*: Install [Driver Sweeper](https://mega.nz/#!wN8BSIwK!Fo_sQzDsvLNkpwLngESfBaiBHtsMiskGxCjNZV_Vs7s), select Realtek - Sound, and then click clean - do not reboot!
6. Navigate to where you extracted the driver zip download, and open the "Modded-KSMRD" folder. Run "Setup.exe" inside of it and install the driver just like a normal Realtek Audio driver.
7. You will get a popup that asks if the driver is safe and should be installed - click "Yes, install this driver" (or something along those lines) - this popup comes up only because the driver signature is not signed, as it is modified. Driver itself is safe and you can scan it with Antivirus for assurance.
8. After the installation is done, reboot
9. Run Command Prompt with Administrator privileges, then run the following commands, pressing enter after each:

    ```powershell
    bcdedit.exe -set loadoptions ENABLE_INTEGRITY_CHECKS
    bcdedit.exe -set TESTSIGNING OFF
    ```

10. Boot back into your BIOS/UEFI and turn back on Secure Boot
11. The testsigning watermark (some text in the corner of your screen, on your wallpaper) should be gone now. If it is not gone, run step 7 again and reboot, then continue to step 10
12. Open task manager, go to startup tab, and disable both "Realtek HD Audio Universal Service" and "Waves MaxxAudio Service Application", and reboot.
13. Open the Windows Registry Editor (regedit.exe) and change the values of ALL instances of ConservationIdleTime and PerformanceIdleTime to FF FF FF under HKEY_LOCAL_MACHINE\SYSTEM\ (use CTRL+F to search for ConservationIdleTime, etc.).
14. Done! Enjoy your crystal-clear audio experience.

## Killer Wireless

The Killer wireless 1435/1535 NIC is notably unreliable, causing disconnections, lag, or even going as far as crashing routers with outdated firmware. For laptops earlier than the 9370, the card can easily be replaced with something like the Intel 8265 adapter. However, on the 9370 and presumably later models of the XPS 15, the NIC is soldered. I have had significant success by uninstalling the Killer Wireless Suite driver and control panel completely, and replacing it with only the driver distributed by Killer, not Dell.

1. Download the installation package from [Killer's Website](https://www.killernetworking.com/driver-downloads/category/other-downloads), making sure that you only get the driver package, not the control panel.
2. Uninstall all Killer Wireless products from your system, and reboot. (You will lose network connectivity temporarily - this is normal.)
3. Install the driver file you downloaded, and reboot again.

## Intel Graphics Drivers

Dell ships a locked down version of the Intel Graphics Driver, and an attempt to install the driver distributed by Intel will be met with the message "The driver being installed is not validated for this computer."

On Dell XPS 15 (95XX) models which include a dual GPU, deviating from the Dell supplied drivers may lead to side effects. On Dell XPS 13 (93XX), the Dell supplied driver can be successfully replaced with that of Intel.

To do so, download the .zip version of the Intel driver [here](https://www.intel.com/content/www/us/en/support/products/128199/graphics-drivers/graphics-for-8th-generation-intel-processors.html) and unpack the zip archive, then follow the steps illustrated [here](https://www.howtogeek.com/343287/how-to-fix-the-driver-being-installed-is-not-validated-for-this-computer-on-intel-computers/) or described below.

1. Access the device manager by right-clicking the "Start" button or by pressing Windows Key + X and selecting "Device Manager"
2. Under "Display adapters", right click on the Intel HD Graphics device, and select "Properties"
3. Under the "Driver" tab in the properties dialog, select "Update Driver."
4. Choose "Browse my computer for driver software" in the wizard, and select "Let me pick from a list of available drivers on my computer"
5. Click the "Have Disk..." button and point the dialog at the directory into which the zip driver was extracted, locating the "igdlh64.inf" file therein.
6. Select the driver and install it.

After a successful installation, re-install the driver using the Intel installer; this procedure is necessary to break away from the Dell locked driver once. Subsequent installations of the Intel driver will succeed by simply running the installer.

## Bloatware and Driver Maintenance

The Dell factory image shipped with XPS laptops comes with a great deal of Dell applications, most of which can be uninstalled. These applications can always be reinstalled from the Windows Store. You should meticulously look through and remove any bloatware, such as Mcafee or metro games like Candy Crush.

I would also advise removing the Dell Update utility, as this may install newer, potentially broken drivers over your existing working ones. You should always use the Dell website and a healthy dose of common sense to ascertain whether you should update. [Snappy Driver Installer Origin](http://www.snappy-driver-installer.org/) is a good source of keeping up-to-date drivers that are provided directly by the hardware vendor, not Dell. I've found it especially useful for Chipset updates.

## Function Key Behavior

Out of the box, your Dell XPS machine will have the function keys "locked." this means that function keys, rather than exhibiting typical functions, will alter mute, volume, screen brightness, media playback and keyboard backlight. To fix this you should enter the system BIOS by restarting and holding down F2 at the Dell logo, then search under advanced for 'Function Key Lock' or 'Function Key Behavior'. What this is called is dependent on your BIOS version. Alternatively, if you wish to temporarily modify the behavior of your function keys, press FN+escape while booted into the operating system of your choice. I've been told that Windows 10 will allow you to change this from within Windows Mobility Center, though have not been able to reproduce this.

## Applications Key

*NOTE*: This tweak is not XPS specific.

To utilize your applications key after changing function key behavior, you will need to press FN+right alt. For many, this is undesirable, and I find myself rarely using right alt the majority of the time. This registry tweak will change right alt to act as an applications key.

[Download](https://github.com/distantorigin/XPS-Enhancements/blob/master/Applications.reg) | [Download SharpKeys for further keymap modification](http://www.randyrants.com/category/sharpkeys/)

## Notebook Sleep FN Key shortcut
Increasingly, as I switched between various keyboards, I found myself often triggering the sleep function via the FN+end shortcut. If like me, you frequently do this in the middle of your work, you can disable this by following these steps (Windows 10 only, sorry Linux users):

1. Go to settings -> System -> Power and Sleep.
2. Scroll down to Related Settings, and then click 'Additional Power Settings'.
3. Go to 'Choose what the power buttons do', and under the options for sleep button, choose do nothing.

## Save Your Windows Product Key

Open up a command prompt window under your administrator account and type:

```powershell
wmic path softwarelicensingservice get OA3xOriginalProductKey > "%UserProfile%\documents\Windows Product Key.txt"
```

This will create a file in your Documents folder aptly named Windows Product Key.txt which contains your Windows 10 product key.

## Linux Tweaks

While this guide is primarily geared towards Windows users, the ArchWiki has very comprehensive information for Dell XPS machines running Linux, often including improvement-of-life adjustments, hardware warnings, power management tips, or Kernel modifications. Follow the link for your respective model.

* [Dell XPS 15 9570](https://wiki.archlinux.org/index.php/Dell_XPS_15_9570)
* [Dell XPS 15 9560](https://wiki.archlinux.org/index.php/Dell_XPS_15_9560)
* [Dell XPS 15 9550](https://wiki.archlinux.org/index.php/Dell_XPS_15_(9550))
* [Dell XPS 13 9370](https://wiki.archlinux.org/index.php/Dell_XPS_13_(9370))
* [Dell XPS 13 2-in-1 (9365)](https://wiki.archlinux.org/index.php/Dell_XPS_13_2-in-1_(9365))
* [Dell XPS 13 9360](https://wiki.archlinux.org/index.php/Dell_XPS_13_(9360))
* [Dell XPS 13 9350](https://wiki.archlinux.org/index.php/Dell_XPS_13_(9350))
* [Dell XPS 13 9343](https://wiki.archlinux.org/index.php/Dell_XPS_13_(9343))
* [Dell XPS 13 9333](https://wiki.archlinux.org/index.php/Dell_XPS_13_(9333))
* [And potentially more by the time you read this](https://wiki.archlinux.org/index.php?search=dell+XPS&title=Special%3ASearch&go=Go)

### Power Plans

It is often beneficial to switch power plans automatically based on what you're doing with your laptop. An application that does a very good job at this is [Power Plan Switcher](https://github.com/petrroll/PowerSwitcher). Once installed, this little program will sit in your system tray and allows you to quickly change your power plan by right-clicking the icon. More importantly, it will start up with Windows and has the ability to switch the current plan when the AC adapter is plugged or unplugged. I have mine set to go to 'High Performance' when on AC and 'Balanced' when on battery.

As of the latest Fall Creators Update, Windows 10 has removed all power plans save for the default. You can get them back by creating a new power plan and selecting the type of power plan (balanced, high performance, power saver) and naming it accordingly. Take note that Connected Standby must be disabled for these secondary plans to appear.

Using an Intel 8th Generation I7 8550U,  my CPU's base frequency is increased from 1.8GHz to 2.0GHz under the high performance plan. Your mileage may vary.

### Undervolting and Speed Shift

Undervolting slightly reduces the voltage supplied to the CPU. The results of this can vary, though upon a successful undervolt you should achieve the same performance with lower temperatures and less thermal throttling. Speed Shift is available on Intel processors and is the predecessor to Intel Speed Step. Unfortunately, most Dell XPS laptops do not have the option for Speed Shift enabled in the Dell BIOS, although the hardware supports it. I found that following the gist of [this](https://www.notebookcheck.net/How-to-Lower-Temperatures-Stop-Throttling-and-Increase-Battery-Life-The-ThrottleStop-Guide-2017.213140.0.html) guide for ThrottleStop at Notebookcheck immensely helpful.

I've spent a while tuning my hardware by small increments, and the results are noticeable. If you're running a mobile Kaby Lake processor, your undervolt offset should range anywhere between -80mv to -115mv. Tweak as needed. Skylake and earlier are more flexible, since they require more power. You may be able to go all the way down to a -135mv offset with these older processors, though you shouldn't move your offset by huge values at a time. Generally the procedure goes:

1. Start at -80mv and gradually lower your offset by -5 or -10MV.
2. Run a stress test, such as [Prime95](https://www.mersenne.org/download/) for several hours. If your offset is too low, the operating system will crash and land you at a blue screen. Critical system errors will reset your voltage settings back to default.
3. If your system doesn't crash after an extended period, go back to step 1.

### Dell Power Manager

TBD

## Miscellaneous Tweaks

This section is still in progress and will be receiving updates in the near future. These are small tweaks that I can verify work on an XPS 13 with Windows 10, though they aren't necessarily machine specific and only serve to enhance performance and privacy.

* Disable Cortana.
 *NOTE*: You should also disable Cortana from within the Group Policy Editor. Go to run, type `gpedit.msc`, and go to Administrative Templates -> Windows Components -> Search. Disable 'Allow Cortana'
* Disable everything under Settings -> Privacy
* Block unnecessary outbound network connectivity with [TinyWall](https://tinywall.pados.hu/)
* Install [Classic Shell](http://classicshell.net/)
* Install Windows 7 Task Manager [here](https://winaero.com/blog/get-classic-old-task-manager-in-windows-10/)
* Disable unnecessary services by following [Black Viper's Windows 10 Service Configurations](http://www.blackviper.com/service-configurations/black-vipers-windows-10-service-configurations/)
* Consider running software to take care of other aspects of Windows 10 spying, such as [ShutUP10](https://www.oo-software.com/en/shutup10) or [DisableWinTracking (GitHub Page)](https://github.com/10se1ucgo/DisableWinTracking/releases)
