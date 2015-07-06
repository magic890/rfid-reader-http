# rfid-reader-http

PC/SC lite HTTP wrapper for reading from a card reader (RFID &amp; NFC).

## Tested card reader

* ACS ACR122 (ACR122U-A9)

## Prerequisites

For running the installation step, due to its dependency from [node-pcsclite](https://github.com/santigimeno/node-pcsclite), this project requires [node-gyp](https://github.com/TooTallNate/node-gyp), so please read carefully and satisfy their [installation requirements](https://github.com/TooTallNate/node-gyp#installation), otherwise it may fails compiling the node module.

You may have to install the proper driver for your card reader.

   * [ACS ACR122U drivers](http://www.acs.com.hk/en/driver/3/acr122u-usb-nfc-reader/)

### Requirements installation

#### Mac OS X

1. Download the ACS ACR122U Drivers for Mac OS X 10.x via the [download page](http://www.acs.com.hk/en/driver/3/acr122u-usb-nfc-reader/) or via direct link for [PC/SC Driver Installer 1.1.0 (Mac 10.5, Mac 10.6, Mac 10.7, Mac 10.8, Mac 10.9, Mac 10.10) (2014-09-17 306KB)](http://www.acs.com.hk/download-driver-unified/5133/ACS-Unified-INST-MacOSX-110-P.zip). You have to install it even if the installer is not properly signed.

    It will install the useful driver in: `/usr/libexec/SmartCardServices/drivers/ifd-acsccid.bundle`

#### Raspberry Pi (ARM) (Debian)

*NOTE: At this time, Raspberry Pi 1 models have issue running NodeJS, due to wrong architecture identification (ARMv7 instead of ARMv6). This guide works properly on Raspberry Pi 2 (refence [1](http://raspberrypi.stackexchange.com/a/30068/32403) [2](https://github.com/v8/v8-git-mirror/blob/master/src/base/cpu.cc#L483-L499)).*

1. Install the latest version of NodeJS and npm, as stated in the official [guide](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#debian-and-ubuntu-based-linux-distributions):

        curl --silent --location https://deb.nodesource.com/setup_0.12 | sudo bash -
        sudo apt-get install --yes nodejs
    
2. Install PC/SC and libnfc (references: [nfc-tools](http://nfc-tools.org/index.php?title=Libnfc#Debian_.2F_Ubuntu), [libnfc](https://github.com/nfc-tools/libnfc)):

        sudo apt-get install pcscd libusb-dev libpcsclite1 libpcsclite-dev dh-autoreconf
    
        cd /opt/
        sudo wget https://github.com/nfc-tools/libnfc/archive/libnfc-1.7.1.zip
        sudo unzip libnfc-1.7.1.zip
        cd libnfc-libnfc-1.7.1/
        sudo autoreconf -vis
        sudo ./configure --with-drivers=all
        sudo make
        sudo make install

    Additionaly, you may need to grant permissions to your user to drive the device.
    Under GNU/Linux systems, if you use `udev`, you could use the provided `udev` rules.
        e.g. under Debian: `sudo cp /opt/libnfc-libnfc-1.7.1/contrib/udev/42-pn53x.rules /lib/udev/rules.d/`
    
3. Make sure the NFC reader is properly recognized:
    
        sudo nfc-list
        
    1. To fix: `error while loading shared libraries: libnfc.so.4: cannot open shared object file: No such file or directory` ([reference](http://seckev.blog.com/2013/04/17/installation-mfterm-with-acr122u-on-kali-linux-system/))

            echo '/usr/local/lib' | sudo tee -a /etc/ld.so.conf.d/usr-local-lib.conf && sudo ldconfig

    2. If you have kernel version > 3.5, probably `pcscd` and also `nfc-list` will report this error: `Unable to claim USB interface (Device or resource busy)` due to the automatic load of `pn533` driver.

        To read the `pcscd` dameon output you can run it using: `pcscd -f -d`

        1. Check which kernel version is installed: `uname -a`
        2. Blacklist `pn533` and `nfc` drivers (references: [Arch Linux wiki Touchatag RFID Reader](https://wiki.archlinux.org/index.php/Touchatag_RFID_Reader), [nfc-tools forum](http://forums.nfc-tools.org/post/5308/#p5308)):

                sudo nano /etc/modprobe.d/blacklist-libnfc.conf

            Add the following lines:

                blacklist pn533
                blacklist nfc

        3. Disable kernel modules:

                modprobe -r pn533 nfc

        4. Restart the `pcscd` daemon: `sudo service pcscd restart`

## Install

1. `git clone https://github.com/goodotcom/rfid-reader-http.git`
2. `cd rfid-reader-http`
3. `npm install`

## Usage

* Run as process: `node index.js`
* Run in background (detached output): `nohup node index.js &`
