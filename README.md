# HackintoshDellInspiron5567
My attempts to install macOS on Dell Inspiron 15-5567 (i3-7100u, Intel HD620)

#### Currently not working
- **32-bit color:** You may notice color gradient (due to 24-bit color) if you see an HD movie or
  open Launchpad. Although this problem is fairly common, nobody seems to bother with it.
  Proper EDID injection may be required to overcome this problem. I've already tried a lot of
  combinations, none solved it however
- **WiFi:** Need to replace the PCIe card with a supported card. I'm currently using an external
  TL-WN725N wireless adapter
- **Card Reader:** Probably it will never work as the system cannot detect the amount of ampere
  required. Some patching at AppleUSBCardReader binary is required to enable it, which requires
  a lot of effort since nobody ever tried that
- **Hibernation:** I've disabled it since I'm using an SSD (which is extremely fast)

#### Applied DSDT Patches
Patches are now located in the Patches directory in this repo. You can add the following URL to MaciASL at **MaciASL > Preferences > Sources**.
```
http://raw.github.com/MuntashirAkon/HackintoshDellInspiron5567/master/Patches
```

#### EDID
##### Current
```
00ffffffffffff0030e4250500000000001a0104902213780a42e59158558f281e505400000001010101010101010101010101010101d01d56f4500016303020350058c21000001ad91756f4500016303020350058c21000001a000000fe004839374831803135365748550a0000000000004131940010000009010a20200092
```
##### Original
```
00ffffffffffff0030e4250500000000001a0104952213780a42e59158558f281e505400000001010101010101010101010101010101d01d56f4500016303020350058c21000001ad91756f4500016303020350058c21000001a000000fe004839374831803135365748550a0000000000004131940010000009010a2020008d
```

#### Notes
- Don't use my DSDT on your system even if your system configuration is similar to me. Use the patches I've described/given above: it's for your own good
- In SSDT-UIAC, only the following ports are enabled: HS01, HS02, HS03, HS05, HS06, HS08, SS01 and SS02. Other ports are not needed. You can also disable HS06 (Card Reader) if you want
- The theme in the **config.plist** is **Outlines**: you should change it to whatever you're using
- It is generally recommended to keep your kexts in both `Clover/kexts/Other` and `/Library/Extensions`
- Only the relevant UEFI drivers are included here. You may need **EmuVariableUefi** in case you're having trouble in shutting down your hack

#### Disable Hibernate Mode
```
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
```

#### AppleALC analysis

| layout-id | Int-Mic | Int-Out | Ext-Mic | Ext-Out | ComboJack | Remark
| --- | --- | --- | --- | --- | --- | ---
| 5 | N | Y | N | Y | N |
| 11 | Y | Y | N | Y | N |
| 13 | Y | Y | N | Y | Y | Native
| 21 | Y | Y | N | Y | N | Separate audio inputs
| 28 | N | Y | N | Y | N | Separate audio outputs
| 56 | Y | Y | N | Y | Y | Indentical to layout-id 13

You can use one of 13, 21 or 56 since external mic isn't working in all of them. If you're using layout 21, you don't need ComboJack and VerbStub.kext: use CodecCommander.kext instead.
