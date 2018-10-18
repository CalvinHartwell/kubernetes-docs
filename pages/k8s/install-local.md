---
title: "Logging | Canonical Distribution of Kubernetes&reg;"
keywords: lxd, conjure-up, requirements,developer
tags: [install]
sidebar: k8smain-sidebar
permalink: install/local.html
layout: base
toc: False
---

# Installing to a Local machine

Installing the **Canonical Distribution of Kubernetes<sup>&reg;</sup> (CDK)**
on a single  machine is possible for the purposes of testing and development.

Be aware that the full deployment of **CDK** has system requirements which may
exceed a standard laptop or desktop machine. It is only recommended for a
machine with 32GB RAM and 250GB of SSD storage.

<div class="p-notification--positive"><p class="p-notification__response">
<span class="p-notification__status">Note:</span>
If you don't meet these requirements or want a lightweight way to develop on
pure Kubernetes, we recommend  <a href="https://microk8s.io/">microk8s</a>
</p></div>

The main requirement for a local install is a local cloud to run **CDK** on. This is achieved by using lightweight containers managed by [LXD][lxd-home]. **LXD**  version 3.0 or better is required for the default installer, and there are some specific profile requirements as detailed below.

## Ubuntu Server

 1. Install **LXD**
     ```bash
     sudo snap install lxd
     ```
 1. Run the **LXD** init script
      ```bash
      /snap/bin/lxd init
      ```
 1. The init script itself can vary depending on the version of LXD. The important configuration options for the installer are:
    - Networking: Do **NOT** enable ipv6 networking on the bridge interface
    - Storage Pool: It is recommended to use the 'dir' storage type

  1. You can now proceed using the main installer. Follow the [install instructions][quickstart] and choose `localhost`  in the `CHOOSE A CLOUD` step.

## Ubuntu Desktop

1. Install **LXD**
    ```bash
    sudo snap install lxd
    ```

1. In order to access the **LXD** service your user will need to be apart of the `lxd` group. Run the following:
    ```bash
    sudo usermod -a -G lxd $USER
    newgrp lxd
    ```

1. Run the **LXD** init script
     ```bash
     /snap/bin/lxd init
     ```

1. The init script itself can vary depending on the version of LXD. The important configuration options for the installer are:
   - Networking: Do **NOT** enable ipv6 networking on the bridge interface
   - Storage Pool: It is recommended to use the 'dir' storage type

1. You can now proceed using the main installer. Follow the [install instructions][quickstart] and choose `localhost`  in the `CHOOSE A CLOUD` step.

## Other  platforms (ArchLinux, Debian, Fedora, OpenSUSE )

1. Install **LXD** using the [instructions for your OS][lxd-install]

1. Install Conjure up using the [instructions for your OS][conjure-up-install]  

1. Proceed with the relevant steps from the [install instructions][quickstart] and choose `localhost`  in the `CHOOSE A CLOUD` step.

<!-- LINKS -->

[lxd-home]: https://linuxcontainers.org/
[lxd-install]: https://linuxcontainers.org/lxd/getting-started-cli/
[conjure-up-install]: https://docs.conjure-up.io/devel/en/user-manual#installing-conjure-up
[quickstart]: /quickstart.html
