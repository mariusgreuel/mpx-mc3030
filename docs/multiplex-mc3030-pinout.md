# Multiplex PROFI mc 3030 Pinout

## Radio Connectors

### DIN Connector

The Multiplex PROFI mc 3030 uses a 7-pin DIN connector.

[![DIN Connector](./din-connector.svg)](./din-connector.svg)

**Pin Assignments**

Pin | Name
----|-----
1   | Charger Input
2   | +V_on, ~6V-9V
3   | GND (brown)
4   | GPIO D4 (white)
5   | GPIO D5 (yellow)
6   | n.c.
7   | GPIO D7 (green)

**Modes**

The Multiplex Profi mc3030 has five modes of operation,
which are selected by grounding the respective GPIO pins.
The table below shows the pin assignments for each mode.

Mode          | D4  | D5  | D7  | Notes
--------------|-----|-----|-----|-------
Data Exchange | TxD |     | RxD | Serial port for data exchange with other radio or PC.
Teacher In    | PPM |     |     | Teacher input mode (Teacher), D4 receives PPM signal.
Teacher Out   | PPM | GND |     | Teacher output mode (Student), D4 transmits PPM signal, disabled HF module.
Rev Counter   | In  |     | GND | Revolution counter sensor connected to D4.
Service       |     | GND | GND | Service mode for personalization of the radio.

### Main Board Connector

Pin | Name
----|-----
1   | GND
2   | +V_on
3   | PPM Output
4   | GPIO D5
5   | GPIO D4
6   | GPIO D7

### HF Module Connector

Pin | Name
----|-----
1   | Antenna
2   | Data (HFM4 only)
3   | GND
4   | +V_on
5   | RF enable (via GPIO D5)
6   | PPM Input

## See Also

- [Multiplex Flash Tool](../)
- [Multiplex PPM Format](./multiplex-ppm-spec.md)
- [Multiplex PCM Format](./multiplex-pcm-spec.md)

## License

[GNU GPLv3](./LICENSE)

Copyright (C) 2018 Marius Greuel.

**Disclaimer**

Multiplex Profi mc3030 is a registered trademark of Multiplex Modellsport GmbH, Germany.
Multiplex Modellsport GmbH is not affiliated with this project, nor endorses it.
