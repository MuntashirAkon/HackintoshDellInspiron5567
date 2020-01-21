# HackintoshDellInspiron5567
My attempts to install macOS on Dell Inspiron 15-5567 (i3-7100u, Intel HD620)

Tested OS version(s): 10.14.6 (macOS Mojave), 10.15.2 (macOS Catalina)

#### Currently not working
- **32-bit color:** You may notice color gradient (due to 24-bit color) if you see an HD movie or
  open Launchpad. Although this problem is fairly common, nobody seems to bother with it.
  Proper EDID injection may be required to overcome this problem. I've already tried a lot of
  combinations, none solved it however. SkyLake-spoofing seems to solve this problem but
  only partially.
- **WiFi:** Need to replace the PCIe card with a supported card. I'm currently using an external
  TL-WN725N wireless adapter
- **Card Reader:** Probably it will never work as the system cannot detect the amount of ampere
  required. Some patching at AppleUSBCardReader binary is required to enable it, which requires
  a lot of effort since nobody ever tried that
- **Hibernation:** I've disabled it since I'm using an SSD (which is extremely fast)
- **Shutdown/reboot problem**: The device may shut itself down with a noise when reboot/shutdown is requested. If this happens, remove the charging cable (or try pushing the button with a regular interval) to start the device again

#### Notes
- Be sure to fill the *MASKED* values at `config.plist/PlatformInfo/Generic` as well as ROM values. (See the PlatformInfo section of the OC's guide if you don't know which values to put there)
- In SSDT-UIAC, only the following ports are enabled: HS01, HS02, HS03, HS05, HS06, HS08, SS01 and SS02. Other ports are not needed. You can also disable HS06 (Card Reader) if you want
- It is generally recommended to keep your kexts in both `OC/Kexts` and `/Library/Extensions`

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

* CodecCommander.kext is needed for layouts other than 13, 14 and 56

#### EDID
##### Current
```
00ffffffffffff0030e4250500000000001a0104902213780a42e59158558f281e505400000001010101010101010101010101010101d01d56f4500016303020350058c21000001ad91756f4500016303020350058c21000001a000000fe004839374831803135365748550a0000000000004131940010000009010a20200092
```
##### Original
```
00ffffffffffff0030e4250500000000001a0104952213780a42e59158558f281e505400000001010101010101010101010101010101d01d56f4500016303020350058c21000001ad91756f4500016303020350058c21000001a000000fe004839374831803135365748550a0000000000004131940010000009010a2020008d
```
