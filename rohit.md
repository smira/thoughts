# Onboarding
- email @siderolabs.com
- GitHub organization access
- KnowledgeBase access, Obsidian, Syncthing
- GPG Key (temporary use local `gpg`, get YubiKey)

# Tasks
No deadline, no rush, just a list of stuff to work on.

## Discovery Service

- [ ] [public debug page](https://github.com/talos-systems/discovery-service/issues/28)
- [ ] [extract peer address from nginx ingress headers](https://github.com/talos-systems/discovery-service/issues/27)
- [ ] (a bit bigger one, feel free to skip) [ratelimiting requests by IP address](https://github.com/talos-systems/discovery-service/issues/9)

## ci-bot
- [ ] set up a test environment for test bot: run it, expose its endpoint, set up GitHub webhook
- [ ] refactor the code to make it simple and easy to read (e.g. try to remove event-to-command interaction via args, use regular Go calls)
- [ ] [remove /lgtm command lgtm status](https://github.com/talos-systems/ci-bot/issues/29)
- [ ] [rename /approve to /ok-to-test](https://github.com/talos-systems/ci-bot/issues/30)
- [ ] [smart /merge command](https://github.com/talos-systems/ci-bot/issues/31)


## Kres 
- [ ] [switch to some better CLI library](https://github.com/talos-systems/kres/issues/10)
- [ ] [add suport for `make conformance`](https://github.com/talos-systems/kres/issues/50)
- [ ] [support different "trunk" branches](https://github.com/talos-systems/kres/issues/84)
- [ ] [Format generated code](https://github.com/talos-systems/kres/issues/87)
- [ ] [Support for debug and race builds](https://github.com/talos-systems/kres/issues/88)

## conform

- [ ] [ Imperative mood can ignore prefacing emoji](https://github.com/talos-systems/conform/issues/212)
- [ ] make 0.1.0-beta release (see [PR](https://github.com/talos-systems/conform/pull/211)), ask for help on how to do releases (it's easy!)

# Talos
## Building
Checkout Talos repository.

Try to follow `make help`.  You need Docker and buildx installed on the host. 

> Note: On Ubuntu 21.10 it's easier to use Ubuntu's `docker`, for earlier versions of Ubuntu use Docker's apt repositories.

> Note: `buildx` is not available with Ubuntu packages, it should be installed as a plugin from GitHub releases.

Set up a builder with access to host network:

```bash
 docker buildx create --driver docker-container  --driver-opt network=host --name local1 --buildkitd-flags '--allow-insecure-entitlement security.insecure' --use
```
 
 > Note: `network=host` allows buildx builder to access host network, so that it can push to local repository (see below).
 
Make sure the following steps work:
- `make talosctl`
- `make initramfs kernel`

Set up a local docker registry:

```bash
docker run -d -p 5005:5005 \
	--restart always \
	--name local registry:2
```

Try to build and push to local registry an installer image:

```bash
make installer IMAGE_REGISTRY=127.0.0.1:5005 PUSH=true
```

Record the image name output in the step above ^^^.

## Running Talos cluster

Set up local caching docker registries (this speeds up Talos cluster boot a lot), script is in the Talos repo:

```bash
bash hack/start-registry-proxies.sh
```

Start your local cluster with:

```bash
sudo -E _out/talosctl-linux-amd64 cluster create \
	--provisioner=qemu \
	--cidr=172.20.0.0/24 \
	--registry-mirror docker.io=http://172.20.0.1:5000 \ 
	--registry-mirror k8s.gcr.io=http://172.20.0.1:5001  \
	--registry-mirror quay.io=http://172.20.0.1:5002 \
	--registry-mirror gcr.io=http://172.20.0.1:5003 \
	--registry-mirror ghcr.io=http://172.20.0.1:5004 \
	--registry-mirror 127.0.0.1:5005=http://172.20.0.1:5005 \
	--install-image=127.0.0.1:5005/talos-systems/installer:<RECORDED HASH from the build step> \
	--masters 3 \
	--workers 2 \
	--with-bootloader=false
```

- `--provisioner` selects QEMU vs. default Docker
- custom `--cidr` to make things easier, but this is subjective
- `--registry-mirror` use the caching proxies set up above to speed up boot time a lot, last one adds your local registry (installer image was pushed to it)
- `--install-image` is the image you built with `make installer` above
- `--masters` & `--workers` configure cluster size, choose to match your resources; 3 masters give you HA control plane; 1 master is enough, never do 2 masters
- `--with-bootloader=false` disables boot from disk (Talos will always boot from `_out/vmlinuz-amd64` and `_out/initramfs-amd64.xz`). This speeds up development cycle a lot - no need to rebuild installer and perform install, rebooting is enough to get new code.

> Note: as boot loader is not used, it's not necessary to  rebuild `installer` each time (old image is fine), but sometimes it's needed (when configuration changes are done and old installer doesn't validate the config).

> Note: `talosctl cluster create` derives Talos machine configuration version from the install image tag, so sometimes early in the development cycle (when new minor version is not released yet), machine config version can be overridden with `--talos-version=v0.14`.

## Console Logs

Watching console logs is easy with `tail`:

```bash
tail -F ~/.talos/clusters/talos-default/talos-default-*.log
```

## Interacting with Talos

Once `talosctl cluster create` finishes successfully, `talosconfig` and `kubeconfig` will be set up automatically to point to your cluster.

Start playing with `talosctl`:

```bash
talosctl -n 172.20.0.2 version
talosctl -n 172.20.0.3,172.20.0.4 dashboard
talosctl -n 172.20.0.4 get members
```

Same with `kubectl`:

```bash
kubectl get nodes -o wide
```

You can deploy some Kubernetes workloads to the cluster.

You can edit machine config on the fly with `talosctl edit mc --immediate`, config patches can be applied via `--config-patch` flags, also many features have specific flags in `talosctl cluster create`.

## Quick Reboot

To reboot whole cluster quickly (e.g. to pick up a change made in the code):

```bash
for socket in ~/.talos/clusters/talos-default/talos-default-*.monitor; echo "q" | sudo socat - unix-connect:$socket; end
```

Sending `q` to a single socket allows to reboot a single node.

## Development Cycle

Bring up a cluster, make code changes, rebuild `initramfs` with `make initramfs`, reboot a node to pick new `initramfs` with the reboot command above (or regular `talosctl reboot`).

Some aspects of Talos development require to enable bootloader (when working on `installer` itself), in that case quick development cycle is no longer possible, and cluster should be destroyed and recreated each time.

## Running Integration Tests

If integration tests were changed (or when running them for the first time), first rebuild the integration test binary:

```bash
rm -f  _out/integration-test-linux-amd64; make _out/integration-test-linux-amd64
```

Running short tests against QEMU provisioned cluster:

```bash
_out/integration-test-linux-amd64 \
  -talos.provisioner=qemu \
  -test.v \
  -talos.crashdump=false \
  -test.short \
  -talos.talosctlpath=$PWD/_out/talosctl-linux-amd64
```

Whole test suite can be run removing `-test.short` flag.

Specfic tests can be run with `-test.run=TestIntegration/api.ResetSuite`.

## Build Flavors

`make <something> WITH_RACE=1` enables Go race detector, Talos runs slower and uses more memory, but memory races are detected.

`make <something> WITH_DEBUG=1` enables Go profiling and other debug features, useful for local development.

## Destroying Custer

```bash
sudo -E ../talos/_out/talosctl-linux-amd64 cluster destroy --provisioner=qemu
```

## Optional

Set up cross-build environment with:

```bash
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```

> Note: the static qemu binaries which come with Ubuntu 21.10 seem to be broken.

## Unit tests

Unit tests can be run in buildx with `make unit-tests`, on Ubuntu systems some tests using `loop` devices will fail because Ubuntu uses low-index `loop` devices for snaps.

Most of the unit-tests can be run standalone as well, with regular `go test`, or with IDE integration. This provides much faster feedback loop