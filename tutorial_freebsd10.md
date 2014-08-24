# FreeBSD 10.0

## Install Dependencies

install cmake, boost-all, git, libevent2

    $ pkg install cmake boost-all git libevent2

## Build and Run Flow Abstractor

### Get Source

clone flow-abstractor from GitHub

    $ git clone git clone https://github.com/SF-TAP/flow-abstractor.git

### Build

set environment variable

    $ setenv LIBRARY_PATH /usr/local/lib:/usr/lib
    $ setenv CPLUS_INCLUDE_PATH /usr/local/include:/usr/include

build by cmake and make

    $ cd flow-abstractor
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

### Run

run specifying a network interface and config file

    $ sudo ./src/sf-tap_fabs -i eth0 -c ./examples/fabs.conf

## Build and Run Protocol Parser

### Get Source

clone protocol-parser from GitHub

    $ git clone https://github.com/sf-tap-project/protocol-parser.git

### Build

build by cmake and make

    $ cd protocol-parser/dns
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

### Run

    $ sudo ./sf-tap_dns
