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

### Build and Run DNS Parser

build by cmake and make

    $ cd protocol-parser/dns
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

and run.

    $ sudo ./stap_dns -j

### Build and Run HTTP Parser

install dependencies

    $ sudo apt-get install leiningen maven
    $ cd protocol-parser/javaclass
    $ ./install.sh
    $ sudo mkdir -p /opt/newsclub/lib-native
    $ sudo cp linux/libjunixsocket-linux-1.5-amd64.so /opt/newsclub/lib-native

and run by using leiningen.

    $ cd protocol-parser/http
    $ lein deps
    $ lein run [path_to_unix_domain_socket]
