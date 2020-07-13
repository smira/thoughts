# Networkd

## Initial Network Discovery

1. we can have static interface set up with the kernel (use it, proceed)
2. no kernel config, we should dhcp on all the 'has carrier' interfaces until we get an address (and wait for dhcp)

## Setup Network

After config was downloaded, we should work only on interfaces with explicit config.

We can consider points  1 & 2 from the above.

## Carrier Signal

Why do we even have to detect carrier signal? So that we know when to dhcp on those interfaces?

## Global Actions

* `/etc/resolv.conf`
* `/etc/hostname`

Should this run each time we have config changes?

## Design

`networkd` discovers all the interfaces (it might skip `bondX` without config as it is today).

For each interface it starts a goroutine which does the following:
   * if static, configure it right away
   * if DHCP: waits for the carrier, does dhcp requests, waits for the response, etc.

It's kind of a tree with root at networkd and going through each interface

We could have a context propagation through the tree so that we can cancel the operation

And we could have had kind of a status endpoint for initial discovery which tells us
when we have 1 link up & configured (for initial discovery phase, so that we know we can proceed).

This way I guess we don't need filtering, as unresponsive interfaces or interfaces
without carrier won't affect the operations: we're waiting for the first interface to be fully up.
