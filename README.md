# SF-TAP: Scalable and Flexible Traffic Analysis Platform

## What?

SF-TAP is a platform for L7-level network traffic analysis.
It can deal with high-bandwidth network traffic becase of the scalable
architecture.
Furthermore, SF-TAP allows developers to easily implement
L7-level traffic analyzers because of some abstractions.
SF-TAP consists of 2 program, which are Flow Abstractor and Cell Incubator.

### Flow Abstractor

Flow Abstractor abstracts network traffic as files by UNIX domain socket.
It captures network traffic via a NIC by pcap,
reassembles fragmented IP packets and TCP packets,
and classifies of flows by using regular expressions for L7 protocol detection.
Classified flows are outputted to the files of UNIX domain socket.
Accordingly, developers can implement L7-level analyzers by accessing the files.

### Cell Incubator

Cell Incubator is a software-based network flow separator.
It captures network traffic via a NIC by netmap, separates and forwards flows to
other multiple NICs for high-bandwidth L7-level network traffic analysis.
Accordingly, analysis can be performed on multiple servers,
which captures flows forwarded by Cell incubator.

## Installation and Tutorial

1. [Install Flow Abstractor on Ubuntu 14.10, and Store and Visualize Data](https://github.com/SF-TAP/documents/blob/master/tutorial_fabs_ubuntu1410.md)

## Configuration of Flow Abstractor

not yet

## Write Protocol Parsers

not yet
