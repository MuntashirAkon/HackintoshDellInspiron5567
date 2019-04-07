# HackintoshDellInspiron5567
My attempts to install macOS on Dell Inspiron 15-5567 (i3-7100u, Intel HD620)

#### Currently not working
- 32-bit color (proper EDID inject may be needed)
- WiFi (Need to replace PCI card, currently using external)
- Card Reader (Will never work)

#### Applied DSDT Patches
##### General Patches
- [sys] Fix _WAK Arg0 v2
- [sys] Fix Mutex with non-zero SyncLevel
- [sys] Shutdown Fix v2
- [usb] USB3 _PRW 0x6D Skylake (instant wake)
##### I2C Patches
- [Windows] Windows 10 Patch
- [GPIO] GPIO Controller Enable [SKL+]
- [TPD0] TPD0 _CRS patch (Bellow)
  ```
  into method label _CRS parent_label TPD0 replace_content begin
  
  Return (ConcatenateResTemplate (SBFB, SBFG))
  
  end;
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
- Only the relevant UEFI drivers are included here. You may need **EmuVariableUefi** in case you're having shutdown problems


#### AppleALC analysis

| layout-id | Int-Mic | Int-Out | Ext-Mic | Ext-Out | ComboJack | Remark
| --- | --- | --- | --- | --- | --- | ---
| 5 | [ ] | [x] | [ ] | [x] | [ ] |
| 11 | [x] | [x] | [ ] | [x] | [ ] |
| 13 | [x] | [x] | [ ] | [x] | [x] | Native
| 13 | [x] | [x] | [ ] | [x] | [ ] | Separate audio inputs
| 28 | [ ] | [x] | [ ] | [x] | [ ] | Separate audio outputs 
| 56 | [x] | [x] | [ ] | [x] | [x] | Indentical to layout-id 13
