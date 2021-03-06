
# arduino-psa-diag
Arduino sketch to send diagnostic frames to PSA cars

## How to use
The serial console is used to send raw diagnostic frames, start it using 115200 baud rate

## Choose ECU
You have to choose the ECU you want to communicate with by inputing its diagnostic frame IDs in hexadecimal:

    >CAN_EMIT_ID:CAN_RECV_ID
Telematic unit access example:

    >764:664
Check out [ECU_LIST.md](https://github.com/ludwig-v/arduino-psa-diag/blob/master/ECU_LIST.md) inside the repo for other ECUs IDs pair

## Easy Unlock
Once you are on the chosen ECU you can unlock writing easily using this shortcut:

    :UNLOCK_KEY:UNLOCK_SERVICE:DIAG_SESSION_ID
NAC Telematic unit (**UDS**) unlock example:

    :D91C:03:03
AMPLI_AUDIO Amplifier unit (**KWP**) unlock example:

    :A7D8:83:C0
Check out [ECU_KEYS.md](https://github.com/ludwig-v/psa-seedkey-algorithm/blob/main/ECU_KEYS.md) for other ECUs keys

## UDS Commands

| Command | Description |
|--|--|
| 3E00 | Keep-Alive session |
| 190209 | List of current faults |
| 14FFFFFF | Clear faults |
| 37 | Flash autocontrol (Unit must be unlocked first) |
| 1001 | End of communication |
| 1002 | Open Download session |
| 1003 | Open Diagnostic session |
| 1103 | Reboot |
| 2701 | Unlocking service for download (Diagnostic session must be enabled first) - SEED |
| 2703 | Unlocking service for configuration (Diagnostic session must be enabled first) - SEED |
| 2702XXXXXXXX  | Unlocking response for download - XXXXXXXX = KEY - Must be given within 5 seconds after seed generation |
| 2704XXXXXXXX  | Unlocking response for configuration - XXXXXXXX = KEY - Must be given within 5 seconds after seed generation |
| 22XXXX | Read Zone XXXX (2 bytes) |
| 2EXXXXYYYYYYYYYYYY | Write Zone XXXX with data YYYYYYYYYYYY (Unit must be unlocked first) |
| 3101FF0081F05A | Empty flash memory for .cal upload (Unit must be unlocked first) |
| 3101FF0082F05A | Empty flash memory for .ulp upload (Unit must be unlocked first) - WARNING: If you don't upload a valid file your device will be temporarily bricked |
| 3103FF00 | Empty flash memory (Unit must be unlocked first) |
| 3481110000 | Prepare flash writing for .cal upload (Unit must be unlocked first) |
| 3482110000 | Prepare flash writing for .ulp upload (Unit must be unlocked first) |
| 3101FF04 | Empty ZI Zone (Unit must be unlocked first) |
| 3103FF04 | Empty ZI Zone (Unit must be unlocked first) |
| 3483110000 | Prepare ZI zone writing (Unit must be unlocked first) |

## KWP2000 Commands

| Command | Description |
|--|--|
| 3E | Keep-Alive session |
| 17FF00 | List of current faults |
| 14FF00 | Clear faults |
| 1081 | End of communication |
| 10C0 | Open Diagnostic session |
| 31A800 | Reboot |
| 31A801 | Reboot 2 |
| 2781 | Unlocking service for download (Diagnostic session must be enabled first) - SEED |
| 2783 | Unlocking service for configuration (Diagnostic session must be enabled first) - SEED |
| 2782XXXXXXXX  | Unlocking response for download - XXXXXXXX = KEY - Must be given within 5 seconds after seed generation |
| 2784XXXXXXXX  | Unlocking response for configuration - XXXXXXXX = KEY - Must be given within 5 seconds after seed generation |
| 21XX | Read Zone XX (1 byte) |
| 3BXXYYYYYYYYYYYY | Write Zone XX with data YYYYYYYYYYYY (Unit must be unlocked first) |
| 318181F05A | Empty flash memory (Unit must be unlocked first) |
| 318101 | Empty flash memory (Unit must be unlocked first) |
| 348100000D07 | Prepare flash writing (Unit must be unlocked first) |

## UDS Answers

| Answer | Description |
|--|--|
| 7E00 | Keep-Alive reply |
| 54 | Faults cleared |
| 7103FF0001 | Flash erasing in progress |
| 7103FF0003 | Flash not erased: error |
| 7101FF0401 | ZI erased successfully |
| 7103FF0402 | ZI erasing in progress |
| 7103FF0403 | ZI not erased: error |
| 77 | Flash autocontrol OK |
| 5001XXXXXXXX | Communication closed |
| 5002XXXXXXXX | Download session opened |
| 5003XXXXXXXX | Diagnostic session opened |
| 5103 | Reboot OK |
| 62XXXXYYYYYYYYYYYY  | Successfull read of Zone XXXX - YYYYYYYYYYYY = DATA |
| 6701XXXXXXXX | Seed generated for download - XXXXXXXX = SEED |
| 6703XXXXXXXX | Seed generated for configuration - XXXXXXXX = SEED |
| 6702 | Unlocked successfully for download - Unit will be locked again if no command is issued within 5 seconds |
| 6704 | Unlocked successfully for configuration - Unit will be locked again if no command is issued within 5 seconds |
| 6EXXXX | Successfull Configuration Write of Zone XXXX |
| 741000 | Download Writing ready |
| 76XX02 | Download frame XX injected with success |
| 76XX0A | Invalid checksum on download frame XX |
| 7F3478 | Download Writing in progress |
| 7F3778 | Flash autocontrol in progress |
| 7FXX78 | In progress |
| 7F2231 | Failed Configuration Read - Not allowed operation |
| 7F2724 | Anti-Bruteforce active |
| 7F2713 | Invalid SEED Answer (KEY) |
| 7F2E78 | Configuration Write in progress |
| 7F2E13 | Failed Configuration Write - Invalid Zone data |
| 7F2E7E | Failed Configuration Write - Unit is locked |
| 7F2E31 | Failed Configuration Write - Not allowed operation |
| 7F2EXX | Failed Configuration Write |
| 7F2E31 | Failed Configuration Read - Not allowed operation |
| 7F22XX | Failed Configuration Read |
| 7FXXYY | Error - XX = Service / YY = Error Number |

## KWP2000 Answers

| Answer | Description |
|--|--|
| 7E | Keep-Alive reply |
| 5081 | Communication closed |
| 50C0 | Diagnostic session opened |
| 71A801 | Reboot |
| 71A802 | Reboot 2 |
| 61XXYYYYYYYYYYYY  | Successfull read of Zone XX - YYYYYYYYYYYY = DATA |
| 6781XXXXXXXX | Seed generated for download - XXXXXXXX = SEED |
| 6783XXXXXXXX | Seed generated for configuration - XXXXXXXX = SEED |
| 6782 | Unlocked successfully for download - Unit will be locked again if no command is issued within 5 seconds |
| 6784 | Unlocked successfully for configuration - Unit will be locked again if no command is issued within 5 seconds |

## PSA Seed/Key algorithm

[Algorithm can be found here with some example source code](https://github.com/ludwig-v/psa-seedkey-algorithm)

## Diagnostic frames explanation / What the Sketch is doing

CAN-BUS is limited to 8 bytes per frame, to send larger data PSA chose a simple algorythm to truncate the data into multiple parts

### To send data smaller or equal to 7 bytes:
#### [SEND] Frame:
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| Full Data Length | Data[0] | Data[1] | Data[2] | Data[3] | Data[4] | Data[5] | Data[6] |

### To send data larger than 7 bytes:
#### [SEND] First Frame:
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| 0x10 | Full Data Length | Data[0] | Data[1] | Data[2] | Data[3] | Data[4] | Data[5] |
#### [RECEIVE] Write Acknowledgement Frame:
| Byte 1 | Byte 2 | Byte 3 |
|--|--|--|
| 0x30 | 0x00 | 0x0A |
#### [SEND] Second Frame:
##### ID starting at 0x21 and increasing by 1 for every extra frame needed, after 0x2F reverting back to 0x20
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| 0x21 | Data[6] | Data[7] | Data[8] | Data[9] | Data[10] | Data[11] | Data[12] |
#### [SEND] Third Frame:
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| 0x22 | Data[13] | Data[14] | Data[15] | Data[16] | Data[17] | Data[18] | Data[19] |

### To receive data smaller or equal to 7 bytes:
#### [RECEIVE] Frame:
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| Full Data Length | Data[0] | Data[1] | Data[2] | Data[3] | Data[4] | Data[5] | Data[6] |

### To receive data larger than 7 bytes:
#### [RECEIVE] First Frame:
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| 0x10 | Full Data Length | Data[0] | Data[1] | Data[2] | Data[3] | Data[4] | Data[5] |
#### [SEND] Read Acknowledgement Frame:
| Byte 1 | Byte 2 | Byte 3 |
|--|--|--|
| 0x30 | 0x00 | 0x05 |
#### [SEND] Second Frame:
##### ID starting at 0x21 and increasing by 1 for every extra frame needed, after 0x2F reverting back to 0x20
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| 0x21 | Data[6] | Data[7] | Data[8] | Data[9] | Data[10] | Data[11] | Data[12] |
#### [SEND] Third Frame:
| Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | Byte 8 |
|--|--|--|--|--|--|--|--|
| 0x22 | Data[13] | Data[14] | Data[15] | Data[16] | Data[17] | Data[18] | Data[19] |

> Received frames could be out-of-order, ID must be used to append parts at the correct position into the final data whose size is known

## Calibration file (.cal / .ulp) explanation

Every line of calibration files has this form:

### Content Size
| TYPE | LENGTH | ADDRESS | LENGTH2 | ZONE | DATA | CHECKSUM | CHECKSUM2 |
|--|--|--|--|--|--|--|--|
| 1h | 1h | Variable Length | 1h | 2h | Variable Length | 2h| 1h |

1h = 1 HEX Byte or 2 characters in the .cal file

### Content Data
| Line Part | Line Detail |
|--|--|
| **TYPE** | S1 = ZI Zone / S2 / S8 |
| **LENGTH** | Hex Length of ADDRESS+ZONE+DATA+CHECKSUM+CHECKSUM2 |
| **ADDRESS** |  |
| **LENGTH2** | Hex Length of ZONE+DATA+CHECKSUM  |
| **ZONE** |  |
| **DATA** |   |
| **CHECKSUM** | *CRC-16/X-25*(DATA) with this order CRC[1] CRC[0] |
| **CHECKSUM2** | *CRC-8/2s_complement*(ADDRESS+ZONE+DATA+CHECKSUM) - 1 |

## Dump Mode

This sketch also provides a way to dump Diagbox frames (read & write) very easily, it is merging split CAN-BUS frames together to have a clean and "human readable" output. Just enable it this way in the source code:

    bool Dump = true;
	
You can also use the serial console typing:

    X
	
To revert into normal mode:

    N

### Example
You can build yourself that kind of cable using an OBD2 Extender cable to use both the Arduino and PSA Interface:
![Dump cable](https://i.imgur.com/UVQRsyr.png)
#### PINOUT

| PIN | Description |
|--|--|
| 3 | CAN-BUS Diagnostic High |
| 8 | CAN-BUS Diagnostic Low |

![OBD2 PINOUT](https://i.imgur.com/sWJF8gg.png)

> Reminder: In Dump mode the termination resistor should not be activated
