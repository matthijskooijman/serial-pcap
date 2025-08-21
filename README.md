serial-pcap
===========
This script is intended to read raw packets (currently only 802.15.4
packets prefixed by a length byte) from a serial port and output them
in pcap format.

You'll need the `pyserial` package.
You can install it through pip using `pip install pyserial`.

Default arguments are currently aimed at capturing packets using an
Atmega256rfr2 microcontroller (with on-chip radio) running the
[At256rfr2Sniffer](https://github.com/matthijskooijman/At256rfr2Sniffer)
Arduino sketch.

When running that sketch you can just run:

    serial-pcap /dev/ttyACM0

and see packets (raw byte values) on stdout.

For all options supported, run `serial-pcap --help`.

To customize the channel and bitrate values, commands can be sent over
Serial. See the [At256rfr2Sniffer
README](https://github.com/matthijskooijman/At256rfr2Sniffer) for
details.

Live capture with wireshark
---------------------------
[Wireshark][] can be used to view the captured packets as they come in.
To enable this, run the script with the `--fifo` option:

	serial-pcap --fifo /tmp/wireshark /dev/ttyACM0

This creates a FIFO at `/tmp/wireshark` and waits for Wireshark to open
the fifo.

Then, start Wireshark and configure it to use the fifo (Capture ->
Options... -> Manage Interfaces). Once you start the capture in
Wireshark, serial-pcap will open the serial interface and start
capturing packets.

[Wireshark]: http://www.wireshark.org

Operating systems
-----------------
This script was developed and tested on Linux. It probably runs on OSX
too. On Windows, running the live capture through a FIFO probably does
not work, but capturing to a file probably does.

Decoding LwMesh packets in wireshark
------------------------------------
This is a bit of specific advice, but since the pinoccio board that this
software was originally written for uses the LwMesh protocol, it is
added here. You can likely ignore it otherwise.

To let Wireshark decode LwMesh packets, configure the key under
Protocols in the preferences. The format for it is colon-separated hex
values, so for the default pinoccio key of all ones, enter
`ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff`.

Related projects
----------------
This script was inspired by [WSBridge][], which serves a similar
purpose. With proper options passed, this script should be usable
instead of WSBridge with hardware running the [Chibi][] stack from
Freaklabs.

[WSBridge]: http://www.freaklabs.org/index.php/WSBridge.html
[Chibi]: http://www.freaklabs.org/index.php/Chibi-A-Simple-Open-Source-Wireless-Stack.html

This script should also be usable with the Contiki-OS [sensniff example
program][Contiki-sensniff] to sniff using Contiki-supported hardware
(again, when passed the right commandline options, and maybe only with
the legacy serial format).

[Contiki-sensniff]: https://github.com/cetic/contiki/tree/master/examples/sensniff

Neither of these two options were tested.

This script was originally created at
https://github.com/Pinoccio/tool-serial-pcap and intended to be used
with the Pinoccio dev board and firmware. Development on Pinoccio has
since halted, so in 2025 this repo was moved here and slightly adapted
to work with the more generic At256rfr2Sniffer sketch instead of the
Pinoccio firmware.

License
-------
This script and related content is licensed under the MIT license.

Copyright (c) 2012-2014, Pinoccio Inc. All rights reserved.

Copyright (c) 2025, Matthijs Kooijman <matthijs@stdin.nl>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
