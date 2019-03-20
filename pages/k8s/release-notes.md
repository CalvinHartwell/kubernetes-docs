---
wrapper_template: "base_docs.html"
markdown_includes:
  nav: "shared/_side-navigation.md"
context:
  title: "Release notes"
  description: Release notes for CDK
keywords: kubernetes,  release, notes
tags: [news]
sidebar: k8smain-sidebar
permalink: release-notes.html
layout: [base, ubuntu-com]
toc: False
---

# 1.14

### April xx, 2019

 [canonical-kubernetes-xxx][bundle]

## What's new

- Tigera Secure EE support

**CDK** extends its support for CNI solutions by adding the option of using
[**Tigera Secure EE**][tigera-home], the enterprise-ready alternative to Calico. Users are now able
to deploy **CDK** with **Tigera Secure EE** installed and subsequently configure additional
features such as ElasticSearch and the CNX secure connectivity manager. For further
details, please see the [**CDK** CNI documentation][tigera-docs]

- Additional options for High Availability

Version 1.13 of **CDK** introduced support for **keepalived** to provide HA for the 
api-loadbalancer. This new release adds support for both **HAcluster** and **MetalLB**. See 
the relevant [HAcluster][hacluster-docs] and [MetalLB][metallb-docs] pages in the
documentation, as well as the [HA overview][haoverview] for more information. 

- Added CoreDNS support

All new deployments of **CDK** with Kubernetes 1.14 will install **CoreDNS** by
default instead of **KubeDNS**.

Existing deployments that are upgraded to **CDK** and Kubernetes 1.14 will
continue to use **KubeDNS** until the operator chooses to upgrade to
**CoreDNS**. To upgrade, set the new dns-provider config:

```bash
juju config kubernetes-master dns-provider=core-dns
```

Please be aware that changing DNS providers will momentarily interrupt DNS
availability within the cluster. It is not necessary to recreate pods after the
upgrade completes.

The enable-kube-dns option has been removed to avoid confusion. The new
dns-provider config allows you to enable or disable **KubeDNS** as needed.

For more information on the new dns-provider config, see the
[dns-provider config description][dns-provider-config].

- Notable feature xxxxx xx

xxxx xxx xxxx xxx xxxx xx xxx xxxx xxx xx xxx xxxx xxxx xxxx xxx xx xxxx xxxx xxxxxx xx
xxxxxx xxx xxx xx xxxx xxxx  [documentation][doc-link].

## Fixes

- Fixed xxxx xxx xx xx xxx xxxxxxx xxxx ([Issue](https://bugs.launchpad.net/charmed-kubernetes/))
- Fixed xxxx xxx xxxxx xx xx xxxxx xxxx ([Issue](https://bugs.launchpad.net/charmed-kubernetes/))
- Fixed xx x xxx x xx xxx xxx xxxx xx xx ([Issue](https://bugs.launchpad.net/charmed-kubernetes/))

## Previous releases

Please see [this page][historic] for release notes of earlier versions.

<!--LINKS-->
[bundle]: https://api.jujucharms.com/charmstore/v5/canonical-kubernetes-xxx/archive/bundle.yaml
[historic]: /kubernetes/docs/release-notes-historic
[tigera-home]: https://www.tigera.io/tigera-secure-ee/
[tigera-docs]: /kubernetes/docs/cni-tigera
[haoverview]:
[metallb-docs]:
[hacluster-docs]:
[dns-provider-config]: https://github.com/juju-solutions/kubernetes/blob/5f4868af82705a0636680a38d7f3ea760d35dadb/cluster/juju/layers/kubernetes-master/config.yaml#L58-L67
