# LUKS

LUKS2 is a more modern format, but there’s no big difference between the formats.
In the end it’s a header with encryption metadata followed by actual encrypted volume (same as dm-crypt plain format).
LUKS supports multiple key slots, so multiple keys which unlock the master key.
On other hand LUKS is more targeted towards end-users with passphrases, hashing, etc.
In automated disk encryption we could have used random N-bit keys directly.

If we decide to go with the cryptsetup approach, we can use Go binding for
[libcryptsetup](https://github.com/martinjungblut/go-cryptsetup) if we can keep it cgo enabled and static build.

## Pros

* out of the box support for multiple keys
* easy to add oob unlock keys

## Cons

* need cryptsetup binary or libcryptsetup during the boot phase;
* risk of losing data if LUKS header is corrupted (but     same with dm-crypt metadata).
