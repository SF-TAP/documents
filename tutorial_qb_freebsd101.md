# Install Cell Incubator on FreeBSD 10.1, and Cooperate with Flow Abstractor

## Network Topology

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

## Build and Run Cell Incubator

