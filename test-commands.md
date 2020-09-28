# Commands to launch tests

## Local buildx with local registry support

```sh
 docker buildx create --driver docker-container  --driver-opt network=host --name local1 --buildkitd-flags '--allow-insecure-entitlement security.insecure' --use
```

## Create cluster

With [registry mirrors](https://www.talos.dev/docs/v0.6/en/guides/local/registry-cache):

```sh
$ sudo -E _out/talosctl-linux-amd64 cluster create --provisioner=qemu --cidr=172.20.0.0/24 --registry-mirror docker.io=http://172.20.0.1:5000 --registry-mirror k8s.gcr.io=http://172.20.0.1:5001 --registry-mirror quay.io=http://172.20.0.1:5002 --registry-mirror gcr.io=http://172.17.0.4:5000 --install-image=docker.io/autonomy/installer:v0.6.0-alpha.6 --cpus 2 --memory 2048 --masters 3 --workers 1 --with-init-node=false --with-bootloader=false
```

## Integration test (qemu)

Full suite:

```sh
$ _out/integration-test-linux-amd64 -talos.provisioner=qemu -test.v -talos.crashdump=false -talos.talosctlpath=$PWD/_out/talosctl-linux-amd64
```

Only short tests:

```sh
$ _out/integration-test-linux-amd64 -talos.provisioner=qemu -test.v -talos.crashdump=false -test.short -talos.talosctlpath=$PWD/_out/talosctl-linux-amd64
```

Only specific tests:

```sh
 _out/integration-test-linux-amd64 -talos.provisioner=qemu -test.v -talos.crashdump=false  -talos.talosctlpath=$PWD/_out/talosctl-linux-amd64 -test.run=TestIntegration/api.ResetSuite
```


## Provision test

```sh
$ sudo -E _out/integration-test-provision-linux-amd64 -test.v -talos.crashdump=false -talos.provision.registry-mirror docker.io=http://172.21.0.1:5000,k8s.gcr.io=http://172.21.0.1:5001,quay.io=http://172.21.0.1:5002,gcr.io=http://172.21.0.1:5003 -talos.talosctlpath=$PWD/_out/talosctl-linux-amd64
```

##  Sfyra

```sh
(cd ../talos; python3 -m http.server 8000 --bind 172.17.0.1)

(cd ../talos/; sudo -E ../sfyra/_out/integration-test -skip-teardown -registry-mirrors docker.io=http://172.24.0.1:5000,k8s.gcr.io=http://172.24.0.1:5001,quay.io=http://172.24.0.1:5002,gcr.io=http://172.24.0.1:5003 -talos-kernel-url http://172.17.0.1:8000/_out/vmlinuz -talos-initrd-url http://172.17.0.1:8000/_out/initramfs.xz -nodes 4 -test.v)
```
