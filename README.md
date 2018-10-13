# Dell XPS Enhancements

This is a comprehensive list of enhancements that aim to create the optimal Dell XPS 13/15 experience. Enhancements have been exclusively tested on a [Dell XPS 13 9370](http://www.dell.com/en-us/shop/dell-laptops/new-xps-13/spd/xps-13-9370-laptop), and should work on other configurations unless otherwise noted. Changes or additions are welcome in the form of pull requests.

* [Audio](#audio)
  * [Installation Instructions](#installation-instructions)
* [Killer Wireless](#killer-wireless)
* [Intel Graphics Drivers](#intel-graphics-drivers)
* [Bloatware and Driver Maintenance](#bloatware-and-driver-maintenance)
* [Function Key Behavior](#function-key-behavior)
* [Applications Key](#applications-key)
* [Notebook Sleep FN Key shortcut](#notebook-sleep-fn-key-shortcut)
* [Save Your Windows Product Key](#save-your-windows-product-key)
* [Advanced Performance Tweaks](#advanced-performance-tweaks)
  * [Power Plans](#power-plans)
  * [Undervolting and Speed Shift](#undervolting-and-speed-shift)
  * [Dell Power Manager](#dell-power-manager)
* [Miscellaneous Tweaks](#miscellaneous-tweaks)

## Audio

The Waves MaxxAudio software, which is part of the Dell RealTek drivers and is packaged with the Dell factory image causes numerous problems:

* Even with all effects disabled, post-processing still occurs.
* Connecting a device to the headphone jack will nag you with a pop-up asking you to pick what device you just plugged in (headphone, headset or microphone). Instructing the software not to ask again is inconsistent and will often be reset on reboot.
* Using the Generic Microsoft High Definition Audio drivers will produce crackling and popping under certain conditions.
* Waves MaxxAudio is known to push audio beyond acceptable volumes, which can cause physical speaker damage to your laptop.

*NOTE*: Waves MaxxAudio is not the same as the Dell Audio Application. These are separate drivers and you should not follow these instructions if your laptop came with the Dell Audio Application instead of Waves MaxxAudio. These instructions will work for any laptop with the Realtek driver package, not only XPS.

*NOTE2*: Previously, there were instructions here for installing a modified Realtek driver that removed Waves MaxxAudio. As of Windows 10 1809, these instructions no longer work and have been removed. If necessary, the instructions can be found [here](https://github.com/distantorigin/XPS-Enhancements/tree/43132b1df3eb53621d978fe2921ee27433afad43#audio)

### Instructions

1. Access the device manager by right-clicking the "Start" button or by pressing Windows Key + X and selecting "Device Manager"
2. Under "Sound, video and game controllers", right-click Realtek Audio and select "Update Driver."
3. Click "Browse my computer for driver software," and then select "Let me pick from a list of available drivers on my computer."
4. Uncheck the "Show compatible hardware" checkbox and scroll down to Microsoft in the list of vendors.
5. Select the latest version of "High Definition Audio Device" and click next.
6. You may receive a warning that says: Installing this device driver is not recommended because Windows cannot verify that it is compatible with your hardware.  If the driver is not compatible, your hardware will not work correctly and your computer might become unstable or stop working completely.  Do you want to continue installing this driver? This is quite normal; we're merely installing the generic Windows 10 audio driver distributed from Microsoft. Click yes and complete the installation.
7. Restart your computer.
8. Done! Enjoy your crystal-clear audio experience.

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

## Advanced Performance Tweaks

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
