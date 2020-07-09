# Age encryption

We could use [age](https://pkg.go.dev/filippo.io/age@v1.0.0-beta4?tab=doc) as Go module to implement key
encryption by control plane nodes.

Each control plane node generates its X25519 identity and publishes public key.

Node downloads list of control plane public identities, encrypts its key with all the control plane nodes
as identities and stores encrypted key locally (e.g. in `/boot` partition):

```Go
w, _  := age.Encrypt(&buf, controlPlaneIdentities...)
w.Write(DEK)
w.Close()
```

On next boot, node reaches out to any of control plane nodes with request to decrypt its key (providing
the encrypted key). Any control plane node can decrypt the key and return decrypted version:

```Go
r, _ := age.Decrypt(bytes.NewReader(encryptedDek), nodeIdentity)
DEK := ioutil.ReadAll(r)
```

Node authentication is out of scope for this document, please see [[trustd-communication]].
