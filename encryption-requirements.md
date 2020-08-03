# Encryption Requirements

## Encryption with Throw Away Keys

* All the data on the node is encrypted at rest (system partition, user disks). [should the `/boot` partition be encrypted? probably not, but we need to make sure that partition doesn't contain any sensitive data.]
* Encryption is performed at boot time with throw away key (key is not stored anywhere, it only stays in kernel memory for the duration of the machine lifetime).
* On reboot/power off, machine without access to the keys does effectively full wipe each time by picking up new encryption key.

## Long-term Vision

* Each node should have disks encrypted with some key:
  * *System* disk should be encrypted
  * Should the user disk(s) be encrypted? (probably yes!)
* OOB recovery key should be available: ability to unlock encrypted disk in case automatic way doesn't work
  * OOB key is stored offline by the user
  * OOB key is important mostly for control-plane nodes
* Keys should support rotation (not deep master key rotation, but next-level disk encryption key rotation)
* On node/disk eviction, there should be a way to "erase" the key in the key escrow so that evicted node contents can't be decrypted anymore
