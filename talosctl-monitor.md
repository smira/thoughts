# Talosctl monitor

 I looked around for good monitor CLI inspiration, one of them is https://github.com/xxxserxxx/gotop.
 In the end they're based on [go-psutil](https://github.com/shirou/gopsutil). So my thought was:

1. add some kind of service for Talos which exposes monitoring API, it uses go-psutil under the hood and exposes results as protobuf API
2. we can take inspiration from gotop or anything like that and simply use API as data source.

So we don't have to do low-level information gathering (could use or take pieces of go-psutil),
we don't do much with the UI go-psutil seems to rely a lot on the framework), and some dead simple version can be done.

Then multiple nodes, aggregation, and monitoring tool becomes immediately super awesome

> Andrew: better stick with domain services.

Alternatives:

* prometheus-exporter
* k8s metrics
