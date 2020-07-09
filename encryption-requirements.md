# Encryption Requirements

* Each node should have disks encrypted with some key:
  * *System* disk should be encrypted
  * Should the user disk be encrypted (probably yes!)
* OOB recovery key should be available: ability to unlock encrypted disk in case automatic way doesn't work
  * OOB key is stored offline by the user
  * OOB key is important mostly for control-plane nodes
* Keys should support rotation (not deep master key rotation, but next-level disk encryption key rotation)
* On node/disk eviction, there should be a way to "erase" the key in the key escrow so that evicted node contents can't be decrypted anymore
