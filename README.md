# UAT (ADS-B 978) test signal generator

## Disclaimer

This low power UAT test source is designed for a lab or an aircraft hangar use only.

## Legitimate use

Radio being used in the project is rated for 14 dBm (25 mW) of maximum transmit power.<br>
FCC compliant built-in RF filter reduces the power even more, down to approximately 1 mW on the reference frequency.<br>
This power is sufficient to cover an area of just only few meters around the transmitter.<br>

## Hardware

### Variant 1. Basic.

[TI SimpleLink CC1310 LaunchPad kit (915 MHz)](http://www.ti.com/tool/LAUNCHXL-CC1310)

<br>

<img src="http://processors.wiki.ti.com/images/2/28/CC2650LPV1.JPG" height="427" width="262">

<br>

### Variant 2. Advanced.

<br>

<img src="https://github.com/lyusupov/SoftRF/raw/master/documents/images/SoftRF-UAT-1.jpg" height="262" width="400"><br>
<br>
<img src="https://github.com/lyusupov/SoftRF/raw/master/documents/images/SoftRF-UAT-2.jpg" height="199" width="400"><br>

Schematics of the adapter and Gerber files of the PCB will be published soon after I'll receive the PCB samples<br>
and will make sure that the design has no any critical issues.

#### Bill of materials

Number|Part|Qty|Picture|Source
---|---|---|---|---
1|PCB|1|<img src="https://github.com/lyusupov/UAT-test-signal/raw/master/notes/pics/SoftRF-UAT-PCB.jpg" height="388" width="300">|&nbsp;TBD&nbsp; <!-- <a href="https://PCBs.io/share/rYeN0"><img src="https://s3.amazonaws.com/pcbs.io/share.png" alt="Order from PCBs.io"></img></a>  -->
2|Ebyte E70-915T14S<br>RF module|1|![](https://github.com/lyusupov/UAT-test-signal/raw/master/notes/pics/E70-915T14S.jpg)|[AliExpress](http://s.click.aliexpress.com/e/nysD1eu)
3|Female SMA-KHD|1|<img src="https://github.com/lyusupov/SoftRF/raw/master/documents/images/bom/sma-khd.jpg" height="300" width="300">|[AliExpress](https://www.aliexpress.com/item/10-Pcs-SMA-Female-Jack-Solder-Edge-1-6mm-Space-PCB-Mount-Straight-RF-Connector-New/32842094243.html)
4|2x7 female header 2.54mm|1|![](https://github.com/lyusupov/SoftRF/blob/master/documents/images/bom/f2x7.jpg)|Local
5|1x40 male header 2.54mm|2|<img src="https://github.com/lyusupov/SoftRF/blob/master/documents/images/bom/m40.jpg" height="283" width="200">|[AliExpress](https://www.aliexpress.com/item/10pcs-40-Pin-1x40-Single-Row-Male-2-54-Breakable-Pin-Header-Connector-Strip-for-Arduino/32806313091.html)
6|XDS110 cJTAG debug tool (clone)|1|<img src="https://github.com/lyusupov/UAT-test-signal/raw/master/notes/pics/XDS110_clone.jpg" height="300" width="221">|[AliExpress](https://www.aliexpress.com/item/-/32938193571.html)
7|40 pcs. female DuPont jumper wires|1|<img src="https://github.com/lyusupov/SoftRF/blob/master/documents/images/bom/40DuPont.jpg" height="237" width="300">|[AliExpress](http://www.aliexpress.com/item/40-pcs-30cm-1p-1p-female-to-female-2-54mm-Spacing-Jumper-Wire-Dupont-Cable/1905309688.html) 



## Firmware

### Build instructions

1) either:
- download this CCS project from GitHub, then upload it into your [TI CCS Cloud IDE](https://dev.ti.com/), or ;
- if you have a GitHub account: "fork" this project into your GitHub space, then import directly into TI CCS Cloud IDE.
2) build, then run the firmware on your CC1310 hardware with assistance of [TI Cloud Agent](http://processors.wiki.ti.com/index.php/TI_Cloud_Agent) and [TI Cloud Agent Bridge](https://chrome.google.com/webstore/detail/ticloudagent-bridge/pfillhniocmjcapelhjcianojmoidjdk) plug-in for Google Chrome browser.

## Validation

### Raw data

```
pi@raspberrypi:/run/tmp/dump978-master $ rtl_sdr -f 915000000 -s 2083334 -g 8 - | ./dump978
Found 1 device(s):
  0:  Generic, RTL2832U, SN: 77771111153705700

Using device 0: Generic RTL2832U
Found Rafael Micro R820T tuner
Exact sample rate is: 2083334.141630 Hz
[R82XX] PLL not locked!
Sampling at 2083334 S/s.
Tuned to 915000000 Hz.
Tuner gain set to 7.70 dB.
Reading samples in async mode...
-0d1abba154d8ec198ba602f0800000000074c28d855bfd0b4aa5c2a0000000000000;
...
```

### Text

```
pi@raspberrypi:/run/tmp/dump978-master $ rtl_sdr -f 915000000 -s 2083334 -g 8 - | ./dump978 | ./uat2text
Found 1 device(s):
  0:  Generic, RTL2832U, SN: 77771111153705700

Using device 0: Generic RTL2832U
Found Rafael Micro R820T tuner
Exact sample rate is: 2083334.141630 Hz
[R82XX] PLL not locked!
Sampling at 2083334 S/s.
Tuned to 915000000 Hz.
Tuner gain set to 7.70 dB.
Reading samples in async mode...
HDR:
 MDB Type:          1
 Address:           1ABBA1 (Fixed ADS-B Beacon Address)
SV:
 NIC:               0
 Latitude:          +59.6583
 Longitude:         +17.9617
 Altitude:          150 ft (barometric)
 Dimensions:        15.0m L x 11.5m W
 UTC coupling:      no
 TIS-B site ID:     0
MS:
 Emitter category:  Service vehicle
 Callsign:          RAMPTEST
 Emergency status:  No emergency
 UAT version:       2
 SIL:               3
 Transmit MSO:      18
 NACp:              10
 NACv:              2
 NICbaro:           1
 Capabilities:      CDTI ACAS
 Active modes:
 Target track type: true heading
AUXSV:
 Sec. altitude:     unavailable
...
```

### Map

```
pi@raspberrypi:/run/tmp $ rtl_sdr -f 915000000 -s 2083334 -g 8 - | ./dump978-master/dump978 | ./dump978-master/uat2json /run/tmp/dump1090-master/public_html/data/
Found 1 device(s):
  0:  Generic, RTL2832U, SN: 77771111153705700

Using device 0: Generic RTL2832U
Found Rafael Micro R820T tuner
Exact sample rate is: 2083334.141630 Hz
[R82XX] PLL not locked!
Sampling at 2083334 S/s.
Tuned to 915000000 Hz.
Tuner gain set to 7.70 dB.
Reading samples in async mode...
...
```

<img src="https://github.com/lyusupov/UAT-test-signal/raw/master/notes/pics/dump978.jpg" height="418" width="674">
<br>

### ADS-B receiver


![](https://github.com/lyusupov/SoftRF/raw/master/documents/images/UATbridge_Stratux.JPG)
<br>

## Signal

[IQ file](https://github.com/lyusupov/UAT-test-signal/raw/master/notes/UAT_977MHz-8_333_MSps.wav) (WAV).<br>
Taken at base frequency: 977 MHz . Sampling rate: 8.333 MSps.

![](https://github.com/lyusupov/UAT-test-signal/raw/master/notes/pics/UAT_Signal.JPG)



![](https://github.com/lyusupov/UAT-test-signal/raw/master/notes/pics/UAT_Spectrum.JPG)



## Data customization

1. Execute *UATEncoder.py* script with `<CallSign>` `<ICAO>` `<Latitude>` `<Longitude>` `<Altitude>` arguments:
```
$ python UATEncoder.py RAMPTEST 0x1ABBA1 59.6583 17.9617 137.8

#define UAT_DATA    "0d1abba154d8ec198ba602f0800000000074c28d855bfd0b4aa5c2a0000000000000"


```

2. Edit *UAT_data.h* file:
```
#ifndef UAT_DATA

#define UAT_DATA    "0d1abba154d8ec198ba602f0800000000074c28d855bfd0b4aa5c2a0000000000000"

#endif
```

3. Build your custom firmware and download it into your signal generator hardware.

