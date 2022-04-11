# HackintoshDellInspiron5567
Use macOS on Dell Inspiron 15-5567 (i3-7100u, Intel HD620) with OpenCore bootloader.

Tested OS version(s): 10.14.6 (macOS Mojave), 10.15.2 (macOS Catalina), 11.6.4 (macOS Big Sur).

#### Issues
Although the OS itself is very stable, there are a few known issues:

- **Card Reader.** ~~Card reader may never work as the system cannot detect the amount of
  ampere required by the port. Patching in the **AppleUSBCardReader** binary is required to
  enable it, which requires a lot of effort since nobody actually ever tried this.~~ There is a new
  driver, but I haven't tried it yet. I will update the info if I try it.
- **Hibernation.** I've disabled it as I'm using an SSD (Samsung, 512 GB).
- **Shutdown/reboot problem.** Rarely, the device may shut itself down with a click sound during
  a reboot or a shutdown attempt. If this happens, remove the charging cable or try pushing the
  button at a regular interval to start the device again.

#### Notes
- Be sure to fill the *MASKED* values at `config.plist/PlatformInfo/Generic` as well as ROM values. (See the PlatformInfo section of the OC's guide if you don't know which values to put there).
- In SSDT-UIAC, only the following ports are enabled: HS01, HS02, HS03, HS05, HS06, HS08, SS01 and SS02. Other ports are not needed. You can also disable HS06 (Card Reader) if you want.

#### Disable Hibernate Mode
```
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
```

#### AppleALC analysis

| layout-id | Int-Mic | Int-Out | Ext-Mic | Ext-Out | ComboJack | Remarks
| --- | --- | --- | --- | --- | --- | ---
| 5 | N | Y | N | Y | N |
| 11 | Y | Y | N | Y | N |
| 13 | Y | Y | N | Y | Y | Needs ComboJack_Installer.zip and SSDT_ALC256
| 14 | Y | Y | N | Y | Y | Similar to layout-id 13
| 21 | Y | Y | N | Y | Y | Native, Separate audio inputs (recommended)
| 22 | - | - | - | - | - | Not tested
| 28 | N | Y | N | Y | N | Separate audio outputs
| 56 | Y | Y | N | Y | Y | Indentical to layout-id 13
| 57 | - | - | - | - | - | Not tested
| 66 | - | - | - | - | - | Not tested
| 97 | - | - | - | - | - | Not tested

* **CodecCommander.kext** is needed for layouts other than 13, 14 and 56

#### EDID
##### Current
```
00ffffffffffff0030e4250500000000001a0104902213780a42e59158558f281e505400000001010101010101010101010101010101d01d56f4500016303020350058c21000001ad91756f4500016303020350058c21000001a000000fe004839374831803135365748550a0000000000004131940010000009010a20200092
```
##### Original
```
00ffffffffffff0030e4250500000000001a0104952213780a42e59158558f281e505400000001010101010101010101010101010101d01d56f4500016303020350058c21000001ad91756f4500016303020350058c21000001a000000fe004839374831803135365748550a0000000000004131940010000009010a2020008d
```
