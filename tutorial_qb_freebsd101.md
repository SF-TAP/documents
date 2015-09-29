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

Here, suppose that we have a following FreeBSD box, which has two 10 GbE (ix0 and ix1) and four 1 GbE (igb0, igb1, igb2 and igb3) interfaces.
-r is a prefix of "RIGHT", and -t is a prefix of "TAP".

![qb01 qb01](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb01.png)

### Flow Based Separating

We can use qb-separator for traffic separating as follows.

    # ./qb-separator -r ix0 -t igb0,igb1,igb2,igb3

![qb02 qb02](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb02.png)

Here, qb-separator captures traffic from ix0, then separates and forwards it to
igb[0-3].
Note that it separates traffic by using the hash values of
IP addresses and port numbers of captured packets.

### Mirroring

We can also use qb-tap for traffic mirroring as follows.

    # ./qb-tap -r ix0 -t igb0,igb1,igb2,igb3

![qb03 qb03](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb03.png)

Here, qb-tap forwards all captured traffic from ix0 to igb[0-3].

### L2 Bridging

We can use qb-separator or qb-tap as a simple software L2 bridge as follows.

    # ./qb-separator -r ix0 -l ix1

-l is a prefix of "LEFT".

![qb04 qb04](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb04.png)

### L2 Bridging + Flow Based Separating

We use qb-separator as a L2 bridge and traffic separator as follows.

    # ./qb-separator -r ix0 -l ix1 -t igb0,igb1,igb2,igb3

![qb05 qb05](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb05.png)

Here, qb-separator separates all traffic from ix0 and ix1
and forwards it to igb[0-3].


### L2 Bridging + Mirroring

Similarly, qb-tap can work as a L2 bridge and traffic mirroring box as follows.

    # ./qb-tap -r ix0 -l ix1 -t igb0,igb1,igb2,igb3

![qb06 qb06](https://raw.githubusercontent.com/SF-TAP/documents/master/pict/qb06.png)

Here, all traffic caputured from ix0 and ix1 is forwarded to igb[0-3].
