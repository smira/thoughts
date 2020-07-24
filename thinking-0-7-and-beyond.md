Thinking 0.7 and beyond
=======================

```mermaid!
graph LR
    qemu-->ARM
    qemu-->arges[arges testing]
    qemu-->bootloader[bootloader testing]
    bootloader-->GRUB
    GRUB-->ARM
    GRUB-->boot-ext[ /boot on ext2/4 or even xfs?]
    boot-ext-->stability{stability}
    tiny-dhcpd-->qemu
    tiny-dhcpd.->networkd-testing[test networkd with DHCP]
    libcryptsetup-Go[libcryptsetup, Go, static].->basic-encryption[basic disk encryption]
    basic-encryption-->encryption-throwaway[encryption with throw away keys]
    basic-encryption-->encryption-key-escrow[disk encryption with external key escrow]
    node-trust[node certs, cross-node trust?]-->encryption-key-escrow
    node-trust-->secretless-config[config w/o secrets]-->external-secrets[external secrets provider]
    node-trust-->apid-proxy[apid proxy w/o client config]

```

Question:

* is DHCP even possible with firecracker/qemu? (as it has to be in sync)
