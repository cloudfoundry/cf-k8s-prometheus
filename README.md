# cf-k8s-prometheus

This repo contains Prometheus components built with an Ubuntu base image (i.e.
Prometheus server, statsd_exporter, etc) that can be deployed in a [CF for
K8s](https://github.com/cloudfoundry/cf-for-k8s) environment.

### How to Deploy in CF for K8s

When deploying the CF for K8s cluster via the [deploy docs](https://github.com/cloudfoundry/cf-for-k8s/blob/master/docs/deploy.md),
include `config/0-prometheus-server.yml` as shown below:

```
ytt -f config -f ${WORKSPACE}/cf-k8s-prometheus/config/0-prometheus-server.yml -f ${TMP_DIR}/cf-values.yml > ${TMP_DIR}/cf-for-k8s-rendered.yml
```
