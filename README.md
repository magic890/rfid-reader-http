# rfid-reader-http

PC/SC lite HTTP wrapper for reading from a remote card reader (RFID &amp; NFC).

## Prerequisites

For running the installation step, due to its dependency from [node-pcsclite](https://github.com/santigimeno/node-pcsclite), this project requires [node-gyp](https://github.com/TooTallNate/node-gyp), so please read carefully and satisfy their [installation requirements](https://github.com/TooTallNate/node-gyp#installation), otherwise it may fails compiling the node module.

### Requirements installation

#### Raspberry Pi (ARM) (Debian)

1. Install the latest version of NodeJS and npm, as stated in the official [guide](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#debian-and-ubuntu-based-linux-distributions):

        curl --silent --location https://deb.nodesource.com/setup_0.12 | sudo bash -
        sudo apt-get install --yes nodejs
    
2. Install PC/SC and libnfc ([official guide](http://nfc-tools.org/index.php?title=Libnfc#Debian_.2F_Ubuntu)):

        sudo apt-get install libusb-dev libpcsclite-dev
    
        cd /opt/
        sudo wget https://libnfc.googlecode.com/files/libnfc-1.7.0-rc7.tar.gz
        sudo tar -xvzf libnfc-1.7.0-rc7.tar.gz &&
        cd libnfc-1.7.0-rc7.tar.gz
        sudo ./configure
        sudo make
        sudo make install
    
    1. Make sure the NFC reader is properly recognized:
        
            sudo nfc-list
            
        1. To fix: `error while loading shared libraries: libnfc.so.4: cannot open shared object file: No such file or directory` ([reference](http://seckev.blog.com/2013/04/17/installation-mfterm-with-acr122u-on-kali-linux-system/))

                echo '/usr/local/lib' | sudo tee -a /etc/ld.so.conf.d/usr-local-lib.conf && ldconfig
    

## Install

* `npm install`

## Usage

* `node index.js`
