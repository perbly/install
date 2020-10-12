Install Onify Hub in Linux
==========================

Installing Onify Hub on a single Linux machine requires [Microk8s](https://microk8s.io/).

# Preparations

1. Install Microk8s
2. [Setup Onify using Kubernetes](https://github.com/onify/install/tree/default/hub/kubernetes)

# Install MicroK8s

## Install

```bash
snap install microk8s --classic
```

## Configure

### Enable DNS

```bash
microk8s.enable dns
```

> If you have issues with k8s default DNS (8.8.8.8 and 8.8.4.4), you need to change this, see `https://microk8s.io/docs/addon-dns`.

### Set kubetcl alias

```bash
snap alias microk8s.kubectl kubectl
```
