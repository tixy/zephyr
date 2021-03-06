.. _dns-resolve-sample:

DNS Resolve Application
#######################

Overview
********

The DNS resolver sample application implements a basic DNS resolver according
to RFC 1035. Supported DNS answers are IPv4/IPv6 addresses and CNAME.

If a CNAME is received, the DNS resolver will create another DNS query.
The number of additional queries is controlled by the
DNS_RESOLVER_ADDITIONAL_QUERIES Kconfig variable.

For more information about DNS configuration variables, see:
:file:`subsys/net/lib/dns/Kconfig`. The DNS resolver API can be found at
:file:`include/net/dns_resolve.h`. The sample code can be found at:
:file:`samples/net/dns_resolve`.

Requirements
************

- :ref:`networking with Qemu <networking_with_qemu>`

- screen terminal emulator or equivalent.

- For the Arduino 101 board, the ENC28J60 Ethernet module is required.

- dnsmasq application. The dnsmasq version used in this sample is:

.. code-block:: console

    dnsmasq -v
    Dnsmasq version 2.76  Copyright (c) 2000-2016 Simon Kelley


Wiring
******

The ENC28J60 module is an Ethernet device with SPI interface.
The following pins must be connected from the ENC28J60 device to the
Arduino 101 board:

===========	===================================
Arduino 101	ENC28J60 (pin numbers on the board)
===========	===================================
D13		SCK  (1)
D12		SO   (3)
D11		SI   (2)
D10		CS   (7)
D04		INT  (5)
3.3V		VCC  (10)
GDN		GND  (9)
===========	===================================


Building and Running
********************

Network Configuration
=====================

Open the project configuration file for your platform, for example:
:file:`prj_frdm_k64f.conf` is the configuration file for the
:ref:`frdm_k64f` board.

In this sample application, both static or DHCPv4 IP addresses are supported.
Static IP addresses are specified in the project configuration file,
for example:

.. code-block:: console

	CONFIG_NET_APP_MY_IPV6_ADDR="2001:db8::1"
	CONFIG_NET_APP_PEER_IPV6_ADDR="2001:db8::2"


are the IPv6 addresses for the DNS client running Zephyr and the DNS server,
respectively.

DNS server
==========

The dnsmasq tool may be used for testing purposes. Sample dnsmasq start
script can be found in net-tools project.

The net-tools can be downloaded from

    https://gerrit.zephyrproject.org/r/gitweb?p=net-tools.git;a=blob;f=README


Open a terminal window and type:

.. code-block:: console

    $ cd net-tools
    $ ./dnsmasq.sh


NOTE: some systems may require root privileges to run dnsmaq, use sudo or su.

If dnsmasq fails to start with an error like this:

.. code-block:: console

    dnsmasq: failed to create listening socket for port 5353: Address already in use


Open a terminal window and type:

.. code-block:: console

    $ killall -s KILL dnsmasq


Try to launch the dnsmasq application again.


QEMU x86
========

Open a terminal window and type:

.. code-block:: console

    $ make


Run 'loop_socat.sh' and 'loop-slip-tap.sh' as indicated at:

    https://gerrit.zephyrproject.org/r/gitweb?p=net-tools.git;a=blob;f=README


Open a terminal where the project was build (i.e. :file:`samples/net/dns_resolve`) and type:

.. code-block:: console

    $ make run


FRDM K64F
=========

Open a terminal window and type:

.. code-block:: console

    $ make BOARD=frdm_k64f


The FRDM K64F board is detected as a USB storage device. The board
must be mounted (i.e. to /mnt) to 'flash' the binary:

.. code-block:: console

    $ cp outdir/frdm_k64f/zephyr.bin /mnt


See :ref:`Freedom-K64F board documentation <frdm_k64f>` for more information
about this board.

Open a terminal window and type:

.. code-block:: console

    $ screen /dev/ttyACM0 115200


Use 'dmesg' to find the right USB device.

Once the binary is loaded into the FRDM board, press the RESET button.

Arduino 101
===========

Open a terminal window and type:

.. code-block:: console

	$ make BOARD=arduino_101


To load the binary in the development board follow the steps
in :ref:`arduino_101`.

Open a terminal window and type:

.. code-block:: console

    $ screen /dev/ttyUSB0 115200


Use 'dmesg' to find the right USB device.

Once the binary is loaded into the Arduino 101 board, press the RESET button.
