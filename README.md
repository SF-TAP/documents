# SF-TAP: Scalable and Flexible Traffic Analysis Platform

## References

[SF-TAP: Scalable and Flexible Traffic Analysis Platform Running on Commodity Hardware (USENIX LISA 2015)](https://www.usenix.org/conference/lisa15/conference-program/presentation/takano)

    @inproceedings {193176,
      author = {Yuuki Takano and Ryosuke Miura and Shingo Yasuda and Kunio Akashi and Tomoya Inoue},
      title = "{SF-TAP: Scalable and Flexible Traffic Analysis Platform Running on Commodity Hardware}",
      booktitle = {29th Large Installation System Administration Conference (LISA15)},
      year = {2015},
      month = Nov,
      isbn = {978-1-931971-270},
      address = {Washington, D.C.},
      pages = {25--36},
      url = {https://www.usenix.org/conference/lisa15/conference-program/presentation/takano},
      publisher = {USENIX Association},
    }

## What?

SF-TAP is a platform for L7-level network traffic analysis.
It can deal with high-bandwidth network traffic because of the scalable
architecture.
Furthermore, SF-TAP allows developers to easily implement
L7-level traffic analyzers because of some abstractions.
SF-TAP provides two main components,
which are Flow Abstractor and Cell Incubator.

### Flow Abstractor

Flow Abstractor abstracts network traffic as files by UNIX domain socket.
It captures network traffic via a NIC by pcap,
reassembles fragmented IP packets and TCP packets,
and classifies of flows by using regular expressions for L7 protocol detection.
Classified flows are outputted to the files of UNIX domain socket.
Accordingly, developers can implement L7-level analyzers by accessing the files.

### Cell Incubator

Cell Incubator is a software-based network flow separator.
It captures network traffic from one or two NIC(s),
separates and forwards flows to other multiple NICs
for high-bandwidth L7-level network traffic analysis.
Accordingly, analyses can be performed on multiple servers,
which capture flows forwarded by Cell incubator.

## Installation and Tutorial

1. [Install Flow Abstractor on Ubuntu 15.04, and Store and Visualize Data](https://github.com/SF-TAP/documents/blob/master/tutorial_fabs_ubuntu1504.md)
2. [Install Cell Incubator on FreeBSD 10.1, and Cooperate with Flow Abstractor](https://github.com/SF-TAP/documents/blob/master/tutorial_qb_freebsd101.md)

## Configuration of Flow Abstractor

not yet

## Write Protocol Parsers

not yet
