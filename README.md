CAN driver for Toucan adapter
====

All examples are based on Ubuntu 18.04.2 LTS


Building kernel module
----

Install build tools:

    sudo apt install build-essential

Compile:

    make

Install module to system:

    sudo make install
    sudo depmod


Build DKMS package for Ubuntu
----

After this, your system will automatically rebuild TouCAN module when you upgrade your Linux kernel.

    sudo apt install git dkms
    git archive --prefix=toucan-1.0/ -o toucan-1.0.tar HEAD
    sudo tar -xf toucan-1.0.tar -C /usr/src/
    sudo dkms add -m toucan -v 1.0 --verbose
    sudo dkms build -m toucan -v 1.0 --verbose
    sudo dkms install -m toucan -v 1.0 --verbose


Set up interface
----

    sudo modprobe toucan
    sudo ip link set can0 up type can bitrate 125000


Shut down interface
----

    sudo ip link set can0 down
    sudo rmmod toucan


Send and receive data
----

    sudo add-apt-repository universe
    sudo apt install can-utils
    # Send packet
    cansend can0 123#
    # Listen for packets
    can dump can0

Set-up persistent interface names
----

Edit `95-toucan.rules` file - change `ATTRS{serial}` value to match your device's serial number and `NAME` to desired interface name

    sudo cp 95-toucan.rules /etc/udev/rules.d/

