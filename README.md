# Point Controller for Raillway Model

[日本語版READMEはこちら](./README-ja.md)

## Overview
A 16-channel point controller for railway models. The drive voltage for the points is assumed to be +12V.

It supports the following functionalities exclusively:
- Control via manual switches
- Control via an external SPI interface

###  Control via SPI
- Setting the `/SPI_MODE` input to Low activates the SPI control mode. The manual switch becomes inactive, and the Manual Mode LED turns off.
- Operates as a Slave Device in SPI Mode 0.
- Data format is 16-bit MSB First (CH16 is the MSB).
- Upon data transfer, the output to the points is updated at the rising edge of `SS` signal.
- Refer to [C103](#c103) for the frequency of `SS` signal.

## Demo
Example of driving TOMIX N-gauge electric points with an AVR microcontroller (video)

[![](https://img.youtube.com/vi/Y3gJkpfgWYg/0.jpg)](https://www.youtube.com/watch?v=Y3gJkpfgWYg)

## Schematic Diagram

### SPI Connector Specifications
| Pin| Signal    |
|----|-----------|
|  1 | +5V       |
|  2 | +5V       |
|  3 | (N.C)     |
|  4 | MOSI      |
|  5 | (N.C)     |
|  6 | SCLK      |
|  7 | /SPI_MODE |
|  8 | SS        |
|  9 | GND       |
| 10 | GND       |

### C103
Upon power-up, if the output Qn of 74HC595 is undefined, the relay turns on.
To prevent this, C103 is used to delay the rise of `RCK` from power-on and set the output to Low.
It's an RC circuit of 22K and 1uF, so the charging time is about 30ms at maximum. Hence, if controlling via SPI, the drive period should be longer than this.

### R105
When `/SPI_MODE` terminal is open, the `/OE` of 74HC595 needs to be fixed at High (>3.15V). The resistor between the input and GND of DTC124E is 44KΩ (input and bias resistors are 22KΩ each). As these are connected in parallel, the voltage of `/OE` becomes a value divided from 5V by R105 and 22KΩ (=44KΩ//44KΩ).
Therefore, it is necessary that R105 * 5 / (R105 + 22K) > 3.15.

## Board
### Prototype
![](https://github.com/46nori/PointController/blob/images/Prototype.jpeg)

### Printed Circuit Board (Not Manufactured)
![](https://github.com/46nori/PointController/blob/images/PCB-Rev1.0-3d.jpeg)  
![](https://github.com/46nori/PointController/blob/images/PCB-Rev1.0-image.jpeg)  

## Disclaimer
The information provided in this repository absolves the information providers of any responsibility for any damage caused by utilizing the information contained herein.

## License
GPL Version 3