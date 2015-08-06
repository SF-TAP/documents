# Install Cell Incubator on FreeBSD 10.1, and Cooperate with Flow Abstractor

## Build netmap Enabled Kernel

edit kernel config

    # cd /usr/src/sys/amd64/conf
    # cp GENERIC GENERIC.netmap
    # vi GENERIC.netmap
    
    device netmap # add device of netmap

compile and install

    # cd /usr/src
    # make -j 30 buildkernel KERNCONF=GENERIC.netmap
    # make installkernel KERNCONF=GENERIC.netmap

reboot

    # reboot

confirm

    # ls /dev/netmap
    /dev/netmap

## Install Dependencies

install git

    # pkg install git-2.3.5

## Build Cell Incubator

clone cell incubator form GitHUB

    $ git clone https://github.com/SF-TAP/sf-incubator.git

build

    $ cd sf-incubator/src
    $ make

## Run Cell Incubator

Before running, disable offload engine of NICs. The shell script included by the repository automatically disable offload engine of all NICs.

    $ sudo ./misc/ifcap_disable.sh

Here, suppose that we have a following FreeBSD box.

![qb01 qb01](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb01.png)

### Flow Based Separating

![qb02 qb02](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb02.png)

### Mirroring

![qb03 qb03](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb03.png)

### L2 Bridging

![qb04 qb04](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb04.png)

### L2 Bridging + Flow Based Separating

![qb05 qb05](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb05.png)

### L2 Bridging + Mirroring

![qb06 qb06](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb06.png)
