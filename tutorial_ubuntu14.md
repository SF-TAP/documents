# Ubuntu 14.04.01

## Install Dependencies

install build-essential, cmake, git, libevent-dev, libboost-all-dev, libpcap-dev

    $ sudo apt-get install build-essential cmake git libevent-dev libboost-all-dev libpcap-dev

## Build and Run Flow Abstractor

### Get Source

clone flow-abstractor from GitHub

    $ git clone git clone https://github.com/stap-project/flow-abstractor.git

### Build

build by cmake and make

    $ cd flow-abstractor
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

if you got an eorror regarding language locale, install suitable launguage pack

    $ sudo apt-get install language-pack-ja

### Run

run specifying a network interface and config file

    $ sudo ./src/stap_fabs -i eth0 -c ./examples/fabs.conf

## Build and Run Protocol Parser

### Get Source

clone protocol-parser from GitHub

    $ git clone https://github.com/stap-project/protocol-parser.git

### Build

build by cmake and make

    $ cd protocol-parser/dns
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

### Run

    $ sudo ./stap_dns
