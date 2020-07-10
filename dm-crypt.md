# dm-crypt

Partition is encrypted as a whole, without any specific headers.
Talos should supply encryption parameters (cipher, key, etc.) to mount encrypted partition.

## Pros

* if some part of the volume is damaged, it only affects part of the volume;
* Talos could interact without userland help directly with the kernel probably

## Cons

* if we need multiple keys to unlock master key, it should be implemented in Talos
