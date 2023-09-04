# RCXAZ Air Quality Sensors

This device is easy to inspect with the Android nRF Connect app. Find a device with a name starting with _XS-_. The 9-in-1 sensor is named _XD-281E_. XConnecting to the ddevice lets you inspect services and characteristics.

## Available [BLE Services](https://devzone.nordicsemi.com/guides/short-range-guides/b/bluetooth-low-energy/posts/ble-services-a-beginners-tutorial)

### 1800 = _Generic Access_ Service
0x2A00 - _Device Name_ Characteristic

### 1801 = _Generic Attribute_ Service
0x2A05 - _Service Changed_ Characteristic

### C760 = RCXAZ Service
C761 - RCXAZ Air Quality Characteristics

It appears that RCXAZ Air Quality devices may implement multiple sensors, but will broadcast them all under the same service/characteristic.

Different sensor data is sent through different message types. The messages can be identified by the first four bytes.

Devices will implement a combination of message types based on the included sensors and send the relevant messages for them.

#### Request Types

| Message Identifier | Sensor Content | Expected Request Length | Item 1 | Item 1 Unit of Measurement | Item 1 Index | Item 2 | Item 2 Unit of Measurement | Item 2 Index | Item 3 | Item 3 Unit of Measurement | Item 3 Index |
|--|--|--|--|--|--|--|--|--|--|--|--|
| 23 08 10 04 | <ul><li>CO₂</li><li>TVOC</li><li>HVOC</li></ul> | 11 bytes | CO₂ | ppm | 4 | TVOC | mg/m³ | 6 | HVOC | mg/m³ | 8 |
| 23 08 10 07 | <ul><li>PM1.0</li><li>PM2.5</li><li>PM10</li></ul> | 11 bytes | PM1.0 | µg/m³  | 4 | PM2.5 | µg/m³ | 6 | PM10 | µg/m³ | 8 
| 23 06 10 07 | <ul><li>Temp</li><li>Relative Humidity</li></ul> | 9 bytes | Temp | 1⁄10 ° C/F | 4 | Humidity | % | 6 |  |  |  |


C762 - RCXAZ configuration service (wrte-only) - unknown format

#### Sample Requests

| Message Data | Message Content |
|--------------|-----------------|
| 23 08 10 04 -  01 9D - 00 06 - 00 04 - FE | 413ppm CO₂ ︱ 6 mg/m3 HCHO ︱ 4 mg/m3 TVOC ︱ unknown byte |
| 23 08 10 07 -  00 02 - 00 06 - 00 11 - 7E | 2 µg/m³ PM1.0 ︱ 6 µg/m³ PM2.5 ︱ 11 µg/m³ PM10 ︱ unknown byte |
| 23 06 10 04 -  00 CC - 00 2A - 43 | 20.4°C ︱ 42% Relative Humidity ︱ unknown byte |
### f000ffc0-0451-4000-b000-000000000000	=	(OAD Service - [Over the Air Download](https://software-dl.ti.com/simplelink/esd/simplelink_cc13x0_sdk/1.40.00.10/exports/docs/blestack/software-developers-guide/cc1350/oad/oad.html))

f000ffc1-0451-4000-b000-000000000000	-	_Image Identify_ Characteristic, lets the OAD target device accept/reject proposed images.
f000ffc2-0451-4000-b000-000000000000	-	_Image Block_ Characteristic, sends the actual block of image data to the OAD target device.