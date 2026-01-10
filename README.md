# Multiplex Profi mc 3030 Resources

This repository contains a number of tools and inofficial specifications related to the **Multiplex Profi mc 3030** radio.

## Multiplex Profi mc 3030 Tool

The mc3030 Tool is a tool that allows you to access the Multiplex Profi mc 3030 from a PC. 

Features include:

- ID the radio.
- Backup and restore single model memories.
- Backup and restore the data RAM.
- Backup and restore the entire RAM.
- Analyse changes to a RAM block.

### Usage

To use this tool, you need to connect the Profi mc 3030 to your PC via a USB Serial Convertor. Any USB to TTL UART, such as a FTDI FT232R serial convertor, will do. See **Serial Port Connection** below for details on how to connect the serial cable.

To verify that the Profi mc 3030 is properly connected to your PC, run `mc3030 -port COMx -id` from a command-prompt. Replace **COMx** with the proper number of your serial adapter.

On success, the following text should be displayed:

```text
Multiplex PROFI mc 3030 Tool, V1.1
Copyright (C) 2018 Marius Greuel. All rights reserved.
Opening COM port 'COM3'...
Reading block 115: Received 257 bytes, Code: 0x06, Checksum: 0xC2 (OK)
Reading block 127: Received 257 bytes, Code: 0x06, Checksum: 0xA7 (OK)

Memory Size: 15
Owner: '  PROFI MC3030  '
V3.7 Text D 3.5
Montage: 000000
Nummer :   0000
Service: ______
```

### Example commands

To display the usage of this tool, run `mc3030` from a command-prompt:

```text
Multiplex PROFI mc 3030 Tool, V1.1
Copyright (C) 2018 Marius Greuel. All rights reserved.
Usage: mc3030 [@response-file] [options] <files>
    -Port <COMx>        Specifies the COM port to be used
    -Id                 Print the transmitter ID block
    -Dump <block>       Dumps the specified block to the console
    -Diff <block>       Repeatatly load the specified block and print changes to the console
    -Save <filename>    Save data from the MC3030 to the specified file
    -Load <filename>    Load the specified file and transfer the data to the MC3030
    -Memory <memory>    Specifies the model memory to be loaded/saved
    -Block <block>      Specifies the block to be saved
    -FirstBlock <block> Specifies the first block to be saved
    -LastBlock <block>  Specifies the last block to be saved
    -Help               Display full help

Examples:
    Print device ID: mc3030 -port COM3 -id
    Save model memory #1 to file: mc3030 -port COM3 -memory 1 -save mc3030-model-1.bin
    Load model memory #1 from file: mc3030 -port COM3 -memory 1 -load mc3030-model-1.bin
    Dump info block 0x73: mc3030 -port COM3 -dump 0x73
    Backup data memory to file: mc3030 -port COM3 -first 0x70 -last 0x7F -save mc3030-dataset.bin
    Restore data memory from file: mc3030 -port COM3 -load mc3030-dataset.bin
    Backup entire memory to file: mc3030 -port COM3 -save mc3030-full-backup.bin
    Restore entire memory from file: mc3030 -port COM3 -load mc3030-full-backup.bin
```

### Downloads

To download a pre-build executable, go to the [releases](https://github.com/mariusgreuel/mpx-mc3030/releases) folder.

### Prerequisites

- On Windows, you need Windows 10 or higher with the .NET Framework installed.
- On Linux or macOS, you need Mono to be installed. You can execute **mc3030** by running `mono mc3030.exe`.

### Building the software

This software is written in C# using Visual Studio 2022.

### Serial Port Connection

The microcontroller of the mc 3030 has a TTL (5V) serial port connected to the radios DIN connector.

The serial port is operated at 9600,N,8,1.

For the pinout, see [Multiplex PROFI mc 3030 Pinout](./docs/multiplex-mc3030-pinout.md#din-connector)

### Technical Details

The Profi mc3030 has a battery backed RAM via a CR2450 coin cell, where the radio stores the model data.

Typical RAM sizes are 8KByte and 32KByte, which is able to hold 15 or 99 models.

Besides model data, the RAM also holds firmware data, which includes localized user interface strings, and a number of unknown data. The RAM firmware data cannot be initialized from the ROM, which means the radio requires service when RAM memory corruption occurs. If you suspect a near empty buffer battery, or you indent to replace the buffer battery, be sure to backup the RAM before.

### Memory Layout

Memory is organized in blocks, each 256 bytes in size. The total number of blocks is model dependent.

An 8KByte RAM has 32 blocks, a 32KByte RAM has 128 blocks. Note that the block numbers are modulo the RAM size, i.e. for the 8K RAM, if you address block 112 (0x70), you will access block 16 (0x10).

The last 16 blocks are reserved for firmware data.

Nr. | Description 
----|------------
0-N | N+1 Model memories
112 | Model memories extentions, system data
... | ...
127 | 

### Memory Read Command

To read memory, the following sequence is used:
- PC: Send BLOCK, 0xCF, 0xCF
- Radio: Responds with 0x06 (ready).
- PC: Send 0x14 (send data)
- Radio: Transmits 256 bytes, plus one byte checksum (XOR over 256 bytes)

### Memory Write Command

To write memory, the following sequence is used:
- PC: Send BLOCK, 0x8F, 0x8F
- Radio: Responds with 0x06 (ready).
- PC: Send 256 bytes, plus one byte checksum (XOR over 256 bytes)
- Radio: Responds with 0x14, 0x06 (ready).

## Multiplex Profi mc 3030 Connectors

For the connector pinouts of the radio, see [Multiplex PROFI mc 3030 Pinout](./docs/multiplex-mc3030-pinout.md)

## Multiplex PPM Format Specification

This document is a specification of the Multiplex PPM format. It is used for RC radio transmitters to represent channel values in a pulse-width modulated form and to transmit it to the receiver.

This specification was derived from a PPM signal as generated by a Multiplex Profi mc 3030 radio.

- [Multiplex PPM Format Specification](./docs/multiplex-ppm-spec.md)

## Multiplex PCM Format Specification

This document is a specification of the Multiplex PCM format, which was introduced around the year 2007. It is used for RC radio transmitters to encode channel values in a digital form (as opposed to the analog PPM format).

This specification was derived from a PCM signal as generated by a Multiplex Profi mc 3030 radio.

- [Multiplex PCM Format Specification](./docs/multiplex-pcm-spec.md)

## License

[GNU GPLv3](./LICENSE)

Copyright (C) 2018 Marius Greuel.
