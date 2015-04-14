# Install Flow Abstractor on Ubuntu 14.10, and Store and Visualize Data

## Install Dependencies

install build-essential, cmake, git, libevent-dev, libboost-all-dev, libpcap-dev

    $ sudo apt-get install build-essential cmake git libevent-dev libboost-all-dev libpcap-dev libre2-dev

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

    $ sudo ./src/sftap_fabs -i eth0 -c ./examples/fabs.conf

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

    $ sudo ./sftap_dns

now, you can capture DNS packets in JSON. type this and confirm outputs

    $ dig www.github.com

DNS parser will output like as follows

```json
{
    "src": {
        "ip":"192.168.163.131",
        "port":5359
    },
    "dst": {
        "ip":"192.168.163.2",
        "port":53
    },
    "id":36092, "qr":0, "op":0, "aa":0, "tc":0,
    "rd":1, "ra":0, "z":0, "ad":1, "cd":0, "rc":0,
    "query_count":1,
    "answer_count":0,
    "authority_count":0,
    "additional_count":1,
    "query":[
        {
            "name":"www.github.com",
            "type":"A",
            "class":1
        }
    ],
    "answer":[],
    "authority":[],
    "additional":[
        {
            "name":".",
            "type":"OPT",
            "class":4096,
            "ttl":0,
            "rr":""
        }
    ]
}
```

### Run HTTP Parser

run HTTP parser

    $ cd protocol-parser/http
    $ python3 sftap_http.py

then, you can capture HTTP packets in JSON. type this and confirm outputs

    $ wget www.github.com

HTTP parser will output like as follows

```json
{
    "server": {
        "header": {
            "connection":"close",
            "location":"https://www.github.com/",
            "content-length":"0"
        },
        "ip":"192.30.252.128",
        "port":80,
        "response": {
            "msg":"Moved Permanently",
            "ver":"HTTP/1.1",
            "code":"301"
        }
    },
    "client": {
        "header": {
            "user-agent":"Wget/1.15 (linux-gnu)",
            "accept":"*/*",
            "host":"www.github.com",
            "connection":"Keep-Alive"
        },
        "port":40031,
        "method": {
            "uri":"/",
            "method":"GET",
            "ver":"HTTP/1.1"
        },
        "ip":"192.168.163.131"
    }
}
```

## Save Result into MongoDB

install dependencies

    $ sudo apt-get install mongodb python3-pip
    $ sudo pip3 install pymongo

run HTTP parser, and redirect to mongostore

    $ cd protocol-parser/http
    $ sudo python3 sftap_http.py | python3 ../mongostore/mongostore.py -d sftap -c http

access some HTTP servers, and show result

    $ mongo
    > use sftap
    > db.http.find()

of course, outputs of DNS parser also can be stored into MongoDB

    $ cd protocol-parser/dns
    $ sudo ./sftap_dns | python3 ../mongostore/mongostore.py -d sftap -c dns

## Save Result into Elasticsearch and Visualize by Kibana

install dependencies

    $ sudo apt-get install openjdk-8-jdk curl
    $ wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.5.1.deb
    $ sudo dpkg -i elasticsearch-1.5.1.deb
    $ sudo pip3 install elasticsearch

run elasticsearch

    $ sudo service elasticsearch start

store results of HTTP parser into Elasticsearch

    $ cd protocl-parser/http
    $ sudo python3 sftap_http.py | python3 ../esstore/esstore.py -i sftap -t http

download and run Kibana

    $ wget https://download.elastic.co/kibana/kibana/kibana-4.0.2-linux-x64.tar.gz
    $ tar xzfv kibana-4.0.2-linux-x64.tar.gz
    $ cd kibana-4.0.2-linux-x64
    $ ./bin/kibana

update mapping

```
$ curl -XPUT 'http://localhost:9200/sftap/_mapping/http?ignore_conflicts=true' -d '{
  "http" : {
    "properties" : {
      "client" : {
        "properties" : {
          "header" : {
            "properties" : {
              "host" : {
                "type" : "string",
                "index": "analyzed",
                "analyzer" : "simple",
                "fields" : {
                  "raw" : {"type" : "string", "index" : "not_analyzed"}
                }
              },
              "referer" : {
                "type" : "string",
                "index": "analyzed",
                "analyzer" : "simple",
                "fields" : {
                  "raw" : {"type" : "string", "index" : "not_analyzed"}
                }
              }
            }
          },
          "method" : {
            "properties" : {
              "uri" : {
                "type" : "string",
                "index" : "analyzed",
                "analyzer" : "simple",
                "fields" : {
                  "raw" : {"type" : "string", "index" : "not_analyzed"}
                }
              }
            }
          }
        }
      }
    }
  }
}'
```

open http://localhost:5600/ and select an index and a key for timestamp

![kibana01 kibana01](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/kibana01.png)

visualize data like as follows

![kibana02 kibana02](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/kibana02.png)

![kibana03 kibana03](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/kibana03.png)

of course, you can use dashboard

![kibana04 kibana04](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/kibana04.png)
