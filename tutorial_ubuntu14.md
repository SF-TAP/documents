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

    $ sudo ./sftap_dns -j

### Build and Run HTTP Parser

install dependencies

    $ sudo apt-get install python3

and run.

    $ cd protocol-parser/http
    $ python3 sftap_http.py

## Save Result into MongoDB

install dependencies

    $ sudo apt-get install mongodb nodejs
    $ cd protocol-parser/mongostore
    $ npm install mongodb

run HTTP parser, and redirect to mongostore

    $ cd protocol-parser/http
    $ python3 sftap_http.py | node ../mongostore/mongostore.js test_db http

show result

    $ mongo
    > use test_db
    > db.http.find()

of course, DNS parser's output can be stored into MongoDB

    $ cd protocol-parser/dns
    $ ./sftap_dns -j | node ../mongostore/mongostore.js test_db dns

## Save Result into Redis

install dependencies

    $ sudo apt-get install redis-server redis-tools leiningen

build redistore

    $ cd protocol-parser/redistore
    $ lein deps
    $ lein uberjar

run HTTP parser, and redirect to redistore

    $ python3 ../http/sftap_http.py | java -jar ./target/uberjar/redistore-0.1.0-sftap-standalone.jar http

show result

    $ redis-cli
    127.0.0.1:6379> lrange "http" 0 -1
