# Ubuntu 14.04.01

## Install Dependencies

install build-essential, cmake, git, libevent-dev, libboost-all-dev, libpcap-dev

    $ sudo apt-get install build-essential cmake git libevent-dev libboost-all-dev libpcap-dev

## Build and Run Flow Abstractor

### Get Source

clone flow-abstractor from GitHub

    $ git clone https://github.com/SF-TAP/flow-abstractor.git

### Build

build by cmake and make

    $ cd flow-abstractor
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

if you got an eorror regarding language locale, install suitable launguage pack

    $ sudo apt-get install language-pack-ja

### Run

run specifying a network interface and config file

    $ sudo ./src/sf-tap_fabs -i eth0 -c ./examples/fabs.conf

## Build and Run Protocol Parser

### Get Source

clone protocol-parser from GitHub

    $ git clone https://github.com/SF-TAP/protocol-parser.git

### Build and Run DNS Parser

build by cmake and make

    $ cd protocol-parser/dns
    $ cmake -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
    $ make

and run.

    $ sudo ./sf-tap_dns -j

### Build and Run HTTP Parser

install dependencies

    $ sudo apt-get install leiningen maven
    $ cd protocol-parser/javaclass
    $ ./install.sh
    $ sudo mkdir -p /opt/newsclub/lib-native
    $ sudo cp linux/libjunixsocket-linux-1.5-amd64.so /opt/newsclub/lib-native

and run.

    $ cd protocol-parser/http
    $ lein deps
    $ lein uberjar
    $ java -jar ./target/uberjar/http-0.1.0-sftap-standalone.jar /tmp/sf-tap/tcp/http

## Save Result into MongoDB

install dependencies

    $ sudo apt-get install mongodb nodejs
    $ cd protocol-parser/mongostore
    $ npm install mongodb

run HTTP parser, and redirect to mongostore

    $ cd protocol-parser/http
    $ lein run | node ../mongostore/mongostore.js test_db http

show result

    $ mongo
    > use test_db
    > db.http.find()

of course, DNS parser's output can be stored into MongoDB

    $ cd protocol-parser/dns
    $ ./sf-tap_dns -j | node ../mongostore/mongostore.js test_db dns

## Save Result into Redis

build redistore

    $ cd protocol-parser/redistore
    $ lein deps
    $ lein uberjar

run HTTP parser, and redirect to redistore

    $ java -jar ./target/uberjar/http-0.1.0-sftap-standalone.jar /tmp/sf-tap/tcp/http | java -jar ../http/target/uberjar/http-0.1.0-sftap-standalone.jar /tmp/sf-tap/tcp/http|java -jar ./target/uberjar/redistore-0.1.0-sftap-standalone.jar http

show result

    $ redis-cli
    127.0.0.1:6379> lrange "http" 0 -1